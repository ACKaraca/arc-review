# Federation and model sources

Federation combines model sources of many formats and disciplines into one scene for review, with
each source placed by its own unit and transform. Sources are read, converted into scene caches and
referenced from the review document; they are never modified (see
[ADR 0001](../../adr/0001-review-data-separate-from-sources.md) and
[ADR 0003](../../adr/0003-content-addressed-scene-cache.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Review document | `.arcrev` package, format version, metadata, linked or packaged mode | Owns one federation and all review data |
| Federation | Ordered list of model sources, project base point, project CRS record | Belongs to a review document; produces the model tree |
| Model source | Source id, path or Hub version reference, format, content hash, declared unit, reader options, enabled flag | Has one placement and one scene cache; root of a subtree in the model tree |
| Placement | Unit scale, rigid transform (translation, rotation), optional survey or CRS alignment | Belongs to one model source |
| Scene cache | Content key (source hash, reader version, reader options), geometry, properties, item keys | Derived from one model source; shared across review documents |
| File list | Plain-text list of model paths, one per line, optional relative paths | Expands into model sources on open or append |
| Reader | Format, version, option schema, fidelity level | Converts a model source into a scene cache |

## Opening and documents

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-FED-001 | Open a model file | Opening a supported model file creates a new unsaved review document whose federation holds that file as its first model source. The scene, model tree and properties are available when conversion completes; a progress indicator and cancel action are shown during conversion. | T1 | 1 |
| RV-FED-002 | Open a review document | Opening an `.arcrev` file restores the federation, resolves every model source, loads or rebuilds its scene cache and restores all review data. Sources that cannot be found are listed as missing and the rest of the document opens. | T1 | 1 |
| RV-FED-003 | Linked review document | A linked review document stores model source references, placements and review data only, so its size is independent of model size. Sources are re-read from their references whenever the document opens or is refreshed. | T1 | 1 |
| RV-FED-004 | Packaged review document | Saving as packaged embeds the scene cache of every enabled model source in the review document so it opens without access to the sources. A packaged document records the content hash and date of each embedded source and does not refresh until relinked. | T1 | 1 |
| RV-FED-005 | Append model sources | Append adds one or more files as new model sources at the end of the federation. Each keeps its own subtree, unit, placement and reader options and can be refreshed, replaced, disabled or removed independently. | T1 | 1 |
| RV-FED-006 | Merge review data | Merge reads another review document and imports its viewpoints, sets, appearance rules, markups, comments and tests into the current document. Objects are matched by identifier; duplicates are skipped or renamed according to a choice made before the merge, and a summary lists what was added, skipped and renamed. | T2 | 1 |
| RV-FED-007 | Merge partial models of one source | When two model sources declare the same originating model (same project identifier and matching item keys), the federation offers to combine them under one subtree so items present in both appear once. Conflicting property values are kept under both sources' property sets. | T3 | 3 |
| RV-FED-008 | Open from a file list | Opening or appending a file list adds every listed path as a model source in list order. Lines that are blank or start with `#` are ignored, relative paths resolve against the list's folder, and unreadable entries are reported without stopping the load. | T2 | 1 |
| RV-FED-009 | Replace a model source | Replace swaps a model source's file for another while keeping its source id, placement and position in the federation. Review data bound to item keys that exist in the new file stays attached; unmatched references are reported. | T2 | 1 |
| RV-FED-010 | Disable and remove model sources | A disabled model source stays in the federation but is not loaded or shown; removing a source deletes it from the federation and reports the review data that referenced its items. | T1 | 1 |
| RV-FED-011 | Reorder and group model sources | Model sources can be reordered and grouped into named folders in the federation; the model tree follows the federation order. | T2 | 1 |
| RV-FED-012 | Open Arc product documents | Arc product documents are opened by invoking the installed product's headless CLI to export IFC or an Arc Scene, which then becomes the converted input of the model source. If the product is not installed, the source is reported as unreadable with the reason. | T2 | 2 |

## Refresh and change awareness

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-FED-013 | Detect changed sources | On open and on demand, the federation compares each model source's current content hash with the hash recorded in the review document and lists changed, missing and unchanged sources. | T1 | 1 |
| RV-FED-014 | Refresh | Refresh re-reads the changed sources, rebuilds their scene caches and updates the scene and model tree while preserving selection, visibility, overrides and review data bound to surviving item keys. | T1 | 1 |
| RV-FED-015 | Automatic refresh policy | A document setting chooses whether changed sources refresh automatically on open, after a prompt, or only on demand. Hub-referenced sources can follow the latest version or stay pinned to a version. | T2 | 2 |
| RV-FED-016 | Refresh report | Each refresh produces a report per source: items added, removed and changed (geometry or properties), and review objects whose referenced items no longer exist. The report links to the comparison area for detail. | T2 | 1 |
| RV-FED-017 | Watch source folders | When enabled, the application watches the folders of linked model sources and marks sources as changed as soon as their files change, without refreshing until asked. | T3 | 2 |

## Units, placement and alignment

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-FED-018 | Detect source units | Each reader reports the unit declared by the source file; when none is declared the reader's configured default unit is used and the source is flagged as unit-assumed. | T1 | 1 |
| RV-FED-019 | Override source unit | The user can set a model source's unit explicitly, which rescales its placement; the override is stored in the review document and shown in the source's properties. | T1 | 1 |
| RV-FED-020 | Edit placement numerically | A model source's translation, rotation about each axis and uniform scale can be entered numerically in display units; the scene updates immediately and the edit is undoable. | T1 | 1 |
| RV-FED-021 | Edit placement of many sources | Unit and placement edits can be applied to several selected model sources at once, either setting absolute values or applying a relative offset to each. | T2 | 1 |
| RV-FED-022 | Align by point pairs | The user picks one to three pairs of corresponding points (a point on the moving source and its target in the federation); the placement is solved as a rigid transform that best fits the pairs and the residual error per pair is shown before applying. | T2 | 1 |
| RV-FED-023 | Shared coordinates and georeference | When sources carry a survey point, map conversion or CRS record (for example IFC map conversion), the federation places them by that reference automatically. Sources in different CRS are transformed into the project CRS when a transformation is available; otherwise the source is flagged and placed by its local origin. | T2 | 2 |
| RV-FED-024 | Alignment suggestions | After append, the federation compares the new source's bounding box, grids and levels with the existing sources and proposes a placement (unit change, offset or rotation) when the source appears misplaced, for example a thousand-fold unit mismatch or a large origin offset. Suggestions are applied only on confirmation. | T3 | 2 |
| RV-FED-025 | Reset placement | Reset restores a model source's placement to the one derived from its file and declared unit. | T1 | 1 |
| RV-FED-026 | Project base point | The federation has a project base point and orientation; display coordinates can be shown relative to it, to the survey point or to the world origin. Large georeferenced coordinates keep full precision through camera-relative rendering. | T1 | 1 |

## Readers and formats

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-FED-027 | IFC reader | Reads IFC 2x3, IFC4 and IFC4.3 (STEP physical file and zipped) with spatial structure, element types, property sets, quantity sets, classification references, materials, units and map conversion. All common geometry representations (extrusions, swept solids, B-rep, tessellations, booleans, mapped items) are converted. | T1 | 1 |
| RV-FED-028 | DWG and DXF reader | Reads 3D solids, surfaces, meshes, polyface meshes, lines, points and text from model space, with layers, blocks and attributes as hierarchy and properties. Objects on frozen or off layers are read when an option asks for them and start hidden. | T1 | 1 |
| RV-FED-029 | STEP reader | Reads STEP AP203, AP214 and AP242 with assembly structure, part names, colors and product metadata; semantic PMI is shown as view-only annotation. | T1 | 1 |
| RV-FED-030 | Mesh and scene readers | Reads glTF and GLB, OBJ, STL, PLY meshes, 3MF, COLLADA and OpenUSD with node hierarchy, materials and textures where the format provides them. | T1 | 1 |
| RV-FED-031 | Additional open readers | Reads IGES surfaces and curves, 3D Tiles tilesets, CityJSON city models with levels of detail and semantics, and LandXML surfaces and alignments as view-level geometry with properties. | T2 | 3 |
| RV-FED-032 | Bridged readers | Optional readers for formats that require libraries outside the dependency policy, including a conversion library the user has licensed separately, run in a separate process behind a byte-level bridge. The library is never distributed with Arc Review. A missing bridge disables only those formats (see [27-application-connectors.md](27-application-connectors.md)). | T3 | 3 |
| RV-FED-033 | Reader options | Each reader exposes typed options (for example include hidden objects, read lines and points, hierarchy depth, default unit, tessellation tolerance). Global defaults are set in settings and can be overridden per model source; changing an option invalidates that source's scene cache. | T2 | 1 |
| RV-FED-034 | Unsupported formats | Opening a file with no reader reports the format and the ways it can be exchanged instead (a connector, a registered bridge or an open format); nothing is partially loaded. | T1 | 1 |
| RV-FED-035 | Conversion loss report | Each conversion records what the reader could not represent (unsupported entities, failed geometry, skipped properties) with counts and examples; the report is available per model source. | T1 | 1 |
| RV-FED-044 | 3DM reader | Reads 3DM files through an open reader under an allowed license: object hierarchy by layer, NURBS and mesh geometry tessellated to the chosen tolerance, block instances, materials, user text as properties and the declared unit. Item key: object identifier. | T3 | 5 |
| RV-FED-045 | FBX reader | Reads FBX scenes through an open reader: node hierarchy, meshes, instances, materials and textures, custom properties and the declared unit. Animation is not read. Item key: node path with the node identifier where present. | T3 | 5 |
| RV-FED-046 | DGN V7 reader | Reads the published DGN V7 format: 3D elements, cells as instances, levels as hierarchy, attributes where present and the declared master unit. Item key: file plus element handle. | T3 | 5 |
| RV-FED-047 | JT reader | Reads JT files from the published specification: assembly structure, tessellated shape levels, product and manufacturing attributes as properties. Item key: product path and instance identifiers. | T3 | 5 |
| RV-FED-048 | Sources from connectors | A model source can be an Arc exchange package or a scene cache written by a connector in an authoring application; its refresh, watch and switch-back behavior is specified in [27-application-connectors.md](27-application-connectors.md). | T3 | 5 |

## Caches, recovery and settings

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-FED-036 | Scene cache reuse | A model source whose content key matches an existing scene cache loads from the cache without re-reading the source. Caches live in a configurable cache directory and are shared by every review document on the machine. | T1 | 1 |
| RV-FED-037 | Cache management | The cache directory has a size limit; the least recently used caches are evicted first. The user can view cache size per source and clear caches; a cleared cache is rebuilt on next open. | T2 | 1 |
| RV-FED-038 | Autosave and recovery | Unsaved review data is journaled continuously; after an abnormal exit the next start offers to restore it. Autosave and temporary locations accept environment variables in their paths. | T1 | 1 |
| RV-FED-039 | Path resolution | Linked source paths are stored relative to the review document when possible, with an absolute fallback and an ordered list of project search folders. A missing source can be relinked by browsing, and relinking one source offers to relink others found in the same folder. | T1 | 1 |
| RV-FED-040 | Scene statistics | A statistics panel shows per source and in total: items, triangles, lines, points, instances, texture memory, scene cache size and world bounding box in project coordinates. Sources far from the project origin are highlighted. | T2 | 1 |
| RV-FED-041 | Source information | Each model source shows its file information: path, size, format and schema version, authoring application, author, creation date, declared unit and content hash. | T1 | 1 |
| RV-FED-042 | Document display settings | Per-document settings store display preferences for the federation (lighting, background, culling and performance limits, default pick level); they travel with the review document. | T2 | 1 |
| RV-FED-043 | Settings profiles | Application settings can be exported to and imported from a settings file; a project or organization profile file can be referenced so a team shares the same options, with locally overridden values marked. | T2 | 1 |
