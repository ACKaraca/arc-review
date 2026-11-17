# 4D sequencing

4D sequencing links a construction schedule to the federation so that the model can be played
through time: what is built, demolished or temporarily present on any date, planned against actual.
Schedules are imported from scheduling files and remain linked for re-synchronization; tasks,
attachments and simulation settings are review data.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Schedule | Name, scenario label, calendar, time zone, data date | Contains tasks; has zero or more schedule sources |
| Schedule source | Kind (MSPDI, P6 XER, P6 XML, CSV, XLSX, Hub link), file reference and checksum, field mapping, synchronization options, last synchronized time | Feeds one schedule |
| Task | Source task id, name, hierarchy position, planned start and end, actual start and end, percent complete, task type, active flag, attached items, cost fields, user columns | Belongs to a schedule; attaches scene items through sets, selections or attachment rules; may link to an animation |
| Task type | Name, appearance at start, during and end, appearance when early or late, end state (visible, hidden, model appearance) | Assigned to tasks |
| Attachment rule | Task field or pattern, query template over item properties, matching options | Attaches items to tasks |
| Simulation | Mode (planned, actual, planned against actual), start and end, interval, playback duration, overlay definition, camera source | Plays one schedule |
| Progress record | Date, task, percent complete or actual dates, author, optional photo or note | Updates task actuals |

## Schedule data

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SEQ-001 | Task table | Tasks appear as a hierarchical table with name, status, planned and actual dates, type, attachment count and cost, together with a Gantt chart on a shared time axis. Columns can be shown, hidden, sorted and filtered. | T1 | 3 |
| RV-SEQ-002 | Task editing | Tasks can be added, nested, indented, outdented, reordered, activated or deactivated and deleted, and their dates edited, as undoable commands. Edits to tasks from a linked source are flagged as local overrides. | T1 | 3 |
| RV-SEQ-003 | Task types | The document defines task types with appearance at start, during and end and for early or late states; construct, demolish and temporary types are provided initially and further types can be added. | T1 | 3 |
| RV-SEQ-004 | Task status | Each task's status at the simulation date is derived from planned and actual dates: not started, in progress, complete, and early, on time or late against plan. | T1 | 3 |
| RV-SEQ-005 | Schedule import | Schedules are imported from Microsoft Project XML (MSPDI), Primavera P6 XER and P6 XML, and CSV or XLSX with a field mapping. Task hierarchy, dates, calendars, percent complete and custom fields are read; unsupported content is listed in an import report. | T1 | 3 |
| RV-SEQ-006 | Field mapping | Each source has a mapping from source fields to task fields and user columns, saved with the source and reusable as a template. | T1 | 3 |
| RV-SEQ-007 | Synchronize with source | Synchronization re-reads a changed source, updates task dates and fields by source task id, adds and removes tasks, and keeps local attachments and costs; options choose whether local overrides are kept or replaced. A summary lists added, removed and changed tasks. | T1 | 3 |
| RV-SEQ-008 | Generate tasks from model | Tasks can be generated from the model tree structure, from sets or from distinct values of a property (for example one task per level and category), each attached to its items. | T2 | 3 |
| RV-SEQ-009 | Schedule export | Tasks with dates and attachments are exported to CSV, XLSX and Microsoft Project XML. | T2 | 3 |

## Attaching the model

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SEQ-010 | Manual attachment | The current selection, a selection set or a search set can be attached to tasks; set-based attachments are re-evaluated so they follow source updates. | T1 | 3 |
| RV-SEQ-011 | Attachment rules | Rules attach items to tasks by matching a task field (name, code, user column) to item properties through a query template, for example a task code to a zone property. Rules run on demand and after synchronization and report each task's attachment count. | T1 | 3 |
| RV-SEQ-012 | Coverage validation | A coverage report lists tasks without attached items, items without a task, items attached to more than one construct task, and tasks whose attachments changed since the last run; each entry selects the items concerned. | T1 | 3 |
| RV-SEQ-013 | Attachment display | Selecting a task highlights its items; selecting an item lists its tasks. | T1 | 3 |

## Simulation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SEQ-014 | Playback | The simulation plays, pauses, steps and scrubs between start and end at the chosen interval (hours to months) and playback duration; the scene shows each item with the appearance of its task's type and state at the current date. | T1 | 3 |
| RV-SEQ-015 | Simulation modes | Modes show planned dates, actual dates, or planned against actual with early and late states colored by the task type appearance and the delay visible as a ghost of planned work. | T1 | 3 |
| RV-SEQ-016 | Overlay text | An overlay shows the current date, week or day number, active tasks, cumulative cost and any task column, with position, font size and color settings. | T2 | 3 |
| RV-SEQ-017 | Camera during simulation | The simulation can keep the current camera, follow a viewpoint animation or switch to task-linked viewpoints. | T2 | 3 |
| RV-SEQ-018 | Task-linked animation | A task can trigger an object animation (for example a crane lift) that plays during the task's duration (animations in [07-viewpoints-animation.md](07-viewpoints-animation.md)). | T2 | 3 |
| RV-SEQ-019 | Simulation video export | The simulation is exported as an image sequence or MP4 video at a chosen size and frame rate using the viewport renderer. | T1 | 3 |
| RV-SEQ-020 | Rendered simulation video | Frames can be rendered photorealistically through RenderBuilder over the Arc Scene contract (see [20-presentation.md](20-presentation.md)). | T3 | 4 |
| RV-SEQ-021 | Time-based clash link | A clash test can be linked to the simulation so that conflicts between items present at the same time, including temporary items, are found (see [10-clash-detection.md](10-clash-detection.md)). | T2 | 3 |

## Progress and scenarios

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SEQ-022 | Progress recording | Actual start, actual end and percent complete can be recorded per task or per item selection for a data date, with optional photo and note; records are kept as a history. | T2 | 3 |
| RV-SEQ-023 | Delay overview | A view lists tasks late against plan at the data date with the delay in days, colors their items and exports the list. | T2 | 3 |
| RV-SEQ-024 | Schedule scenarios | A document can hold several schedules as scenarios; two scenarios play side by side in synchronized split views with a list of tasks whose dates differ. | T3 | 3 |
| RV-SEQ-025 | Resource-loaded tasks | Tasks can carry resource quantities (crews, equipment) imported from the schedule source, shown as histograms over time. | T3 | 5 |
| RV-SEQ-026 | Location-based flowline view | A flowline chart plots tasks by location (level or zone) against time, derived from task attachments. | T3 | 5 |
| RV-SEQ-027 | Site logistics items | Temporary items without source geometry (cranes, hoarding, laydown areas) can be placed from a library as review-only geometry and attached to temporary tasks. | T3 | 5 |
