# Read-only viewer

The read-only viewer, `arc-review-viewer`, is a slim executable built from the same engine and user
interface as the desktop application that registers only non-mutating commands. It lets people who
receive review documents, packages, models and drawings inspect them without the coordination tools
(see [ADR 0002](../../adr/0002-one-engine-three-delivery-targets.md) and
[delivery-targets.md](../../architecture/delivery-targets.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Viewer mode | Registered command set (non-mutating only), feature list | The second delivery target; shares engine crates with the desktop application |
| Opened content | Review document (linked or packaged), model file, drawing source | Read without write access; no review data is saved |
| Session state | Camera, visibility, section, unsaved measurements | Lives only for the viewing session |

## Opening and compatibility

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-READ-001 | Separate viewer executable | `arc-review-viewer` installs and runs on Windows, macOS and Linux without the desktop application, an account or a network connection. | T1 | 1 |
| RV-READ-002 | Open review documents and packages | The viewer opens linked and packaged review documents; linked documents resolve their model sources as the desktop does. | T1 | 1 |
| RV-READ-003 | Open model files and drawings | The viewer opens every model and drawing format the desktop reads, using the same readers and scene caches. | T1 | 1 |
| RV-READ-004 | Format compatibility | The viewer opens review documents and packages written by every earlier released version, and newer minor versions with unknown data ignored and reported; a newer major format is refused without touching the file. | T1 | 1 |
| RV-READ-005 | Publish restrictions honored | The viewer refuses expired packages, asks for the passphrase of encrypted packages, never shows removed properties or excluded items, and disables export when the package disables it. | T1 | 1 |
| RV-READ-006 | No writes | The viewer never writes to a review document, package, model file or drawing; scene caches it builds go to the user cache directory only. | T1 | 1 |

## Viewing functions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-READ-007 | Navigation | All navigation modes of [04-navigation.md](04-navigation.md) are available, including walk with gravity and collision. | T1 | 1 |
| RV-READ-008 | Tree, properties and find | The model tree, properties, quick properties, find and stored sets are available for selecting items; sets cannot be created or changed. | T1 | 1 |
| RV-READ-009 | Session-only visibility and sections | Hide, isolate, section planes and section boxes can be used during the session; changes are discarded when the content closes. | T1 | 1 |
| RV-READ-010 | Measure without saving | Measurements are available and shown during the session but are not stored. | T1 | 1 |
| RV-READ-011 | Play stored review data | Viewpoints, camera paths, animation clips, interaction scripts and simulations stored in the document play as authored. | T1 | 1 |
| RV-READ-012 | Read markups, comments and issues | Markups, tags, comment threads, issues and clash results stored in the document are shown read-only with their statuses. | T1 | 1 |
| RV-READ-013 | Sheets | Sheets of the document open with layers, named views, markup sets (read-only) and calibrated measurement. | T1 | 1 |
| RV-READ-014 | Print and image export | The current view, viewpoints and sheets can be printed and exported as images unless export is disabled by the package, in which case print and image capture remain. | T1 | 1 |
| RV-READ-015 | Commands not registered | Append, merge, save, publish, clash, check, takeoff, scheduling and markup authoring commands are absent from the viewer's registry, so no menu, command line, script or automation call can run them. | T1 | 1 |
| RV-READ-016 | Accessibility | The viewer is keyboard-complete, names every control for screen readers, scales its interface and offers high-contrast themes. | T1 | 1 |
| RV-READ-017 | Read-only automation | The viewer serves the Arc Automation Protocol with its non-mutating commands, so scripts can open content, query properties and export images. | T3 | 2 |
