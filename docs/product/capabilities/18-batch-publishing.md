# Batch processing and publishing

Batch processing converts, combines and refreshes many model sources and review documents without
interaction; publishing produces a packaged review document, images, video and prints for people
who do not take part in coordination. Every batch operation is a command that runs the same way from
the desktop, the CLI and Arc Hub jobs (see [25-automation.md](25-automation.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Batch job | Input list (files, folders, file lists, review documents), operation, output location, options, log path | Runs commands over its inputs; produces outputs and a log |
| Publish profile | Name, contents to include, metadata (title, subject, author, organization, copyright, keywords, notes), start viewpoint, restrictions | Applied to a review document to produce a published package |
| Restrictions | Expiry date, hidden property sets, exclusion of hidden items, disabled export, optional passphrase encryption | Part of a publish profile; enforced by every delivery target |
| Published package | Packaged review document, publish profile snapshot, content hashes, publish date | Opened by the desktop, the read-only viewer and the browser viewer |
| Capture settings | Size in pixels, resolution, anti-aliasing level, background, overlays, file format | Used by image, image sequence and video export |

## Batch processing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-PUB-001 | Batch inputs | A batch job takes files, folders with optional recursion and file-name patterns, file lists and review documents as inputs; duplicates are removed and unreadable inputs are reported before work starts. | T1 | 1 |
| RV-PUB-002 | Batch conversion to scene caches | A batch job converts every input model file to its scene cache, skipping files whose cache is current, so that later opening is fast. Failures are logged per file and do not stop the job. | T1 | 1 |
| RV-PUB-003 | Batch assembly of review documents | A batch job appends all inputs into one new linked or packaged review document, or produces one review document per input, applying a saved placement and reader option set. | T2 | 1 |
| RV-PUB-004 | Batch refresh and save | A batch job opens existing review documents, refreshes changed model sources, optionally re-runs clash tests and checks named in the job, and saves, recording what changed in the log. | T2 | 3 |
| RV-PUB-005 | Batch job definitions | Batch jobs are saved as documented JSON definitions that can be edited, versioned and run unchanged from the desktop or `arc-review-cli`. | T2 | 1 |
| RV-PUB-006 | Scheduled runs | A batch job definition runs unattended from `arc-review-cli` under the operating system's scheduler on Windows, macOS and Linux, or as an Arc Hub job; the exit code reports success, partial success or failure. | T2 | 2 |
| RV-PUB-007 | Batch logs | Every batch job writes a human-readable log and a machine-readable JSON summary with inputs, outputs, durations, warnings and failures. | T1 | 1 |
| RV-PUB-008 | Cancellation and resume | A running batch job can be cancelled between inputs; a resumed job skips inputs whose outputs are complete and newer than the inputs. | T2 | 2 |

## Publishing packages

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-PUB-009 | Publish a package | Publishing writes a packaged review document from the current document using a publish profile, embedding scene caches and the review data the profile includes. The source review document is unchanged. | T1 | 1 |
| RV-PUB-010 | Package metadata | The publish profile's metadata fields are written into the package and shown by every delivery target in a document information panel. | T1 | 1 |
| RV-PUB-011 | Start viewpoint | A published package can name a viewpoint that every delivery target restores when the package opens. | T2 | 1 |
| RV-PUB-012 | Choose included content | A publish profile selects which model sources, viewpoint folders, sets, markups, comments, issues, clash results, simulations and takeoff data are included; excluded data is not written to the package. | T2 | 1 |
| RV-PUB-013 | Exclude hidden items | When enabled, items hidden in the current visibility state are not written to the package, so their geometry and properties are absent rather than hidden. | T2 | 1 |
| RV-PUB-014 | Hide property sets | A publish profile lists property sets or individual properties that are removed from the package, leaving geometry and item names. | T2 | 1 |
| RV-PUB-015 | Expiry date | A package with an expiry date refuses to open in any delivery target after that date, with a message giving the date and the publisher. The restriction is a declared policy enforced by Arc applications, not encryption. | T2 | 2 |
| RV-PUB-016 | Passphrase encryption | A package can be encrypted with a passphrase using an authenticated cipher from the allowed dependency list; without the passphrase the package contents cannot be read by any tool. | T3 | 2 |
| RV-PUB-017 | Disable export | A package can declare that geometry and property export is disabled; delivery targets then disable export commands while keeping printing and image capture available. | T3 | 2 |
| RV-PUB-018 | Save for older version | A review document or package can be saved in the previous major format when it uses no features introduced since; otherwise the command lists the features that prevent it and writes nothing. | T2 | 2 |

## Images, video and print

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-PUB-019 | Image export | The current view exports to PNG, JPEG or TIFF at a chosen pixel size or resolution, independent of window size, with anti-aliasing, optional transparent background and optional overlays. | T1 | 1 |
| RV-PUB-020 | Image export of viewpoints | A batch image export writes one image per viewpoint of a folder, named after the viewpoint, with the same capture settings. | T2 | 1 |
| RV-PUB-021 | Image sequence export | A camera path, animation clip or simulation exports as a numbered image sequence at a chosen frame rate, rendered off-screen so the frame rate never drops frames. | T2 | 2 |
| RV-PUB-022 | Video export | Image sequences encode to WebM with the AV1 codec at a chosen quality; encoding runs in the background and can be cancelled. | T2 | 4 |
| RV-PUB-023 | Rendered frames | Image, sequence and video export can take frames from RenderBuilder instead of the viewport (see [20-presentation.md](20-presentation.md)). | T3 | 4 |
| RV-PUB-024 | Print the view | The current 3D view or a set of viewpoints prints to a system printer or vector-plus-raster PDF with a title block of viewpoint names, comments and date, with a preview before printing. | T1 | 1 |
| RV-PUB-025 | Viewpoint report | A viewpoint report writes the viewpoints of chosen folders with images, comments, markups and statuses to PDF or HTML. | T2 | 1 |
