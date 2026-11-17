# Large models

The scene representation that lets Arc Review open, navigate and analyze federations far larger
than memory: scene caches, spatial hierarchy, level of detail, streaming, out-of-core loading,
GPU-driven culling, memory budgets and performance targets. Implemented in `review-scene` with
rendering support in `arc-gpu`; the cache decision is [ADR 0003](../adr/0003-content-addressed-scene-cache.md).

## Scale assumed

A federation of 50 to 200 model sources, tens of millions of scene items, billions of source
triangles before deduplication, point clouds of billions of points, and property stores of hundreds
of millions of values. The design goal is that opening cost and memory use follow what is visible
and what is queried, not what is loaded.

## Scene cache

One scene cache per model source, addressed by content key (source hash, reader version, reader
options). Its content is split into pages of bounded size (target 256 KiB compressed), each
addressed by its own hash, so that disk, packaged documents and Hub serve the same pages.

| Section | Content | Access pattern |
|---|---|---|
| Header | Format version, content key, units, bounds, page directory, statistics | Read on open |
| Item table | Columnar arrays per item: item key, parent, kind, flags, geometry reference, bounds | Read fully for the tree and queries; compact (tens of bytes per item) |
| Property store | Dictionary-encoded strings, typed value columns, per property-set tables, item-to-row index | Paged; columns loaded on demand for queries and inspection |
| Geometry store | Unique meshes deduplicated by hash, split into clusters of up to 128 triangles; quantized positions relative to cluster origin; normals and texture coordinates compressed | Paged by cluster; streamed by residency |
| Level-of-detail chain | Per cluster group, simplified parents built with `arc-mesh` simplification, with geometric error bounds | Streamed coarse to fine |
| Instances | Item to mesh references with transforms (f64 translation, f32 rotation-scale) | Read with the item table |
| Spatial hierarchy | Bounding volume hierarchy over instances and clusters, with per-node error bounds | Read top levels on open, deeper nodes on demand |
| Lines, points, text | Separate primitive streams for drawings, grids and annotations | Paged |
| Point octree | For point cloud sources: octree nodes with subsampled points per level | Streamed like clusters |

Federation level: a top-level hierarchy combines the source hierarchies with their placements, so
culling, picking, clash broad phase and spatial queries see one tree without copying source data.

## Precision

Georeferenced models have coordinates in the millions of meters. Cluster origins and instance
translations are f64 on the CPU; vertex positions are quantized offsets from the cluster origin.
The GPU receives positions relative to the camera, computed per frame from f64 origins, so precision
is constant anywhere in the federation (camera-relative rendering in `arc-gpu`).

## Level of detail

- Cluster groups form a hierarchy in which every parent is a simplified version of its children
  with a known geometric error. Selection picks, per region, the coarsest level whose projected
  error is under a screen-space threshold (default about one pixel at rest).
- Small items are culled by projected size during motion and restored at rest (frame governor).
  Items with visibility state *required* are exempt from size culling and are loaded at a minimum
  level of detail regardless of budget pressure.
- Lines and text use projected-length thresholds; point clouds use octree depth by screen density.

## Streaming and out-of-core loading

- A residency manager keeps a priority queue of pages. Priority combines projected error reduction,
  visibility in the last frames, distance to the camera, membership in the current selection or
  required set, and pending analysis requests.
- Pages are read with memory-mapped files on desktop and with HTTP range requests (or per-page
  requests) from Hub in the browser and in Hub-backed desktop sessions. The browser keeps pages in
  its cache storage keyed by page hash.
- Eviction is least-useful-first within the budget; evicted pages are reloaded transparently.
- Analysis engines use the same pages through a separate reader that bypasses render budgets and
  streams sequentially, so a clash run never competes with interactive residency for GPU memory.
- Opening shows the coarsest levels of all visible sources first, then refines; the first frame
  does not wait for any source to be complete.

## GPU-driven culling and drawing

- Cluster metadata (bounds, error, item index) lives in GPU buffers. A compute pass per frame
  performs frustum culling, section-plane and section-box culling, level-of-detail selection and
  two-phase occlusion culling against a hierarchical depth buffer built from the previous frame.
- Surviving clusters are written to indirect draw buffers; the CPU issues a constant number of draw
  calls regardless of item count.
- Per-item attributes (visibility state, override color, transparency, selection highlight,
  transform override index) are a GPU buffer indexed by item, so appearance changes are buffer
  updates.
- Transparent items are drawn in a separate pass with weighted blending to avoid sorting millions
  of items.
- Picking renders item identifiers into an offscreen target; measurement snapping reads nearby
  cluster geometry on the CPU from resident pages.
- On WebGL2, where compute shaders are absent, culling falls back to CPU-side hierarchy traversal
  with the same level-of-detail rules.

## Memory budgets

Budgets are settings with defaults derived from the device; the residency manager never exceeds
them, and the user is told when the visible set needs more than the budget allows.

| Target | Resident CPU memory | GPU memory | Notes |
|---|---|---|---|
| Desktop application | 50 percent of physical memory by default | 70 percent of dedicated video memory | Analysis readers stream outside the render budget |
| Read-only viewer | Same as desktop | Same as desktop | No analysis engines |
| Browser bundle | 1.5 GiB WebAssembly heap target | Browser-reported limits, 1 GiB default | Pages beyond the heap stay in browser cache storage |
| Immersive mode | Desktop budget | Reduced to keep frame time at the runtime rate | Error threshold raised automatically to hold frame rate |

## Performance targets

| Target | Value | Rationale |
|---|---|---|
| Time to first frame, warm cache, any federation size | under 2 s | Only header, item tables of visible sources and coarse levels are needed. |
| Time to first frame from Hub in a browser | under 5 s on a 50 Mbit/s link | Coarse levels are small; refinement continues during interaction. |
| Frame rate during navigation | 60 target, 30 minimum on reference hardware | Frame governor trades detail for rate. |
| Refinement to full detail after motion stops | under 1 s for the visible set from disk | Residency prefers pages with the largest error reduction. |
| Opening the model tree of 10 million items | under 1 s after headers are read | Item tables are columnar and virtualized in the tree widget. |
| Memory per scene item when not visible | under 64 bytes on average | Item table only; properties and geometry stay paged. |
| Conversion of a source into a scene cache | scales with cores; streamable while converting | Pages become visible as soon as they are written. |

Targets are measured on generated federations (procedural buildings, infrastructure corridors and
point clouds of known sizes) so the corpus contains no third-party sample models.
