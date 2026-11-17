# 0001 Review data is stored apart from source models, which are never edited

- Status: Accepted
- Scope: `review-model`, `review-io`, `review-commands`; all capability areas; the `.arcrev` format

## Context

Arc Review federates models produced by many authors and many applications: IFC exports, DWG and
DXF drawings, STEP assemblies, glTF and OpenUSD scenes, point clouds and documents of other Arc
products. The people who review these models (coordinators, planners, estimators, owners) are
rarely their authors and usually have no right to change them. Review work nevertheless produces a
large amount of data: viewpoints, selection and search sets, appearance rules, markups, comments,
clash tests and results, check findings, issues, 4D tasks, cost and takeoff records, comparisons.

Two designs are possible: write review data into the source files, or keep it in a document of its
own that refers to the sources. The suite principle "the document is the authority" requires each
document to have one owner; a review tool that writes into an author's model breaks that ownership,
invalidates signatures and checksums, and creates conflicts with the author's next export.

## Decision

1. Arc Review never writes to a source model. Readers open sources read-only; no command in the
   registry has a source file as its output. Exports (glTF, OpenUSD, 3D Tiles, BCF, reports,
   converted DWG copies) always write new files chosen by the user.
2. Arc Review derives a federation from the sources: an ordered list of model sources, each with
   a content hash, a declared unit and a placement (unit scale, rigid transform, optional survey or
   CRS alignment). Placement, visibility, appearance and transform overrides live in the review
   document and change only how the source is presented.
3. All review data lives in the review document (`.arcrev`), a document package as defined by the
   suite (one SQLite file, typed objects with stable identifiers, transactions, operation journal).
4. Review data refers to source content only through item keys (model source identifier plus the
   source's own element identifier, such as an IFC GlobalId or a DWG handle, or a derived path hash
   when the source has none). It never stores copies of source geometry, except in the scene cache
   (see [ADR 0003](0003-content-addressed-scene-cache.md)), which is derived and rebuildable.
5. A review document exists in two forms: **linked** (references and review data only) and
   **packaged** (the same document with the scene caches of its sources embedded, for hand-off to
   recipients without access to the sources). Both are the same schema.
6. User-defined properties, external data links and classification mappings are review data: they
   are attached to item keys in the review document and shown beside source properties, clearly
   marked as review-side data.
7. Round trips to authoring tools go through identifiers, never through file modification: BCF
   components, exported element identifier lists, and the automation-protocol selection request to a
   running Arc product.

## Consequences

- A review document stays small and fast to save, independent of model size; review data survives
  any number of source updates.
- Refreshing a source is safe: the new content gets a new scene cache, item keys are matched, and
  review objects that refer to vanished items are reported, not deleted.
- Identity quality of sources becomes the main risk; Arc Review must measure it (key quality per
  source) and offer mapping when keys change (RV-TREE-012 to RV-TREE-014).
- Review-side properties cannot be pushed back into a source; authors receive them as issues,
  reports or exported tables.
- The packaged form duplicates source content; publishing restrictions (excluded items, hidden
  properties) apply at packaging time.

## Alternatives considered

- **Write review data into source files** (for example IFC property sets or DWG extension
  dictionaries). Rejected: needs write access, breaks author ownership, conflicts with re-export,
  and is impossible for formats without extension mechanisms.
- **Store review data per source in sidecar files.** Rejected: federation-level data (clash tests
  between sources, 4D tasks spanning sources) has no natural home, and sidecars get separated from
  their source.
- **Import sources into a single editable model.** Rejected: turns the reviewer into an author,
  loses the link to source versions and duplicates work done by authoring products.

## Reopen when

A required workflow cannot be served without changing a source file, for example a standard that
mandates review results to be embedded in the delivered model, and identifier-based exchange
(BCF, IDS reports) cannot satisfy it.
