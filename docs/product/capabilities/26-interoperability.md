# Interoperability

Interoperability covers what Arc Review writes and the exchange of review data with other tools.
Importers are specified in [01-federation.md](01-federation.md),
[19-reality-data.md](19-reality-data.md) and [27-application-connectors.md](27-application-connectors.md); fidelity levels per format and direction are listed in
[interoperability.md](../../architecture/interoperability.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Export job | Format, scope (federation, selection, visibility state), placement option, output path | Produces a file and a loss report |
| Loss report | Per-item and per-feature list of what the target format cannot carry | Accompanies every export and import |
| Review data exchange file | Kind (sets, viewpoints, appearance rules, clash tests, clash results, takeoff, sequencing), schema version | Documented JSON written and read by Arc Review |
| Identifier map | Item key to source identifier (IFC GlobalId, DWG handle, STEP id) | Used for round trips and BCF components |

## Model exports

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-XCHG-001 | glTF export | The federation, a selection or the current visibility state exports to glTF or GLB with materials, hierarchy and item keys plus chosen properties as extras. | T1 | 1 |
| RV-XCHG-002 | OpenUSD export | The federation exports to OpenUSD with one layer per model source, the model tree as prims, properties as custom attributes and presentation materials as MaterialX. | T2 | 4 |
| RV-XCHG-003 | 3D Tiles export | The federation exports to a georeferenced 3D Tiles tileset with level of detail for web and GIS viewers. | T3 | 3 |
| RV-XCHG-004 | KML and KMZ export | Viewpoints export as placemarks and the federation as a georeferenced model in KMZ when the federation has a CRS record. | T3 | 3 |
| RV-XCHG-005 | Arc Scene contract version | Every Arc Scene export (RV-REND-013) declares the contract version it was written with. Writing for an older minor version drops newer constructs and lists them in the loss report; a newer major version is refused with a message. | T2 | 4 |
| RV-XCHG-006 | Georeference on export | Exports carry the federation's CRS record and project base point where the format supports them, or are written in a chosen local or projected coordinate system. | T2 | 2 |
| RV-XCHG-007 | Loss reports | Every export writes a loss report listing items, properties, materials and review data the format cannot carry. | T1 | 1 |
| RV-XCHG-008 | Independent validation | Every export format is validated in continuous integration by an independent reader. | T1 | 1 |

## Open standards

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-XCHG-009 | BCF schema conformance | BCF 2.1 and 3.0 files written by Arc Review (RV-ISSUE-017) validate against the published BCF schemas in CI. When an issue is written as 2.1, content that only 3.0 can carry, such as document references, is listed in the loss report instead of being dropped silently. | T1 | 2 |
| RV-XCHG-010 | BCF API conformance | The BCF API client used by RV-ISSUE-019 follows the published BCF API and OpenCDE foundation API, including server discovery, OpenID Connect or OAuth 2 authorization and paging. Conformance is tested in CI against Arc Hub and against recorded responses of the published specification examples. | T2 | 2 |
| RV-XCHG-011 | IDS conformance | IDS files are read and written according to IDS 1.0, and the validation engine behind RV-CHECK-016 is tested in CI against the published IDS test cases. Facets or restrictions that the engine does not support are reported per specification instead of being treated as passed. | T2 | 3 |
| RV-XCHG-012 | Dictionary references preserved | Classification and property references that carry a bSDD or other dictionary URI keep that URI from the source model through rules, findings, BCF topics and tabular exports. | T3 | 3 |
| RV-XCHG-013 | COBie input | COBie data supplied as a spreadsheet is read and matched to scene items by name and type so the validation of RV-CHECK-020 can run on handover data that arrives separately from the model. | T3 | 3 |
| RV-XCHG-014 | IFC review outputs | Clash results and check findings can be written as IFC-referenced BCF; quantities from takeoff can be written as IFC quantity sets in a separate file that references source GlobalIds, never into the source model. | T3 | 3 |
| RV-XCHG-015 | CityJSON export | A selection of the federation can be written as CityJSON objects with attributes for urban context exchange. | T3 | 3 |

## Review data exchange

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-XCHG-016 | Documented review data formats | Search sets, selection sets, viewpoints, appearance rules, clash tests, clash results, takeoff workbooks and sequencing data export to and import from documented, versioned JSON schemas. | T1 | 1 |
| RV-XCHG-017 | Tabular exports | Properties of a selection, clash results, issues, takeoff and tasks export to CSV and XLSX with chosen columns. | T1 | 1 |
| RV-XCHG-018 | Report exports | Reports render to HTML with images and to PDF. | T2 | 2 |
| RV-XCHG-019 | Identifier maps | Exports and reports carry each item's key and source identifier so other tools can find the element in the source model. | T1 | 1 |
| RV-XCHG-020 | Element identifiers in every output | Every report row, tabular export and BCF component written by Arc Review carries the model source name and the source element identifier in addition to the item key. Authoring tools can therefore locate elements without reading Arc Review data; Arc products use the direct selection request of RV-AUTO-010. | T2 | 3 |
| RV-XCHG-021 | Proprietary formats through connectors and open formats | Formats without a published specification are not read or written directly. Their content enters through a connector running in the authoring application (RV-CONNECT-001), a registered licensed bridge (RV-CONNECT-018) or IFC, STEP, glTF or OpenUSD exported by the authoring tool. | T1 | 1 |
| RV-XCHG-022 | Sheet exports | Sheets and markups export to PDF, SVG, PNG and converted DWG or DXF releases (see [17-drawings.md](17-drawings.md)). | T1 | 1 |
