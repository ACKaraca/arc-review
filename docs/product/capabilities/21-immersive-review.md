# Immersive review

Immersive review puts reviewers inside the federated model through OpenXR, alone or in a multi-user
session that desktop participants can join. Sessions synchronize review state, not geometry: every
participant loads the same scene caches and exchanges cameras, selections, markups and issues
(see [ADR 0006](../../adr/0006-review-sessions-synchronize-state-not-geometry.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| XR session | Session id, host, review document and federation revision, scale mode, lock state, start time | Hosted by Arc Hub or a LAN host; has participants |
| Participant | Display name, device kind (headset or desktop), avatar, role (host, presenter, participant), audio state | Belongs to one XR session |
| Avatar | Head and hand poses, color, name label | Represents one participant |
| Scale mode | Full scale or tabletop with a scale factor and anchor | Shared by a session or set per participant |
| Session record | Participants, created issues and markups, viewpoints captured, duration | Stored in the review document after the session ends |

## Single-user immersive viewing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-XR-001 | OpenXR runtime support | Immersive mode runs on any conformant OpenXR runtime on Windows and Linux, including PC-tethered headsets and standalone headsets connected through the runtime's streaming path. Missing extensions are detected at start and the matching features are disabled with a notice. | T2 | 5 |
| RV-XR-002 | Enter and leave immersive mode | Entering immersive mode starts at the current camera position and visibility state; leaving it returns the desktop view to where the headset was. | T2 | 5 |
| RV-XR-003 | Teleport and smooth movement | Controllers move the reviewer by teleport to a picked floor point or by smooth movement and snap turning, with gravity and collision applied as configured for walking. | T2 | 5 |
| RV-XR-004 | Full scale and tabletop | The model is viewed at full scale or as a tabletop miniature that can be moved, rotated and scaled with two hands; switching keeps the point of interest centered. | T2 | 5 |
| RV-XR-005 | Laser pointer and selection | A controller ray highlights and selects scene items; the selected item's properties appear on a panel attached to the other hand. | T2 | 5 |
| RV-XR-006 | Tree, hide and isolate | A hand menu shows a simplified model tree and saved sets; items, sets and categories can be hidden or isolated, and the visibility state is shared with the desktop document. | T2 | 5 |
| RV-XR-007 | Section in immersive mode | Section planes and a section box can be placed and moved with controllers; existing section states from viewpoints are applied on request. | T2 | 5 |
| RV-XR-008 | Measure in immersive mode | Point-to-point and shortest distance measurements are taken by pointing at snap points; results display in the document's units and can be saved as markups. | T2 | 5 |
| RV-XR-009 | Viewpoints in immersive mode | Saved viewpoints can be visited, and the current head pose saved as a new viewpoint with a snapshot. | T2 | 5 |
| RV-XR-010 | Comfort and performance | The renderer keeps the headset's native frame rate by streaming and culling within a frame budget, reduces detail during motion, and offers vignetting during smooth movement. | T1 | 5 |

## Multi-user sessions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-XR-011 | Host a session | A host starts a session for a review document through Arc Hub or as a LAN host on the local network; the session advertises the federation revision so every participant loads identical scene caches. | T2 | 5 |
| RV-XR-012 | Join from a headset or the desktop | Participants join from a headset or from the desktop application with a mouse and keyboard; desktop participants see headset avatars and appear as avatars themselves. | T2 | 5 |
| RV-XR-013 | Avatars and presence | Each participant appears as an avatar with head and hand poses and a name label, updated at interactive rates; a participant list shows who is present and their device kind. | T2 | 5 |
| RV-XR-014 | Voice | Spatial voice uses the system's audio devices, with mute per participant and host mute for all; voice is carried by the session transport and is not recorded. | T2 | 5 |
| RV-XR-015 | Shared pointer and selection | Each participant's laser pointer and selection are visible to others in that participant's color. | T2 | 5 |
| RV-XR-016 | Follow the presenter | Participants can follow the presenter's position and view, and leave following at any time; the presenter can bring all participants to their location. | T2 | 5 |
| RV-XR-017 | Host controls | The host can lock the session, admit or remove participants, assign the presenter role and end the session. | T2 | 5 |
| RV-XR-018 | Shared review state | Visibility, section states, viewpoints and markups made in the session are synchronized as review-state changes; geometry is never transmitted between participants. | T1 | 5 |
| RV-XR-019 | Issues from immersive review | An issue created in a session records the creator, the head pose as a BCF viewpoint, the selected items, the section state and a snapshot, and becomes a BCF issue in the review document and on Arc Hub (see [12-issues-bcf.md](12-issues-bcf.md)). | T1 | 5 |
| RV-XR-020 | Session record | When a session ends, a session record lists participants, duration and every issue, markup and viewpoint created, and is saved in the review document. | T3 | 5 |
| RV-XR-021 | Mismatched revisions | A participant whose model sources differ from the session's federation revision is told which sources differ and is offered to fetch them from Hub before joining. | T2 | 5 |
