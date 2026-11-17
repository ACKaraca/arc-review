# Model and drawing comparison

Comparison reports what changed between two versions of a model or a drawing, or between two
selections in the same federation. Model comparison matches items by item key and reports
geometry, placement and property differences; drawing comparison overlays two sheets and
highlights differences. Results are kept as comparison runs in the review document.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Comparison run | Left and right inputs (model source versions, roots, sets or sheets), match settings, tolerance, time, counts per difference kind | Owns difference records |
| Difference record | Kind (added, removed, geometry changed, moved, properties changed, unchanged), left and right item keys, changed property list with old and new values, geometric deviation | Belongs to a comparison run; references scene items |
| Match settings | Match keys (item key, name, property, geometry), ignored properties, geometry tolerance, include hidden | Used by a comparison run |
| Sheet overlay | Left and right sheets, alignment transform, colors for left-only, right-only and common content | Belongs to a comparison run |

## Model comparison

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-COMP-001 | Compare model versions | Two versions of a model source, two model roots or two sets are compared; items are matched by item key and classified as added, removed, geometry changed, moved, properties changed or unchanged. The comparison runs in the background and is cancellable. | T1 | 3 |
| RV-COMP-002 | Match settings | Matching uses item key by default and can fall back to name, a chosen property or geometric similarity; properties can be ignored and a geometry tolerance set so that re-tessellation noise is not reported. | T1 | 3 |
| RV-COMP-003 | Property-level differences | For each changed item the result lists every changed property with old and new value and property set name; property changes can be filtered by property name. | T1 | 3 |
| RV-COMP-004 | Geometry differences | Geometry changes are reported with the maximum deviation and the change in volume and bounding box; pure placement changes are reported as moved with the translation and rotation. | T2 | 3 |
| RV-COMP-005 | Difference coloring | Items are colored by difference kind in the viewport, with removed items shown as ghosted geometry from the earlier version, and unchanged items optionally hidden. | T1 | 3 |
| RV-COMP-006 | Results as sets | Each difference kind can be saved as a selection set for use in viewpoints, clash tests and takeoff. | T1 | 3 |
| RV-COMP-007 | Comparison on refresh | When a model source is refreshed, a comparison against the previous version runs automatically if enabled, and the change summary is shown with the refresh result. | T2 | 3 |
| RV-COMP-008 | Comparison report | A report lists differences grouped by kind, category and level with property changes, writes HTML, PDF, CSV, XLSX and JSON, and can export difference groups as BCF topics. | T2 | 3 |
| RV-COMP-009 | Compare two items | Two selected items are compared directly, listing property and geometry differences side by side. | T2 | 3 |
| RV-COMP-010 | Version timeline | For a model source with several versions available locally or on a Hub, a timeline shows change counts between consecutive versions and opens any pair for comparison. | T3 | 3 |

## Drawing comparison

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-COMP-011 | Sheet overlay compare | Two sheets or two revisions of one sheet are overlaid with content only in the first, only in the second and in both shown in three colors; vector sheets are compared by entity geometry and raster or scanned sheets by pixels. | T1 | 3 |
| RV-COMP-012 | Sheet alignment | Sheets are aligned automatically by their sheet frame or by two picked point pairs when the revisions are shifted or scaled. | T2 | 3 |
| RV-COMP-013 | Change regions | Differences are clustered into change regions listed with thumbnails; selecting a region zooms both sheets to it, and a region can be turned into a markup cloud or an issue. | T2 | 3 |
| RV-COMP-014 | Side-by-side view | Two sheets are shown side by side with synchronized pan and zoom as an option to the overlay. | T2 | 3 |
| RV-COMP-015 | Drawing entity comparison | For DWG and DXF drawings, entities are matched by handle and differences in geometry, layer and properties are listed in addition to the visual overlay. | T3 | 3 |
