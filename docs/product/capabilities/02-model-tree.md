# Model tree, properties and external data

The model tree presents every model source of the federation as one navigable hierarchy of scene
items, and the property inspector shows the typed data attached to each item. Users can add their
own properties and link external tables; both are stored in the review document and never written
to the sources.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Scene item | Item key, kind (source root, container, object, geometry, instance), name, source type, bounding box | Child of one parent item; carries property sets; referenced by sets, viewpoints, issues and results |
| Item key | Model source id, source element identifier (IFC GlobalId, DWG handle, STEP product id) or derived path hash, key quality (native or derived) | Identifies one scene item across refreshes |
| Property set | Name, origin (source, user, external link, derived), properties | Attached to scene items |
| Property | Name, value type (text, integer, real, boolean, date, quantity with unit, URI), value | Belongs to one property set |
| User property | Name, type, value, author, date | Stored in the review document; attached to an item key |
| External data link | Name, connection (file or database), table or query, key property, key column, refresh policy | Produces one external property set on matching items |
| Tree view mode | Full, compact, grouped by property, sets | Presentation of the model tree |

## Hierarchy and tree

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-TREE-001 | Federated hierarchy | The model tree shows one root per model source and, under it, the hierarchy produced by the reader (for example site, building, storey, element, part for IFC; layer, block reference, entity for DWG; assembly, part, body for STEP). Each node shows an icon for its kind and its name. | T1 | 1 |
| RV-TREE-002 | Tree selection | Clicking a node selects the scene item and its descendants in the scene; modifier keys extend, toggle and range-select. The tree scrolls to and expands the items selected in the scene. | T1 | 1 |
| RV-TREE-003 | Compact mode | Compact mode hides nodes below a configurable depth and merges single-child chains so the tree shows meaningful objects only; the depth setting is stored per document. | T2 | 1 |
| RV-TREE-004 | Group by property | The tree can be regrouped by the value of a chosen property (for example storey, category, system or material), producing one branch per distinct value; items without the property go to an explicit unassigned branch. | T2 | 1 |
| RV-TREE-005 | Sets mode | Sets mode shows the selection sets and search sets of the document as a tree whose leaves expand to their current member items. | T2 | 1 |
| RV-TREE-006 | Tree context actions | A node's context menu offers select, hide, show only, require, override appearance, zoom to, save as set, add attachment link and copy item key; each action is also a command with a keyboard shortcut. | T1 | 1 |
| RV-TREE-007 | Virtualized large trees | The tree renders only visible rows and expands lazily, so a federation with millions of scene items scrolls and filters at interactive rates. | T1 | 1 |
| RV-TREE-008 | Filter the tree | A filter field narrows the tree to nodes whose name or chosen property contains the text, keeping their ancestors visible. | T1 | 1 |
| RV-TREE-009 | Accessible tree | The tree is fully operable from the keyboard (move, expand, collapse, select, invoke context actions) and exposes each node's name, kind, state and level to screen readers. | T1 | 1 |
| RV-TREE-010 | Visibility state markers | Each node shows whether it is hidden, required or overridden, and a partially hidden branch is marked distinctly from a fully hidden one. | T1 | 1 |
| RV-TREE-011 | Grids and levels | Grids and levels read from sources are listed as navigable datum objects; picking a level offers to frame the view on it or section at it, and the nearest grid intersection and level are available to measurements, clash results and issues. | T2 | 1 |

## Identity

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-TREE-012 | Stable item keys | Every scene item has an item key built from the model source id and the source's own element identifier. Selection sets, viewpoints, comments, issues, clash results, tasks and takeoff items refer to items by key, so they reattach after a refresh. | T1 | 1 |
| RV-TREE-013 | Derived keys and key quality | When a source has no element identifier, the key is derived from the item's path and name and is flagged as derived. The model source's information reports how many items have native and derived keys. | T1 | 1 |
| RV-TREE-014 | Key mapping on refresh | On refresh, items whose native identifier changed but whose path, type and geometry match within tolerance are proposed as the same item; the user can accept the mapping so review data follows. | T3 | 3 |
| RV-TREE-015 | Copy and locate by key | An item key can be copied as text and pasted into a locate command, which selects and frames the item if it exists in the federation. | T2 | 1 |

## Properties

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-TREE-016 | Property inspector | The inspector shows every property set of the selected item as a collapsible group with typed values; with several items selected it shows common properties and marks differing values as varied. | T1 | 1 |
| RV-TREE-017 | Typed values with units | Quantity properties are stored in source units and displayed in the document's display units with configurable precision; dates use the user's locale; booleans and enumerations show as text values. | T1 | 1 |
| RV-TREE-018 | Item and geometry properties | Every item also exposes derived properties: name, kind, source file, item key, layer or container, material, bounding box, and for geometry the triangle count, surface area and enclosed volume where the mesh is closed. | T1 | 1 |
| RV-TREE-019 | Type and instance data | Properties defined on an element type are shown in a separate set from instance properties, and the inspector can navigate from an instance to its type and to the other instances of that type. | T2 | 1 |
| RV-TREE-020 | URI properties | Properties holding a URL or file path are shown as links; following one opens it with the system handler after confirming any non-local target. | T2 | 1 |
| RV-TREE-021 | Hover properties | Hovering over an item in the scene shows a tooltip with a configurable list of properties (by property set and name); the list is a user setting and can be turned off. | T2 | 1 |
| RV-TREE-022 | Copy and export properties | The properties of the selected items can be copied as a table or exported to CSV or XLSX, with one row per item and one column per chosen property. | T2 | 1 |
| RV-TREE-023 | Property visibility rules | Property sets can be hidden from the inspector by name pattern; hidden sets remain available to queries. | T3 | 1 |
| RV-TREE-024 | Classification display | Classification references from sources (for example a Uniclass or OmniClass code) are shown as a dedicated property set with system, code and title; dictionary links resolve when a bSDD URI is present. | T2 | 3 |

## User-defined properties

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-TREE-025 | Add user properties | Users can add typed properties to one or many items under a user property set; values are stored in the review document against item keys, with author and date. | T2 | 1 |
| RV-TREE-026 | Bulk assign user properties | A user property value can be assigned to every member of a selection, selection set or search set in one command, and a CSV with item keys and values can be imported to assign many values at once. | T2 | 3 |
| RV-TREE-027 | User property protection | User properties are included in packaged and published review documents unless the publish profile excludes them; they are never written back to a source. | T2 | 1 |

## External data links

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-TREE-028 | Link an external table | An external data link joins rows of a CSV, XLSX or SQLite table, or of a database query through a configured driver, to scene items by matching a chosen item property to a key column. Matched columns appear as a read-only property set named after the link. | T2 | 3 |
| RV-TREE-029 | Typed link columns | Each linked column is given a type and optional unit so linked values can be used in queries, appearance rules, clash rules and takeoff formulas like source properties. | T2 | 3 |
| RV-TREE-030 | Refresh links | Links refresh on open, on demand or on an interval; the link status shows the last refresh time, matched and unmatched item counts and any connection error. | T2 | 3 |
| RV-TREE-031 | Test and manage links | Link definitions can be added, edited, tested against the data source with a match preview, disabled and deleted. | T2 | 3 |
| RV-TREE-032 | Exchange link definitions | Link definitions can be exported to and imported from a definition file; credentials are never stored in the file or the review document and are requested from the system credential store. | T2 | 3 |
