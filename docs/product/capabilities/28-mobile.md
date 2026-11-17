# Mobile

Arc Review runs on tablets and phones for site work: viewing, navigation, measuring, markups,
issues and offline packages, and, in phase 5, an augmented-reality overlay of the model on site. The
mobile build is the same Rust engine as the desktop application, compiled for iOS and Android with
the same UI kit (suite
[ADR-0018](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0018-web-and-mobile-targets.md);
product decision in [ADR 0007](../../adr/0007-mobile-is-a-fourth-target-of-the-same-engine.md)).
It does not run analysis engines: clash tests, rule checks, comparison and takeoff are run on the
desktop, in the CLI or as Hub jobs, and their stored results are viewable on mobile.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Offline package | Source (Hub project folder or published package), content hashes, size, download time, storage budget | Holds scene caches and review data for use without a connection |
| Device budget | Memory limit, storage limit, thermal and battery policy | Governs streaming and level of detail |
| Field capture | Photo, device pose, time, optional note | Attached to an issue, markup or progress record |
| AR session | Anchor set, alignment transform, display options | Overlays one federation on the camera view |

## Viewing and navigation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MOBILE-001 | Mobile builds | Arc Review is built for iOS and Android tablets and phones from the engine crates and the UI kit used on the desktop. The mobile registry holds read commands plus the review-data commands of this file, and never links the analysis engines. | T2 | 5 |
| RV-MOBILE-002 | Open sources | A mobile device opens packaged review documents, Hub versions and published packages, and honors package restrictions exactly as the read-only viewer does (RV-READ-005). | T2 | 5 |
| RV-MOBILE-003 | Touch navigation | Orbit, pan and zoom use one- and two-finger gestures; a thumb-driven walk control moves at walking pace; a tap picks, a double tap focuses; the pick and navigation modes match the desktop. | T2 | 5 |
| RV-MOBILE-004 | Adaptive layout | The interface adapts to phone and tablet sizes and orientations: panels become sheets on phones and dock on tablets. Text scales with the system setting, targets meet the platform's minimum touch size and every control has an accessible name for the platform screen reader. | T2 | 5 |
| RV-MOBILE-005 | Tree, properties and find | The model tree, property inspector and quick find work on small screens with virtualized lists; saved sets and viewpoints are listed and restored. | T2 | 5 |
| RV-MOBILE-006 | Visibility and sections | Hide, isolate, appearance by stored rule sets and session-only section planes or box are available by touch; none of these changes persist unless saved as a viewpoint. | T2 | 5 |
| RV-MOBILE-007 | Playback | Stored viewpoints, camera paths, animation clips and 4D simulations play on mobile from data stored in the document. | T2 | 5 |
| RV-MOBILE-008 | Clash and finding results | Stored clash results and check findings are listed, selected and located in the model, and can be turned into issues (see [12-issues-bcf.md](12-issues-bcf.md)); runs are not available. | T2 | 5 |
| RV-MOBILE-009 | Sheets | Sheets and sheet sets open with layers, named views and calibration, and can be navigated by pinch and pan. | T2 | 5 |

## Measuring and marking up

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MOBILE-010 | Touch measurement | Distances, angles and areas are measured with snapping; a magnifier shows the point under the finger so that points can be placed precisely, and the result is shown in display units. | T2 | 5 |
| RV-MOBILE-011 | Markup by finger and pen | Redlines, clouds, text and arrows are drawn on viewpoints and sheets with a finger or a stylus, with pressure where the device reports it; markups are review data and undoable. | T2 | 5 |
| RV-MOBILE-012 | Issues in the field | A BCF issue is created from the current view with a pin, markup, snapshot and assignee, and existing issues are commented and moved through their statuses. | T2 | 5 |
| RV-MOBILE-013 | Camera captures | A photo taken in the application attaches to an issue, a markup or a progress record (RV-SEQ-022) with its time and, when available, device position; the original image is stored unmodified. | T2 | 5 |

## Offline and device limits

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MOBILE-014 | Offline packages | The user downloads a Hub folder or a published package for offline use, sees its size before the download and removes it later. Offline packages open without a connection. | T2 | 5 |
| RV-MOBILE-015 | Offline queue and sync | Issues, comments, markups and progress records made offline enter the queue of RV-HUB-007 and are replayed on reconnection with conflict-preserving merge; the queue is visible and survives restarts. | T2 | 5 |
| RV-MOBILE-016 | Device budgets | Streaming and level of detail follow the memory and storage budgets of the device and react to thermal and low-power states by lowering detail, never by dropping required items (RV-LOOK-004). A source too large for the device opens as a streamed subset with a notice. | T2 | 5 |
| RV-MOBILE-017 | Local data protection | Review documents, caches and queued changes are kept in the application's private storage; the user can wipe all local data from settings, and nothing is sent to a service other than the user's configured Hub. | T2 | 5 |

## Augmented reality on site

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MOBILE-018 | Model overlay | The federation, or a selection, is drawn over the live camera image using the device's motion tracking, so that a reviewer on site sees the model in place. | T3 | 5 |
| RV-MOBILE-019 | Alignment to the site | The overlay is aligned by picking one to three corresponding points on site and in the model (RV-FED-022), by a scanned marker placed at a surveyed point, or by the device position and heading when the federation has a CRS record. The residual is shown, and the alignment is saved with the viewpoint. | T3 | 5 |
| RV-MOBILE-020 | Overlay display controls | Opacity, X-ray, section box and category visibility apply to the overlay, and items nearer to the camera than real surfaces can be shown through them or hidden, according to a user setting. | T3 | 5 |
| RV-MOBILE-021 | Issues and captures in place | Issues, markups and photos can be created in the overlay view; a capture stores the camera image, the rendered overlay and the alignment so that the position can be reproduced. | T3 | 5 |
| RV-MOBILE-022 | Progress against plan | A capture with overlay can be attached to a task as a progress record with the simulation state of that date, so planned and observed work can be compared. | T3 | 5 |
