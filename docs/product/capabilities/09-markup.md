# Markup, comments and links

Markup is the conversation layer of a review: redline drawings and tags on viewpoints and sheets,
threaded comments with statuses on review objects, and attachment links on scene items. Promoting
markup to tracked issues is specified in the issues area.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Markup | Kind (freehand, line, polyline, arrow, cloud, ellipse, rectangle, text, dimension), points in view or sheet space, color, line width, author, date | Belongs to one viewpoint or one sheet |
| Tag | Number, anchor point on a scene item, leader, label | Belongs to a viewpoint; carries a comment thread |
| Comment thread | Target (viewpoint, tag, set, clash result, check finding, task, takeoff item), comments | One per target |
| Comment | Text, author, date, status (new, active, approved, resolved) | Belongs to one thread |
| Attachment link | Target item keys, kind (URL, file, viewpoint, issue, label), name, description, anchor position, category | Attached to scene items; shown as an icon |

## Redlines

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MARK-001 | Redline tools | Draws freehand strokes, lines, polylines, arrows, revision clouds, ellipses, rectangles and text notes over the current view. Markups are drawn in view space and saved on the current viewpoint, which is created automatically if none is active. | T1 | 1 |
| RV-MARK-002 | Markup style | Color, line width and text size are chosen before or after drawing; a markup can be restyled, moved or deleted. | T1 | 1 |
| RV-MARK-003 | Erase markups | An eraser removes markups or parts of freehand strokes that it touches. | T1 | 1 |
| RV-MARK-004 | Markups on sheets | The same markup tools work on sheets, where markups are stored in sheet coordinates and scale with the sheet (see the drawings area). | T1 | 1 |
| RV-MARK-005 | Markup layers by author | Markups record author and date and can be filtered by author and date range. | T2 | 1 |

## Tags and comments

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MARK-006 | Tags | A tag places a numbered leader from a picked point on a scene item to a label; the tag stores the item key and opens a comment thread. | T1 | 1 |
| RV-MARK-007 | Tag numbering | Tag numbers increase automatically within a document; the next number can be set and existing tags renumbered in order. | T2 | 1 |
| RV-MARK-008 | Comment threads | Viewpoints, tags, sets, clash results, check findings, tasks and takeoff items each carry a comment thread; each comment records author, date and status. | T1 | 1 |
| RV-MARK-009 | Comment statuses | A comment's status is new, active, approved or resolved; the thread shows the latest status and its history. | T1 | 1 |
| RV-MARK-010 | Comments panel | A comments panel lists the comments of the selected review object and of the whole document with filters by status, author, date and target kind; selecting a comment restores its viewpoint or selects its target. | T1 | 1 |
| RV-MARK-011 | Find comments | Comments are searchable by text, author, status, date range and target kind; results list with their targets. | T2 | 1 |
| RV-MARK-012 | Mentions and assignment | A comment can mention a person by name and carry an assignee; with a Hub connection, mentions and assignees resolve to Hub users and notify them. | T2 | 2 |
| RV-MARK-013 | Merge markup from other reviewers | Merging another review document of the same federation imports its viewpoints, markups, tags and comments, keeping authors and dates (see RV-FED-006). | T2 | 1 |

## Attachment links

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MARK-014 | Add attachment link | Attaches a URL, a file path, a viewpoint, an issue or a text label to the selected items, with a name, description, category and anchor point. | T2 | 1 |
| RV-MARK-015 | Link icons | Attachment links show as icons at their anchor points; icons can be shown always or on hover, with a size setting, a maximum count and a filter by category. | T2 | 1 |
| RV-MARK-016 | Follow link | Activating a link opens a URL or file with the system handler after confirmation, restores a linked viewpoint, or opens a linked issue. | T2 | 1 |
| RV-MARK-017 | Edit and manage links | Links can be edited, moved to another anchor, and deleted; a links panel lists all links with filters. | T2 | 1 |
| RV-MARK-018 | Links from sources | Hyperlinks carried by sources (for example IFC document references and DWG hyperlinks) appear as attachment links in a source category and cannot be edited. | T2 | 1 |
