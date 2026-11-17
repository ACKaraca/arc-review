# Model checking

Model checking evaluates the federation against rules about information and geometry: required
properties, classification, naming, spatial and clearance rules and model quality. It complements
clash detection with semantic checks and validates models against IDS information requirements.
Rule sets, runs and findings are review data; checking runs headless for use in continuous
integration.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Check rule set | Name, version, description, project phase, ordered rules, parameter values | Contains check rules; may be loaded from a library or an IDS file |
| Check rule | Rule kind (from the rule library), applicability query, parameters, severity, message template | Belongs to a rule set; produces check findings |
| Information requirement | IDS specification: name, applicability facets, requirement facets (entity, attribute, property, classification, material, part-of), cardinality, IFC version | Imported from IDS; evaluated as rules of a rule set |
| Check run | Rule set version, model source revisions, start time, duration, counts per severity | Owns check findings |
| Check finding | Finding key (rule plus item key), severity, message, failing values, status, assignee, first and last run, viewpoint, comment thread | Belongs to a check run; references scene items; may be promoted to an issue |
| Classification system | Name, edition, source (file, bSDD dictionary), code tree | Contains classification references |
| Classification reference | Code, title, parent, mapped properties | Assigned to scene items in the source or by a mapping rule in the review document |
| Classification mapping | Query-to-code rules, target system | Derives classification references for items lacking them |

## Rule sets and rule library

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CHECK-001 | Rule sets | The review document holds named, versioned rule sets with ordered rules and per-rule parameters, severity (error, warning, information) and enabled flag. Rule sets are created, edited, duplicated and deleted as undoable commands. | T1 | 3 |
| RV-CHECK-002 | Rule library | A built-in library offers parameterized rule kinds; a rule is created by choosing a kind, an applicability query and parameter values. The library is extensible by plug-ins (see [25-automation.md](25-automation.md)). | T1 | 3 |
| RV-CHECK-003 | Rule set exchange | Rule sets are exported to and imported from a documented JSON format and kept in user and project libraries, so a project applies one rule set per phase across documents. | T2 | 3 |
| RV-CHECK-004 | Property presence and value rules | A rule requires that items matching the applicability query carry a property, optionally with a value type, a unit dimension, an allowed value list, a numeric range or a pattern. Each failing item yields one finding naming the property and the found value. | T1 | 3 |
| RV-CHECK-005 | Naming convention rules | A rule checks item names, type names, layer names or any text property against a pattern with named fields (for example discipline code, level code, sequence), and reports which field failed. | T2 | 3 |
| RV-CHECK-006 | Uniqueness rules | A rule reports items whose identifier or chosen property value is not unique within a scope (model source, level, federation), including duplicated IFC GlobalIds across sources. | T1 | 3 |
| RV-CHECK-007 | Relationship rules | A rule requires that items are contained in a spatial element (site, building, storey, space), belong to a system, are hosted by a given kind of item, or that a property value agrees with the same property of the container. | T2 | 3 |
| RV-CHECK-008 | Clearance and free space rules | A rule requires a minimum free volume in front of or around items (for example access space in front of equipment or doors), defined by an offset box relative to the item's orientation, and reports intruding items. | T2 | 3 |
| RV-CHECK-009 | Space and area rules | A rule checks spaces for minimum area, minimum dimension, height, enclosure by bounding elements and overlap with other spaces, and compares computed area with the area property in the source. | T2 | 3 |
| RV-CHECK-010 | Accessibility geometry rules | Parameterized rules check geometric accessibility values: clear door width, ramp slope, landing size, turning circle in a space, stair riser and tread limits. Parameters are set per project; no jurisdiction's values are built in as defaults. | T3 | 3 |
| RV-CHECK-011 | Egress distance rule | A rule computes walking distance from points in spaces to the nearest exit door through the space and door graph and reports spaces exceeding a limit. | T3 | 5 |
| RV-CHECK-012 | Model quality rules | Rules report items with no geometry, degenerate or open geometry, items far from the federation's main extent, items without a container, zero-size items, and overlapping items of the same type. | T1 | 3 |
| RV-CHECK-013 | Comparative value rules | A rule compares two properties of one item, or a property with a measured geometric value (length, area, volume, height), within a tolerance, and reports disagreements. | T2 | 3 |
| RV-CHECK-014 | Scripted rules | A rule kind delegates evaluation to a sandboxed plug-in that receives the applicable items and returns findings, so organizations add rules without changing the application. | T3 | 3 |

## Information requirements and standards

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CHECK-015 | IDS import | IDS 1.0 files are imported as information requirement rule sets; every facet kind and cardinality is supported, and unsupported content is reported rather than ignored. | T1 | 3 |
| RV-CHECK-016 | IDS validation | Validation evaluates each IDS specification against IFC-based model sources and reports, per specification, the applicable items, passing items and failing items with the failing facet and value. | T1 | 3 |
| RV-CHECK-017 | IDS audit report | An audit report summarizes pass and fail counts per specification and lists failures; it is written as HTML, PDF, CSV, JSON and BCF. | T1 | 3 |
| RV-CHECK-018 | IDS authoring | Information requirements can be created and edited in the application and exported as valid IDS 1.0 files, validated against the IDS schema before writing. | T2 | 3 |
| RV-CHECK-019 | bSDD lookup | Property and classification names in rules and mappings can be looked up in buildingSMART Data Dictionary dictionaries when online; chosen entries store their URI so rules remain valid offline. | T2 | 3 |
| RV-CHECK-020 | COBie validation | A built-in rule set checks handover data for the presence and consistency of COBie-required information (facility, floors, spaces, zones, types, components, systems) and exports the COBie spreadsheet derived from the federation. | T2 | 3 |

## Classification

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CHECK-021 | Classification systems | Classification systems are loaded from documented CSV or JSON code tables or from bSDD dictionaries, and browsed as code trees with item counts per code. | T2 | 3 |
| RV-CHECK-022 | Classification mapping | Mapping rules assign classification references to items by query; mapped references are stored in the review document as derived properties and never written to the sources. Conflicts between source and mapped codes are reported. | T2 | 3 |
| RV-CHECK-023 | Classification checks | Rules require that applicable items carry a reference from a given system, that codes exist in the system edition, and that codes agree with the item's entity type through a mapping table. | T2 | 3 |
| RV-CHECK-024 | Information takeoff tables | A table definition lists items grouped by classification or any property with chosen columns and aggregated quantities, refreshed after each source update and exported to CSV and XLSX (quantity workflows in [15-takeoff.md](15-takeoff.md)). | T2 | 3 |

## Runs and findings

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CHECK-025 | Run checks | A check run evaluates selected rule sets over the federation on background threads with progress and cancellation and records the rule set version and every model source revision. | T1 | 3 |
| RV-CHECK-026 | Findings list | Findings appear in a filterable table by rule, severity, status, item and model source; selecting a finding frames and highlights the failing items and shows the failing values. | T1 | 3 |
| RV-CHECK-027 | Finding identity across runs | A finding's key is derived from the rule and the item key, so re-running keeps status, assignee and comments of persisting findings, marks absent findings resolved and flags returning findings as reopened, with the same transitions as clash results. | T1 | 3 |
| RV-CHECK-028 | Finding statuses and acceptance | Findings carry the review statuses of clash results; a finding can be accepted as an approved exception with a reason, which suppresses it in later runs while the failing value is unchanged. | T2 | 3 |
| RV-CHECK-029 | Grouping of findings | Findings are grouped by rule, item type, level, model source or failing value, so one systematic omission is handled as one group. | T2 | 3 |
| RV-CHECK-030 | Promote finding to issue | A finding or group can be promoted to an issue with a viewpoint, item keys as components and the rule message as description (see [12-issues-bcf.md](12-issues-bcf.md)). | T1 | 3 |
| RV-CHECK-031 | Check report | A report summarizes findings per rule set, rule and severity with trend against earlier runs and is written as HTML, PDF, CSV, XLSX, JSON and BCF. | T1 | 3 |
| RV-CHECK-032 | Headless checking in CI | The command-line tool validates model files against a rule set or IDS file without a review document and returns a non-zero exit code when errors are found, writing the report in a chosen format for continuous integration pipelines. | T1 | 3 |
| RV-CHECK-033 | Automatic checks on source update | Rule sets can run automatically when a model source is refreshed or when a new version is published on a Hub, with results visible in the check dashboard. | T2 | 3 |
| RV-CHECK-034 | Check dashboard | A dashboard charts findings per rule, severity, model source and status over runs, so information quality can be tracked toward a delivery milestone. | T2 | 3 |
