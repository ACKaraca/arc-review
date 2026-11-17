# 0003 Content-addressed scene cache per model source

- Status: Accepted
- Scope: `review-scene`, `review-io`; RV-FED, RV-PUB, RV-HUB, RV-WEB; see
  [large models](../architecture/large-models.md)

## Context

Source formats are slow to parse and poorly suited to interactive display: IFC needs geometry
evaluation of extrusions, sweeps and Booleans; STEP needs tessellation of B-rep faces; DWG needs
proxy and block expansion. A federation of tens of sources must open in seconds and refresh only
what changed. The same converted data must be usable by the desktop, the read-only viewer, the
browser (streamed from Hub) and headless jobs.

Caches written next to the source file break on read-only shares, collide when two machines use
different reader versions, and leak when sources move.

## Decision

1. Each model source is converted into a **scene cache**: a self-describing, versioned binary
   container with the item table (item keys, hierarchy, flags), the property store, deduplicated
   geometry in clusters with a level-of-detail chain, and a spatial hierarchy.
2. A scene cache is addressed by a **content key**: hash of the source bytes, reader identifier and
   version, and the normalized reader options. Equal keys mean equal caches on any machine.
3. Caches live in a cache directory (per user by default, a shared path or a Hub store optionally),
   never next to the source. A review document stores content keys, not cache paths.
4. A scene cache is derived data: it can be deleted at any time and is rebuilt on demand. A packaged
   review document embeds caches by content key; Hub stores them by the same key.
5. Caches are split into pages (bounded size, individually hashed) so that streaming, partial
   loading and HTTP range requests work identically from disk and from Hub.
6. The cache format is documented and versioned. A newer application reads every older cache major
   version or rebuilds it; it never fails to open a document because a cache is old.

## Consequences

- Opening a document whose caches are warm costs no source parsing; refresh reconverts only
  sources whose hash changed.
- A converted source is shared by every review document and every user of a shared cache or Hub.
- Reader fixes change the reader version and invalidate affected caches automatically.
- Disk use grows with source versions; cache management (RV-FED-037) bounds it with size limits and
  least-recently-used eviction.
- The cache format becomes a public contract for Hub and the browser bundle and needs the same
  compatibility discipline as the document format.

## Alternatives considered

- **Cache next to the source.** Rejected: fails on read-only locations, ties identity to paths and
  makes reader-version mismatches silent.
- **Embed converted geometry in every review document.** Rejected for linked documents: duplicates
  data per document and makes saves slow. Kept as the packaged form only.
- **Use glTF or 3D Tiles as the cache.** Rejected as the primary cache: they lack the property
  store, item keys and the review-specific spatial index; both remain export targets.

## Reopen when

Hub-side conversion and streaming become the dominant path and a single shared store would be
simpler than local caches, or a standard streaming format gains the property and identity features
the cache needs.
