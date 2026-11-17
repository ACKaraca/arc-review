# Viewpoints and animation

Viewpoints capture a camera and, optionally, the review state around it, and carry review
conversations. Camera paths, animation clips and interaction scripts add motion: walkthroughs,
moving equipment, section sweeps and simple interactive presentations that also play in the
read-only viewer mode.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Viewpoint | Name, camera, motion and realism settings, optional state bundle (visibility, overrides, appearance rule set, section state, markups, display settings), thumbnail, author, date | In one viewpoint folder; carries comment threads and markups; basis of BCF viewpoints |
| Viewpoint folder | Name, children, kind (plain or camera path) | Organizes viewpoints |
| Camera path | Ordered viewpoints or recorded camera samples, transition time, smoothing | Played as a walkthrough; used by 4D simulation and exports |
| Animation clip | Name, duration, tracks, loop mode | Contains tracks; referenced by tasks and scripts |
| Animation track | Target (set or items, camera, section state), keyframes of transform, color, transparency, section | Belongs to one clip |
| Interaction script | Name, enabled flag, events, conditions, actions, variables | Stored in the review document |

## Viewpoints

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-VIEW-001 | Save viewpoint | Saves the current camera as a named viewpoint with a thumbnail, author and date. The state bundle options chosen at save time decide which review state is stored with it. | T1 | 1 |
| RV-VIEW-002 | State bundle options | Each viewpoint records whether it includes visibility state, appearance overrides, transform overrides, the active appearance rule set, section state, display settings and navigation realism settings. Restoring a viewpoint changes only the included parts. | T1 | 1 |
| RV-VIEW-003 | Restore viewpoint | Selecting a viewpoint animates or jumps the camera to it, per user setting, and applies its included state. | T1 | 1 |
| RV-VIEW-004 | Organize viewpoints | Viewpoints are kept in folders with drag and drop, rename, copy, paste, delete and sort, in a list that shows thumbnails or names. | T1 | 1 |
| RV-VIEW-005 | Update viewpoint | Update overwrites a viewpoint's camera and included state with the current view while keeping its name, comments and markups. | T1 | 1 |
| RV-VIEW-006 | Edit viewpoint values | A viewpoint's camera position, target, field of view, projection, motion speeds, avatar and collision settings can be edited numerically. | T2 | 1 |
| RV-VIEW-007 | Viewpoint references by key | Visibility and override state in viewpoints are stored against item keys and query results, so viewpoints remain valid after refresh; unresolved keys are reported on restore. | T1 | 1 |
| RV-VIEW-008 | Viewpoint report | A report lists chosen viewpoints with image, name, comments and markups, and exports to PDF or HTML or prints. | T2 | 1 |
| RV-VIEW-009 | Exchange viewpoints | Viewpoints and folders export to and import from an open viewpoint file and to BCF viewpoints (camera, visible and selected components, colors, clipping planes, snapshot). | T2 | 2 |
| RV-VIEW-010 | Start-up viewpoint | A review document can name the viewpoint it opens at. | T2 | 1 |
| RV-VIEW-011 | Viewpoint thumbnails strip | A strip of viewpoint thumbnails can be docked along a view edge for presentations; arrow keys step through them. | T3 | 1 |

## Camera paths

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-VIEW-012 | Path from viewpoints | A camera path folder plays its viewpoints in order, interpolating position and orientation smoothly with a transition time per segment; pauses can be inserted. | T2 | 2 |
| RV-VIEW-013 | Record a camera path | Recording captures the camera continuously while the user navigates and stores it as a camera path that plays at the recorded speed. | T2 | 2 |
| RV-VIEW-014 | Playback controls | Camera paths play, pause, stop, loop and scrub with a timeline; playback is available in the read-only viewer mode. | T2 | 2 |
| RV-VIEW-015 | Edit camera paths | Path keys can be inserted, moved, deleted and retimed, and the path is shown as a curve in the view for editing. | T3 | 4 |

## Object animation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-VIEW-016 | Animation clips | An animation clip holds tracks that animate sets or items, the camera and section state over a timeline; a document holds many clips organized in folders. | T2 | 4 |
| RV-VIEW-017 | Bind items to tracks | A track targets the current selection, a selection set or a search set; search-set targets are re-evaluated when the federation refreshes. | T2 | 4 |
| RV-VIEW-018 | Capture keyframes | Capturing a keyframe records the target's transform override, color and transparency at the current time; keys are moved and edited on a timeline. | T2 | 4 |
| RV-VIEW-019 | Interpolation and timing | Values interpolate linearly or smoothly between keys; clips have duration, speed, loop and back-and-forth playback. | T2 | 4 |
| RV-VIEW-020 | Camera tracks | A clip's camera track uses a camera path or captured camera keys, so motion and camera play together. | T2 | 4 |
| RV-VIEW-021 | Section tracks | A clip's section track keyframes section planes or the section box, producing section sweeps. | T2 | 4 |
| RV-VIEW-022 | Clip playback | Clips play, pause, stop and scrub on a timeline in the view; playback is available in the read-only viewer mode and to 4D tasks that reference a clip. | T2 | 4 |
| RV-VIEW-023 | Clash along a clip | A clip can be referenced by a clash test so moving items are checked at each step of the clip (see the clash detection area). | T3 | 4 |

## Interaction scripts

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-VIEW-024 | Event and action scripts | An interaction script runs actions when events occur. Events include start, key press, timer, collision of the avatar with a set, entering or looking at a hotspot, a variable change and the end of an animation. | T3 | 4 |
| RV-VIEW-025 | Script actions | Actions include play, pause or stop a clip or camera path, restore a viewpoint, show a message, set a variable, store and restore an item property used by a condition, and load or show a model source. | T3 | 4 |
| RV-VIEW-026 | Hotspots and conditions | Hotspots are points or sets with a radius; conditions on variables and hotspot state gate actions. | T3 | 4 |
| RV-VIEW-027 | Enable scripts | Scripts are enabled individually and by a global switch; scripts never modify sources or review data other than their own variables. | T3 | 4 |
| RV-VIEW-028 | Scripts in the viewer mode | Interaction scripts and their animations run in the read-only viewer mode and the browser viewer. | T3 | 4 |
