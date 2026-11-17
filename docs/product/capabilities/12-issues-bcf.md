# Issues and BCF

Issues are the unit of coordination: each records a problem, a location in the model, the items
concerned, a responsible party and a discussion until it is closed. The issue model is the BCF topic
model, so issues travel to any BCF-capable tool without conversion and synchronize with Arc Hub
through the BCF API ([ADR 0005](../../adr/0005-bcf-native-issue-model.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Issue | GUID, number, title, description, type, status, priority, stage, labels, assignee, due date, creation and modification author and time, server-assigned index | Has comments and issue viewpoints; references scene items by item key; may reference clash results, clash groups, check findings, markups or sheets; may relate to other issues |
| Issue comment | GUID, author, time, text, optional viewpoint reference | Belongs to one issue |
| Issue viewpoint | GUID, camera (perspective or orthogonal), components (selection, visibility exceptions, coloring), clipping planes, line markups, bitmaps, snapshot image | Belongs to an issue; can be opened as a review viewpoint |
| Issue board | Name, filter, grouping, columns | Displays issues of the document or a Hub project |
| Project issue settings | Allowed types, statuses, priorities, labels, stages, participants | Constrain issue fields; imported from BCF project extensions or a Hub project |
| Issue template | Name, preset fields, description text | Used when creating issues |

## Issue authoring

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ISSUE-001 | Create issue | Creating an issue captures the current camera, visibility, coloring, section planes and a snapshot image as its first viewpoint and the selected items as components. Title is required; other fields default from project settings. | T1 | 2 |
| RV-ISSUE-002 | Issue fields | Issues carry type, status, priority, stage, labels, assignee and due date whose allowed values come from the project issue settings; values outside the allowed lists are rejected with a message. | T1 | 2 |
| RV-ISSUE-003 | Issue placement pin | An issue can be pinned at a picked point on a surface or a sheet; pins appear in the viewport and on sheets with their number and status color and open the issue when clicked. | T1 | 2 |
| RV-ISSUE-004 | Additional viewpoints | Further viewpoints can be added to an issue at any time, and comments can refer to a specific viewpoint. | T1 | 2 |
| RV-ISSUE-005 | Markup in issue snapshots | Line markups and text drawn on the view are stored with the issue viewpoint as BCF line markups and in the snapshot image. | T2 | 2 |
| RV-ISSUE-006 | Comments | Issues have a chronological comment list; editing or deleting a comment is limited to its author and recorded in the issue history. | T1 | 2 |
| RV-ISSUE-007 | Related issues and references | An issue can relate to other issues and reference documents, sheets, markups or external URLs; references open in the application where possible. | T2 | 2 |
| RV-ISSUE-008 | Issue templates | Templates prefill type, priority, labels and description structure for recurring issue kinds. | T2 | 2 |
| RV-ISSUE-009 | Issue history | Every field change, comment and viewpoint addition is recorded as an issue event with author and time and is listed in the issue's history. | T1 | 2 |

## Issue management

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ISSUE-010 | Issue list | Issues appear in a virtualized, sortable and filterable list by any field, including overdue, assigned to me and changed since a date; filters are saved per user. | T1 | 2 |
| RV-ISSUE-011 | Open issue in model | Opening an issue restores its viewpoint: camera, components visibility and coloring, clipping planes and markups, resolving components by item key; components that cannot be found in the current federation are listed. | T1 | 2 |
| RV-ISSUE-012 | Issue board | A board view groups issues in columns by status, assignee, priority or label; moving a card changes the field through a command. | T2 | 2 |
| RV-ISSUE-013 | Bulk edit | A field change applies to all selected issues in one undoable command. | T2 | 2 |
| RV-ISSUE-014 | Issue report | A report of selected issues with snapshots, fields and comments is written as PDF, HTML, CSV and XLSX. | T1 | 2 |
| RV-ISSUE-015 | Issue statistics | Charts show open and closed issues over time, by assignee, type and priority, and the average age of open issues. | T2 | 2 |

## BCF exchange

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ISSUE-016 | BCF file import | BCF 2.1 and 3.0 files are imported with topics, comments, viewpoints, snapshots, document references and project extensions; import matches existing issues by GUID and merges changes by modification time, keeping both versions of conflicting fields for the user to choose. | T1 | 2 |
| RV-ISSUE-017 | BCF file export | Selected or filtered issues are exported to BCF 2.1 or 3.0 files; exported files validate against the BCF schemas. Content that BCF 2.1 cannot hold is listed in an export report. | T1 | 2 |
| RV-ISSUE-018 | Component identity in BCF | Components are written with their IFC GlobalId when the source provides one and with an authoring tool identifier otherwise, so tools reading the BCF can select the same elements. | T1 | 2 |
| RV-ISSUE-019 | BCF API synchronization | With a server that implements the BCF API (including Arc Hub), issues synchronize in both directions; offline edits are queued and applied on reconnection, and conflicts keep both values for the user to resolve. | T1 | 2 |
| RV-ISSUE-020 | Project settings from server | Allowed types, statuses, priorities, labels, stages and participants are read from the server's project extensions and refreshed on synchronization. | T2 | 2 |

## Links to analysis results

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ISSUE-021 | Issues from clash results | Issues created from clash results or groups keep a reference to the result keys; the issue shows the current clash status, and resolving the clash in a later run adds a system comment to the issue. | T1 | 3 |
| RV-ISSUE-022 | Issues from check findings | Issues created from check findings keep the rule and finding keys and show whether the finding persists in the latest run. | T1 | 3 |
| RV-ISSUE-023 | Bulk issue creation | One issue per selected result group or finding group is created in one command with fields from a template and an assignee taken from a grouping criterion. | T2 | 3 |
| RV-ISSUE-024 | Issues in authoring applications | Through Arc Hub, issues are visible in other Arc applications, which select the referenced elements by item key; resolving an issue there updates its status here after synchronization. | T2 | 3 |
| RV-ISSUE-025 | Issues on sheets | Issues pinned on a sheet store the sheet reference and sheet coordinates in addition to the 3D viewpoint and open on the sheet (see [17-drawings.md](17-drawings.md)). | T2 | 2 |
| RV-ISSUE-026 | Issues in immersive sessions | Participants in an immersive session create issues with a location and snapshot that enter the same issue list (see [21-immersive-review.md](21-immersive-review.md)). | T3 | 5 |
