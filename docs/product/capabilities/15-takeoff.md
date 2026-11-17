# Quantity takeoff

Quantity takeoff measures counts, lengths, areas, volumes and weights from model items and from
drawing sheets and organizes them in a catalog of takeoff items and resources with formulas. Takeoff
records stay linked to their geometry, so source updates are detected and quantities can be
reviewed rather than measured again. Workbooks have a published JSON schema.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Takeoff catalog | Name, group tree, units system | Contains takeoff items and resources |
| Takeoff item | Code, description, group, unit, quantity fields (count, length, width, thickness, height, perimeter, area, volume, weight), formulas, property mapping | Has takeoff records; uses resources and formulas |
| Resource | Code, description, kind (labor, material, equipment, other), unit, rate | Used by takeoff items through formulas |
| Formula | Target field, expression over quantities, item properties, constants and resource rates, unit dimension | Belongs to a takeoff item or resource use |
| Takeoff record | Kind (3D, 2D, virtual), source reference (item key or sheet geometry), measured values, computed values, change state, author, time | Belongs to one takeoff item |
| Property mapping | Quantity field to item property or geometric measure, per model source or category | Used to compute 3D records |

## Catalog and formulas

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-QTO-001 | Takeoff workbook | A takeoff workbook shows the catalog tree with each item's records and totals and a resource view with totals per resource. Workbooks are part of the review document. | T1 | 3 |
| RV-QTO-002 | Item catalog | Takeoff items are organized in a group tree with code, description and unit; items can be added, moved, copied and deleted as undoable commands. | T1 | 3 |
| RV-QTO-003 | Resource catalog | Resources with kind, unit and rate are managed in their own catalog and used by takeoff items through formulas. | T2 | 3 |
| RV-QTO-004 | Formulas | Formulas compute quantity fields and resource quantities from measured values, item properties, constants and rates using the unit-aware expression engine; a formula whose unit dimension does not match its target is rejected. | T1 | 3 |
| RV-QTO-005 | Catalog exchange | Catalogs import from and export to CSV, XLSX with a column mapping and the documented JSON workbook format. | T1 | 3 |
| RV-QTO-006 | Units | Quantities are stored in SI units and displayed in the workbook's display units with configurable precision. | T1 | 3 |

## 3D takeoff

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-QTO-007 | Model takeoff | Selected scene items, a selection set or a search set are taken off to a takeoff item, creating one record per item (or one aggregated record) with values from the property mapping. | T1 | 3 |
| RV-QTO-008 | Property mapping | Each quantity field maps to a source property (for example a net volume property) or to a geometric measure computed from the item's geometry; the mapping is set per model source or category and the value source is shown per record. | T1 | 3 |
| RV-QTO-009 | Geometric measures | When no property supplies a quantity, volume, surface area, projected area, length along the main axis and bounding dimensions are computed from closed geometry; items with open geometry are flagged instead of returning a value. | T2 | 3 |
| RV-QTO-010 | Takeoff by sets | A command creates takeoff items and records for each set in a set folder or each distinct value of a property, so a full workbook is generated from the sets library. | T2 | 3 |
| RV-QTO-011 | Takeoff highlighting | Items already taken off are shown in a takeoff color, items not taken off can be isolated, and selecting a record or takeoff item highlights its geometry. | T1 | 3 |
| RV-QTO-012 | Double-count check | Items taken off to more than one takeoff item of the same group are listed. | T2 | 3 |

## 2D takeoff

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-QTO-013 | Sheet takeoff | On a calibrated sheet, count markers, polylines (length) and polygons (area, with cut-outs) are placed as takeoff records with the sheet's scale (calibration in [17-drawings.md](17-drawings.md)). | T1 | 3 |
| RV-QTO-014 | Snapping on vector sheets | 2D takeoff snaps to endpoints, midpoints, intersections and edges of vector sheet geometry. | T2 | 3 |
| RV-QTO-015 | 2D record display | 2D records are drawn on the sheet in the takeoff item's color and can be selected, edited and deleted; editing a shape updates its values. | T1 | 3 |
| RV-QTO-016 | Sheet revision carry-over | When a sheet is replaced by a new revision, 2D records are carried to the new revision at the same sheet coordinates and flagged for review. | T3 | 3 |

## Virtual takeoff

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-QTO-017 | Virtual records | Records without geometry hold entered quantities or formulas for unmodeled scope (allowances, preliminaries) and can optionally be associated with scene items for reference. | T1 | 3 |

## Change analysis

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-QTO-018 | Change detection after refresh | After a model source refresh, records are compared by item key and marked changed (geometry or mapped property differs, with old and new values), removed (item no longer present) or unchanged; items matching a takeoff set but not yet taken off are listed as new. | T1 | 3 |
| RV-QTO-019 | Change review | Changed and removed records can be accepted (values update, history kept) or rejected individually or in bulk; the workbook shows totals before and after acceptance. | T2 | 3 |
| RV-QTO-020 | Quantity history | Each record keeps its values per accepted revision so quantities can be reported per revision. | T3 | 3 |

## Output

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-QTO-021 | Workbook export | The workbook exports to XLSX, CSV and JSON with chosen columns, grouping and totals; record-level export includes item keys. | T1 | 3 |
| RV-QTO-022 | Print and PDF | The workbook prints and writes PDF with a header, totals per group and optional images of highlighted items. | T2 | 3 |
| RV-QTO-023 | IFC quantity sets | Mapped quantities can be exported as IFC element quantity sets referencing the source elements by GlobalId. | T3 | 3 |
| RV-QTO-024 | Link to cost and schedule | Takeoff items feed cost items and can be attached to tasks, so quantities drive 5D cost (see [14-cost.md](14-cost.md)). | T2 | 3 |
