# Selection, sets and search

Selection is the starting point of almost every review action, so the product defines precisely what
a pick resolves to, how selections are stored and how items are found by their data. One query
language serves search, search sets, appearance rules, clash scopes, check rules, 4D attachment and
takeoff.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Selection | Ordered item keys, active item | Volatile; one per view; input to commands |
| Pick level | Source root, container, first object, last object, geometry | Setting that resolves a pick to a scene item |
| Query | Condition rows (property set, property, operator, value, unit), groups, AND/OR, scope | Evaluated against the model tree; stored by search sets and rules |
| Selection set | Name, item keys, folder, description | Static; referenced by clash tests, tasks, takeoff, appearance rules |
| Search set | Name, query, folder, description | Dynamic; re-evaluated on change; referenced like selection sets |
| Set folder | Name, children | Organizes sets in a tree |

## Picking and selection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SET-001 | Pick in the scene | Clicking geometry selects the scene item that the current pick level resolves to; modifiers add, toggle or remove items. Picking is exact against the rendered geometry, including sectioned and transparent items. | T1 | 1 |
| RV-SET-002 | Pick level | The pick level setting chooses whether a click resolves to the model source root, the nearest container, the first object below the container, the deepest object above geometry, or the geometry itself. The level is shown in the status bar and changes with a keyboard shortcut. | T1 | 1 |
| RV-SET-003 | Walk up and down the hierarchy | With an item selected, commands move the selection to its parent or first child, so a user can refine a pick without changing the pick level. | T2 | 1 |
| RV-SET-004 | Box and lasso selection | Dragging a rectangle or lasso selects items inside it; dragging in one direction selects only fully enclosed items and the other direction selects crossing items. | T1 | 1 |
| RV-SET-005 | Select all, none and invert | Commands select every visible item, clear the selection, or invert it within the visible scene. | T1 | 1 |
| RV-SET-006 | Select similar | Select similar extends the selection to every item that shares a chosen characteristic with the selected items: name, type, a chosen property value, material, layer or identical geometry. | T1 | 1 |
| RV-SET-007 | Selection list | A selection list shows the selected items with name, path and source; items can be removed from the selection, hidden or framed from the list. | T2 | 1 |
| RV-SET-008 | Selection highlight | Selected items are highlighted by a tint or outline whose color and intensity are user settings; the highlight remains visible through transparency and inside section boxes. | T1 | 1 |
| RV-SET-009 | Selection from review objects | Selecting a clash result, check finding, issue, task, takeoff item or difference record selects its associated scene items; each area provides this through the same selection command. | T1 | 1 |
| RV-SET-010 | Selection history | Previous selections are kept in a history so the user can step back and forward through them. | T3 | 1 |

## Find and query

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SET-011 | Quick find | A single text field searches item names and the values of a configured list of property sets and selects or lists the matches. Repeated invocation steps to the next match. | T1 | 1 |
| RV-SET-012 | Query builder | The query builder composes condition rows of property set, property, operator and value. Rows combine with AND and OR and can be nested in groups. | T1 | 1 |
| RV-SET-013 | Query operators | Operators include equals, not equals, contains, starts with, wildcard pattern, regular expression, defined, undefined, less than, greater than, between, and in list. Numeric and quantity comparisons are unit-aware; text comparisons can be case sensitive. | T1 | 1 |
| RV-SET-014 | Query scope | A query runs against the whole federation, the current selection, chosen model sources or the members of a set, and can include or exclude hidden items and limit results to a pick level or item kind. | T1 | 1 |
| RV-SET-015 | Query results | Running a query selects the matches or lists them with match count; find next and find previous move through the list and frame each match. | T1 | 1 |
| RV-SET-016 | Text query form | Every query has an equivalent text form that can be typed, copied, stored in scripts and reloaded into the builder without loss. | T2 | 1 |
| RV-SET-017 | Spatial conditions | Query conditions can test spatial relations: inside a section box or volume item, on a level, within a distance of a selection, or intersecting a given set. | T3 | 3 |
| RV-SET-018 | Queries over review data | Queries can test review-data attributes of items: member of a set, has comments, has open issues, involved in clash results, attached to a task, taken off. | T2 | 3 |

## Selection sets and search sets

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SET-019 | Save a selection set | Saving the current selection creates a selection set that stores the item keys at that moment. Its membership changes only by explicit add, remove or update commands. | T1 | 1 |
| RV-SET-020 | Save a search set | Saving a query creates a search set that stores the query. Its membership is re-evaluated whenever the federation is refreshed or the query changes, so it follows model updates. | T1 | 1 |
| RV-SET-021 | Edit sets | Sets can be renamed, described, duplicated, deleted, updated from the current selection or query, and have the current selection added to or removed from them. | T1 | 1 |
| RV-SET-022 | Set folders | Sets are organized in folders with drag and drop; folders can be nested and reordered. | T1 | 1 |
| RV-SET-023 | Sets of sets | A search set's query can reference other sets (member of set A and not member of set B), so composite sets are built from existing ones. Circular references are rejected when saved. | T2 | 1 |
| RV-SET-024 | Set membership status | Each set shows its member count and, for selection sets, how many stored item keys no longer resolve after a refresh. | T2 | 1 |
| RV-SET-025 | Use sets as inputs | Sets are accepted wherever a group of items is an input: visibility and appearance commands, appearance rules, clash scopes, check rule scopes, task attachment, takeoff, animation clips and interaction scripts. | T1 | 1 |
| RV-SET-026 | Export and import sets | Sets and folders can be exported to and imported from an open set file, so set libraries move between review documents and projects. Search sets travel as queries; selection sets travel as item keys. | T2 | 1 |
| RV-SET-027 | Generate sets by property | A command creates one search set per distinct value of a chosen property (for example one per storey or system) inside a new folder. | T2 | 1 |
| RV-SET-028 | Read-only sets | Sets can be shown and used in the read-only viewer mode but not created or changed there. | T1 | 1 |
