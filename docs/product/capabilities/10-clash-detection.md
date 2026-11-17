# Clash detection

Clash detection finds geometric conflicts between scene items of a federation: intersections,
insufficient clearances and duplicated items. Tests, rules, runs and results are review data stored
in the review document; results keep a stable identity across model revisions so that coordination
can follow one conflict from first detection to resolution. The test and result data have a
published JSON schema ([ADR 0004](../../adr/0004-clash-identity-and-open-result-format.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Clash test | Name, side A and side B (each a query, search set, selection set or fixed selection), self-test flag, geometry kinds per side (surfaces, lines, points), type (hard, hard conservative, clearance, duplicate), tolerance, enabled rules, hidden-item policy, optional simulation link and step, discipline-pair tag | Owns clash runs and clash results; references sets and clash rules; may link to a simulation (see [13-sequencing.md](13-sequencing.md)) |
| Clash rule | Kind (built-in template or custom query rule), parameters, enabled flag | Referenced by clash tests; custom rules use the shared query language |
| Clash run | Run number, start and end time, revision of each model source involved, settings snapshot, counts per status, duration | Belongs to one clash test; results record the first and last run that found them |
| Clash result | Result key, item A key, item B key, contact point, penetration or gap distance, nearest grid intersection and level, status, assignee, approver, due date, description, first found run, last found run, viewpoint, comment thread | Belongs to a clash test; may belong to one clash group; may be referenced by issues |
| Clash group | Name, grouping origin (manual or automatic rule), status, assignee, member results, representative viewpoint | Groups results of one test; may be promoted to an issue |
| Grouping rule | Ordered criteria (item, system, level, grid cell, space, model source, property value, distance band), sort | Applied to the results of one test |
| Discipline-pair matrix | Rows and columns of discipline sets, per-cell test template, tolerance and type | Generates and maintains clash tests |
| Clash report definition | Scope (tests, statuses, groups), sections, columns, image settings, output format | Produces report files from current results |

## Test definition

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-001 | Clash test list | The review document holds any number of named clash tests, each showing its last run time, result counts per status and a freshness state (never run, current, outdated because a model source or setting changed since the last run, partially run). Tests can be created, renamed, duplicated, reordered, disabled and deleted as undoable commands. | T1 | 3 |
| RV-CLASH-002 | Side A and side B | Each side of a test takes a query, a search set, a selection set or the current selection at definition time. Query and search-set sides are re-evaluated at every run, so items added to the federation by a source update join the test automatically. | T1 | 3 |
| RV-CLASH-003 | Self-test | A test with the self-test flag checks side A against itself and skips pairs that are the same scene item or ancestor and descendant. | T1 | 3 |
| RV-CLASH-004 | Geometry kinds per side | Each side states which geometry kinds take part: surfaces, lines, points, in any combination. Point geometry from point clouds can be tested against surfaces (see [19-reality-data.md](19-reality-data.md) for point cloud loading). | T2 | 3 |
| RV-CLASH-005 | Pick level for tests | Each side resolves items at a chosen pick level (whole object, composite, geometry part), which decides what one clash result refers to. The default follows the document's pick level setting. | T2 | 3 |
| RV-CLASH-006 | Hidden-item policy | A test states whether hidden scene items take part and, when results are displayed, whether items hidden by a rule are shown, left unchanged or kept hidden. | T2 | 3 |
| RV-CLASH-007 | Test templates | A test definition without its results can be saved as a template in a user or project library and instantiated in another review document; references to sets are resolved by set name and reported when missing. | T2 | 3 |
| RV-CLASH-008 | Import and export of tests | Tests and rules are exported to and imported from the documented JSON test format. Importing merges by test name with a choice to replace or keep both. | T2 | 3 |

## Clash types and tolerance

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-009 | Hard clash | Reports pairs whose tessellated geometry intersects with a penetration depth greater than the tolerance. The result stores the penetration depth as a negative distance and a contact point on the intersection. | T1 | 3 |
| RV-CLASH-010 | Hard conservative clash | Reports pairs whose bounding volumes and triangle sets cannot be proven disjoint, including coincident and touching faces and closed shells fully inside one another. It finds containment cases that the hard test misses at the cost of more results; results are marked with the type that produced them. | T2 | 3 |
| RV-CLASH-011 | Clearance clash | Reports pairs whose minimum distance is smaller than the tolerance, including intersecting pairs. The result stores the measured gap (or penetration) and the two closest points. | T1 | 3 |
| RV-CLASH-012 | Duplicate detection | Reports pairs of items with matching geometry within the position tolerance and, optionally, matching type or a chosen property. It is used to find items modeled twice or loaded twice from overlapping sources. | T1 | 3 |
| RV-CLASH-013 | Tolerance | The tolerance is a typed length in display units stored in metres. For hard tests it ignores penetrations up to the tolerance; for clearance tests it is the required gap; for duplicate tests it is the position deviation allowed. | T1 | 3 |
| RV-CLASH-014 | Exact solid refinement | For pairs where both items carry exact solid or analytic geometry, an optional refinement step confirms the tessellated result against the exact geometry and discards contacts caused only by tessellation error. | T3 | 5 |
| RV-CLASH-015 | Insulation and envelope offset | A side can be inflated by a per-item offset taken from a property (for example insulation thickness or maintenance space) so that clearance needs modeled only as data are tested as geometry. | T3 | 3 |

## Rules and noise reduction

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-016 | Built-in exclusion templates | Tests can enable exclusion rules that skip pairs which share the same model source, the same layer, the same source group or block, the same composite object, or that stand in an ancestor and descendant relation. Each rule's effect is counted in the run summary. | T1 | 3 |
| RV-CLASH-017 | Custom query rules | A custom rule excludes pairs where item A matches one query and item B matches another, or where both share or differ in a named property value (for example same system name). Rules are written in the shared query language and are reusable across tests. | T2 | 3 |
| RV-CLASH-018 | Previously found pairs | A rule skips pairs of composite objects already reported by an earlier test in the same batch, so overlapping tests do not report the same conflict twice. | T2 | 3 |
| RV-CLASH-019 | Hosted and connected items | A rule skips pairs that the source declares as connected or hosted (openings in walls, fittings on pipes, fasteners on members) when the source carries those relationships. | T2 | 3 |
| RV-CLASH-020 | Duplicate result suppression | Results whose item pairs are the same element seen through two pick levels, or whose contact points fall within one tolerance of an existing result between the same two objects, are merged into one result that lists all contact points. | T2 | 3 |
| RV-CLASH-021 | Rule import and export | Clash rules are exported to and imported from the documented JSON format independently of tests. | T2 | 3 |

## Running tests

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-022 | Run one or all tests | A run executes selected tests or all enabled tests in sequence on background threads with progress, an estimated pair count and cancellation. A cancelled run leaves the previous results unchanged. | T1 | 3 |
| RV-CLASH-023 | Run summary | Each completed run records counts of new, still found, not found and suppressed results, excluded pairs per rule, the revision of every model source used and the duration. | T1 | 3 |
| RV-CLASH-024 | Incremental re-run | When only some model sources changed since the last run, the run retests only pairs involving changed or added items and carries forward the results between unchanged items. The run summary states that it was incremental. | T2 | 3 |
| RV-CLASH-025 | Automatic run on source update | A test can be set to run automatically when any model source it involves is refreshed to a new version, locally after a refresh or as a Hub job on a new published version (see [24-hub-integration.md](24-hub-integration.md)). | T2 | 3 |
| RV-CLASH-026 | Headless runs | The command-line tool runs tests of a review document without a window, updates the document and writes reports, with a non-zero exit code when the count of new or active results exceeds a given threshold. | T2 | 3 |
| RV-CLASH-027 | Time-based clash | A test linked to a 4D simulation is evaluated at each simulation step using only the items present at that time, including temporary items; each result records the step and the tasks involved. | T2 | 3 |
| RV-CLASH-028 | Animation-linked clash | A test linked to an object animation is evaluated at a chosen time step along the animation; each result records the time at which the moving items first meet. | T3 | 5 |

## Result identity and lifecycle

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-029 | Stable result identity | A result's key is derived from the item keys of its pair and the test, so a re-run matches each found conflict to its existing result even when the source files were re-exported. When item keys are missing a fallback match uses item path and contact location, and the match method is recorded on the result. | T1 | 3 |
| RV-CLASH-030 | Status lifecycle | Results carry a status of new, active, reviewed, approved or resolved, set manually or by a run. Status changes are commands recorded with author and time. | T1 | 3 |
| RV-CLASH-031 | Re-run status transitions | On re-run, a newly found pair becomes new; a found pair keeps its manual status, assignee and comments and updates its distance; a previously found pair that is no longer found becomes resolved with the run number recorded; a resolved pair that is found again becomes active and is flagged as reopened. Approved results that are found again stay approved. | T1 | 3 |
| RV-CLASH-032 | Result history | Each result keeps a history of the runs that found it with distance per run, every status change, assignment and comment, so its development across model revisions is visible in one list. | T2 | 3 |
| RV-CLASH-033 | Assignment and approval | Results and groups can be assigned to a person or company from the document's participant list and given a due date; the approver and approval time are recorded when a result is approved. | T1 | 3 |
| RV-CLASH-034 | Comments on results | Each result and group has a comment thread (see [09-markup.md](09-markup.md)); comments are included in reports and exported issues. | T1 | 3 |
| RV-CLASH-035 | Include or exclude results | A result can be marked as excluded with a reason; excluded results do not count toward totals and are not reopened by re-runs while their pair persists. | T2 | 3 |

## Grouping

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-036 | Manual groups | Selected results can be combined into a named group with its own status, assignee and viewpoint; group status changes can optionally apply to all members. | T1 | 3 |
| RV-CLASH-037 | Automatic grouping | A grouping rule groups results by ordered criteria: item on side A or B, system, level, grid cell, space, model source, layer, property value, status, assignee or distance band. Groups are recomputed after each run while manual groups are preserved. | T2 | 3 |
| RV-CLASH-038 | Element-centric grouping | A grouping mode collects all results that share one item (for example one beam against many ducts) into one group named after that item, so one design change resolves one group. | T2 | 3 |
| RV-CLASH-039 | Spatial clustering | A grouping mode clusters results whose contact points lie within a given distance of each other into one group, with the cluster bounding box as the group viewpoint extent. | T3 | 3 |
| RV-CLASH-040 | Grid and level location | Each result is located at the nearest grid intersection and the level below its contact point when grids and levels are present in the federation, and these locations are available for sorting, grouping and reports. | T2 | 3 |

## Review of results

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-041 | Results table | Results and groups appear in a virtualized table with sortable, filterable columns (name, status, distance, type, items, location, assignee, found date, run) and remain responsive with one million results. | T1 | 3 |
| RV-CLASH-042 | Result display | Selecting a result highlights item A and item B in two distinct colors, frames the camera on the contact point and optionally dims, makes transparent or hides all other items. These display options persist per user. | T1 | 3 |
| RV-CLASH-043 | Result viewpoint | Each result stores a viewpoint captured at first display or on demand, including camera, visibility, section box around the contact and highlighting; the viewpoint is used in reports and issues. | T1 | 3 |
| RV-CLASH-044 | Step-through review | Keyboard commands move to the next and previous result in the current sort and filter order and apply a status in one keystroke. | T2 | 3 |
| RV-CLASH-045 | Reselect clash items | A command selects the items of the chosen results in the scene and the model tree, so they can be hidden, measured or added to sets (selection behavior in [03-selection-search.md](03-selection-search.md)). | T1 | 3 |
| RV-CLASH-046 | Item properties side by side | The result panel lists the properties of item A and item B in two columns with differing values marked. | T2 | 3 |

## Coordination at scale

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-047 | Discipline-pair matrix | A matrix of discipline sets generates one clash test per enabled cell with the cell's type and tolerance, shows result counts per cell and keeps the generated tests in step when the matrix changes. | T2 | 3 |
| RV-CLASH-048 | Clash dashboard | A dashboard charts result counts per status, test, discipline pair, assignee and level over the sequence of runs, and lists results opened, resolved and reopened since a chosen run. Charts export as PNG and their data as CSV. | T2 | 3 |
| RV-CLASH-049 | Promote to issue | A result or group can be promoted to an issue that carries its viewpoint, the item keys of both sides as components and a reference back to the result; the result shows the linked issue and its status (see [12-issues-bcf.md](12-issues-bcf.md)). | T1 | 3 |

## Reports and round trip

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CLASH-050 | Report definition | A report definition selects tests, groups and statuses, the sections (summary, per-result details, images) and the columns (location, distance, dates, assignee, properties of each item). Definitions are saved in the review document and reusable from the command line. | T1 | 3 |
| RV-CLASH-051 | Report formats | Reports are written as HTML with images, PDF, CSV, XLSX, JSON in the documented result schema and BCF files with one topic per result or group. One report can cover the current test, all tests combined or each test separately. | T1 | 3 |
| RV-CLASH-052 | Results as viewpoints | A command writes the selected results as a viewpoint folder in the review document, one viewpoint per result with its comments, for sharing through published packages. | T2 | 3 |
| RV-CLASH-053 | Open result format | Tests, runs and results export to and import from the published JSON schema with item keys, so external tools can analyze results and other clash engines can deliver results for review in Arc Review. | T2 | 3 |
| RV-CLASH-054 | Select in authoring application | From a result, the item keys of both items are sent to a running Arc application through the Arc Automation Protocol, which selects and frames those elements; for other applications the keys are exported as an element identifier list or as BCF components. | T2 | 3 |
| RV-CLASH-055 | Hub issue from result | With a Hub connection, promoting a result creates the issue on the Hub project so that authors see it in their applications; status changes on the Hub are reflected on the result. | T2 | 3 |
