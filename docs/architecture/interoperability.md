# Interoperability

Every format Arc Review reads or writes, by direction and fidelity level, with the mapping
principles, validation method and loss reporting behind them. Fidelity levels follow the suite
interoperability policy: **view** (geometry and appearance), **data** (semantics, properties,
relationships), **edit** (native editing). Arc Review never edits source models
([ADR 0001](../adr/0001-review-data-separate-from-sources.md)), so no source format is read at edit
fidelity; review data formats are read and written at edit fidelity because Arc Review owns them.

## Model sources (read)

| Format | Fidelity | Crate | Phase | Notes |
|---|---|---|---|---|
| IFC 2x3, IFC4, IFC4.3 (STEP and ZIP forms) | data | `arc-io-ifc` | 1 | All geometry representations used in practice (extrusions, sweeps, B-rep, tessellations, CSG, mapped items); property and quantity sets; spatial structure; relationships; classification references; map conversion and CRS. Item key: GlobalId. |
| DWG (read), DXF | data | `arc-io-dwg`, `arc-io-dxf` | 1 | 3D solids, surfaces, meshes, lines, points, blocks with attributes, layers, extension data; proxy graphics shown as stored. Item key: drawing plus handle. |
| STEP AP203, AP214, AP242 | view, plus assembly structure and product data | `arc-io-step` | 1 | Tessellated by `arc-brep`; semantic PMI displayed as view data. Item key: product path and instance identifiers. |
| glTF 2.0, GLB | view | `arc-io-mesh` | 1 | Node hierarchy, names and extras as properties. Item key: node path. |
| OBJ, STL, PLY, 3MF | view | `arc-io-mesh` | 1 | Groups as items where present. |
| OpenUSD (usda, usdc, usdz) | view, plus prim metadata | `arc-io-usd` | 3 | Composed stage; prim attributes as properties. Item key: prim path. |
| COLLADA, IGES | view | `arc-io-mesh`, `arc-io-step` | 3 | Lower priority readers. |
| CityJSON | data | `arc-io-geo` | 3 | City objects with semantics and levels of detail. |
| LandXML | view, plus alignment and surface names | `arc-io-geo` | 3 | Surfaces and alignments as context. |
| 3D Tiles | view | `review-io` | 3 | Streamed as reality context, not converted. |
| E57, LAS, LAZ, PLY, PTS, PTX, ASCII XYZ | view | `arc-io-pointcloud` | 3 | Converted to octree scene caches. |
| Arc product documents (`.archb`, `.civilb`, `.solidb`, `.meshb`, `.draftb`) | data | via product CLI | 2 | Arc Review asks the installed product's headless CLI to export IFC or an Arc Scene; otherwise the user exports. Direct package reading is an open question. |
| Plain-text file lists | n/a | `review-io` | 1 | One path per line; expands to model sources. |
| Arc exchange package and scene cache written by a connector | data | `review-io` | 5 | Written by an add-in inside the authoring application through its public API; item key: source id plus host element identifier. See [connectors](#connectors-and-licensed-bridges). |
| 3DM | view, plus layers and user text | `review-io` over an open reader | 5 | NURBS and meshes tessellated; blocks as instances. Item key: object identifier. |
| FBX | view | `review-io` over an open reader | 5 | Hierarchy, meshes, materials, custom properties; animation not read. |
| DGN V7 | view, plus attributes | `review-io` from the published format | 5 | Cells as instances, levels as hierarchy. |
| JT | view, plus product attributes | `review-io` from the published specification | 5 | Assembly structure and tessellated shape levels. |
| SKP and other native project files without an open reader | view or data | connector or licensed bridge | 5 | Through a connector in the authoring application, or a conversion library the user has licensed and registered. Never required for a core workflow. |

## Drawings and sheets (read)

| Format | Fidelity | Phase | Notes |
|---|---|---|---|
| DWG, DXF layouts and model space | data | 1 | Layers, layer states, named views, external references, attached images, annotation scales, plot settings. |
| PDF | view, plus text and layers | 1 | Vector pages with optional content groups as layers; calibration for scale. |
| PNG, JPEG, TIFF | view | 1 | Raster sheets with calibration. |
| DWF, DWFx | view | 2 | Optional through a bridge; open question. |

## Planning and cost data (read)

| Format | Fidelity | Phase | Notes |
|---|---|---|---|
| Microsoft Project XML (MSPDI) | data | 3 | Tasks, hierarchy, planned and actual dates, cost fields; from phase 5 also links, lags, calendars and constraints for critical path calculation. |
| Primavera P6 XER and P6 XML | data | 3 | Activities, WBS, dates, cost; from phase 5 also relationships, calendars and constraints. |
| CSV, XLSX | data | 3 | Field mapping defined per schedule source or catalog. |
| Unit-rate cost databases (CSV, XLSX, documented JSON) | data | 5 | Entries with code, unit and rates by category; no price data is shipped, and licensed databases are imported by mapping. |
| IDS 1.0 | edit | 3 | Information requirements; read and written. |
| BCF 2.1, 3.0 (files and API) | edit | 2 | Native issue model ([ADR 0005](../adr/0005-bcf-native-issue-model.md)). |
| COBie (spreadsheet or IFC) | data | 3 | Validation input. |
| bSDD | data | 3 | Online dictionary lookup with offline cache of chosen entries. |

## Outputs (write)

| Format | Fidelity | Phase | Notes |
|---|---|---|---|
| Review document `.arcrev` (linked, packaged) | edit | 1 | Documented SQLite package schema. |
| Documented JSON review data | edit | 1 | Sets, viewpoints, appearance rules, clash tests and results, check rule sets and findings, takeoff workbooks, sequencing; schemas published in this repository. |
| BCF 2.1, 3.0 | edit | 2 | Issues with viewpoints, components, snapshots and extensions. |
| IDS 1.0 | edit | 3 | From authored information requirements. |
| glTF 2.0, GLB | view | 1 | Visible federation with appearance and properties as extras. |
| OpenUSD | view | 4 | Federation as a layered stage. |
| Arc Scene | view | 4 | For RenderBuilder and other Arc products. |
| 3D Tiles | view | 3 | For web and GIS context delivery. |
| KML, KMZ | view | 3 | Placemarks for viewpoints and issues, model as glTF. |
| CityJSON | view | 3 | Building envelopes for urban context exchange. |
| IFC (review outputs) | data | 3 | Cost and quantity sets, BCF-referenced annotations; never a modified copy of a source. |
| COBie spreadsheet | data | 3 | Derived from federation properties. |
| PDF (vector), SVG | view | 1 | Sheets, prints, reports, section outlines. |
| PNG, JPEG, TIFF, EXR | view | 1 | Images; EXR for rendered output. |
| Image sequences, WebM (AV1) | view | 2 to 4 | Camera paths, simulations, animations. |
| CSV, XLSX | data | 1 | Properties, results, workbooks, task tables. |
| Schedule scenarios (MSPDI, CSV, XLSX) | data | 5 | Resequenced schedules and change lists as proposals for the scheduling tool. |
| Unit-rate databases and estimate versions (CSV, XLSX, JSON) | data | 5 | Databases, estimates and version comparisons. |
| HTML reports | view | 3 | Self-contained reports with images. |
| DWG, DXF (version conversion) | data | 2 | Converted copies of drawings to a chosen version; DWG write arrives in phase 2 with `arc-io-dwg`. |

## Connectors and licensed bridges

Formats without a published specification are exchanged by connectors, not read from disk. A
connector is an add-in that runs inside the user's own licensed copy of the authoring application,
uses only that application's public API and writes an Arc exchange package or a scene cache
([suite ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md)).
Connectors are developed in the `arc-connectors` repository and never modify the model of the
application they run in. Arc Review reads their output at data fidelity and, through the
selection exchange of RV-AUTO-010, sends selection and view requests back to them
([capabilities](../product/capabilities/27-application-connectors.md)).

A licensed bridge is a conversion library that the user has licensed from its vendor or consortium
and registered by path. It runs in a separate process behind the byte-level bridge, is never
distributed with Arc Review and is never required for a core workflow. An open reader is used where
a permissively licensed reader or a published specification exists.

## Mapping principles

1. **Readers produce neutral models.** `arc-io-*` crates parse formats into neutral structures;
   `review-io` maps them into scene caches. Arc Review owns the mapping, not the format crate.
2. **Hierarchy as authored.** The model tree reflects the source's own structure (spatial
   structure for IFC, layers and blocks for DWG, assembly for STEP, node hierarchy for glTF and
   USD). Grouping by property is a view, not a restructuring.
3. **Typed properties.** Values keep their type and unit (quantity with unit, date, boolean,
   identifier, URI). Source property-set names are preserved; no renaming into an Arc vocabulary.
4. **Stable identity.** Each item gets an item key from the source identifier the format provides.
   When none exists, a derived key from the item path and geometry hash is used and marked as
   derived, and the source's key quality is reported.
5. **Units and coordinates.** Source units are detected and recorded; placement converts to
   meters. CRS and map conversion records are kept, so sources with georeference align without
   manual transforms.
6. **Nothing silently dropped.** Content the scene cache cannot represent (unsupported geometry,
   unknown entities) is listed in the conversion loss report with counts and examples; where
   possible it is still shown as stored proxy graphics.
7. **Outputs carry identifiers.** Every export, report and BCF topic includes source element
   identifiers alongside item keys so receiving tools can locate elements.

## Validation

| Output | Independent check in CI |
|---|---|
| BCF files | Validation against the published BCF XML schemas |
| BCF API client | Recorded exchanges against Arc Hub and the published specification examples |
| IDS | Published IDS test cases |
| glTF | Khronos glTF validator |
| IFC outputs | Reading with IfcOpenShell and schema validation |
| OpenUSD | Composition and validation with the reference OpenUSD tools |
| 3D Tiles | Published 3D Tiles validator |
| PDF | Reading back with an independent PDF parser; text and layer checks |
| DWG, DXF | Round trip through Arc readers plus an independent open-source reader where available |
| JSON review data | JSON Schema validation of every written file |
| Exchange packages from connectors | Schema validation of the package and a corpus of packages with expected item counts, keys and properties; scene caches are verified on load |
| Schedule scenario exports | Reading back with the published MSPDI and P6 XML schemas |

Readers are validated against corpus files with expected item counts, keys, properties and bounds,
and are fuzzed.

## Loss reporting

Every import and export produces a loss report stored with the model source or beside the output:
items or entities not converted (by type, with counts), properties not mapped, geometry
approximated (tessellation tolerance), units assumed, and features dropped for an older target
version. The report is viewable in the application and exportable as JSON and CSV.

## Deliberately unsupported

| Content | Why | How it is exchanged instead |
|---|---|---|
| Any format without a published specification, read by inspecting its files or binaries | Reverse engineering is excluded by suite ADR-0017; compiled or protected script files of other applications and codecs that forbid open distribution are excluded for the same reason | A connector, a licensed bridge, an open reader, or IFC, STEP, glTF, OpenUSD and JT |
| Proprietary cache and published-package formats of other review tools | No published specification | IFC, BCF and the documented JSON review data |
| Binary project-scheduling files | No published specification | MSPDI XML, P6 XML or XER, CSV |
| Writing to any source model | Review data is kept apart ([ADR 0001](../adr/0001-review-data-separate-from-sources.md)) | BCF, reports and identifier lists to the author |
