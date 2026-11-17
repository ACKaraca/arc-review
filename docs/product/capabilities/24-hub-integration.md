# Hub integration

Hub integration connects Arc Review to a self-hosted Arc Hub through the Hub API for model versions,
review documents, issues, markups and headless jobs. The connection is optional: every capability in
other areas works offline, and Hub adds sharing, history and automation.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Hub connection | Server URL, OpenID Connect session, user, selected project | One per desktop session; may be offline |
| Hub model source | Project, file, version, content hash | A model source kind; resolves to a downloaded file and scene cache |
| Offline queue | Pending uploads of issues, comments, markups and review documents | Replayed when the connection returns |
| Hub job | Job definition, trigger, worker, status, outputs | Runs `arc-review-cli` on a Hub worker |

## Connection and files

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-HUB-001 | Connect to a Hub | The user enters a Hub server URL and signs in through the Hub's OpenID Connect provider in the system browser; tokens are kept in the operating system's credential store. | T1 | 2 |
| RV-HUB-002 | Browse projects and files | A Hub browser lists projects, folders, files and versions with lifecycle state, and opens or appends any version. | T1 | 2 |
| RV-HUB-003 | Hub versions as model sources | A model source can reference a Hub file version or the latest version in a chosen lifecycle state; refresh offers newer versions and records which version each scene cache came from. | T1 | 2 |
| RV-HUB-004 | Server-side scene caches | When Hub already holds a scene cache for a version, it is downloaded instead of converting locally. | T2 | 2 |
| RV-HUB-005 | Save review documents to Hub | A review document is saved to Hub as a new version of a file, with a version note; check-out locks prevent concurrent overwrites. | T1 | 2 |
| RV-HUB-006 | Publish packages to Hub | A published package is uploaded to a Hub folder for viewing in the browser viewer and download by the read-only viewer. | T2 | 2 |
| RV-HUB-007 | Offline work | Without a connection, Hub sources open from their local copies and changes to issues, comments and markups enter the offline queue, replayed with conflict reporting when the connection returns. | T1 | 2 |

## Issues and review data

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-HUB-008 | Issue synchronization | Issues synchronize both ways with the Hub project through the BCF API; conflicting edits keep both values and ask the user to choose. | T1 | 2 |
| RV-HUB-009 | Markups and comments from the web | Markups and comments made in the browser viewer appear in the review document with author and date after synchronization. | T1 | 2 |
| RV-HUB-010 | Notifications | Assignments, mentions and status changes on issues assigned to the user appear in a notification list linked to the issue. | T2 | 2 |
| RV-HUB-011 | Shared sets and rules | Search sets, appearance rules, clash tests and check rule sets can be published to a Hub project library and reused by other review documents, with library updates offered on open. | T2 | 3 |
| RV-HUB-012 | Reviews and approvals | A Hub review of file versions can be opened in Arc Review; the reviewer records approval or rejection with comments per file. | T2 | 2 |

## Hub jobs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-HUB-013 | Hub review jobs | A review document can be registered with Hub so that a headless Arc Review job runs its marked clash tests, check rule sets and comparisons when a new version of a participating model source reaches a chosen state; results are written back as a new document version and assignees of new results are notified. Test-level behavior is specified in RV-CLASH-025. | T2 | 3 |
| RV-HUB-014 | Automatic checking | A Hub job validates new versions against IDS specifications and check rule sets and attaches the report to the version. | T2 | 3 |
| RV-HUB-015 | Conversion jobs | Hub runs headless conversion to scene caches for each new version so the browser viewer can show it. | T1 | 2 |
| RV-HUB-016 | Batch jobs on Hub | Any batch job definition (see [18-batch-publishing.md](18-batch-publishing.md)) can be scheduled or triggered on Hub workers, with logs and outputs stored as versions. | T2 | 3 |
| RV-HUB-017 | Coordination dashboard data | Clash and issue counts per run are published to Hub for project dashboards. | T3 | 3 |
