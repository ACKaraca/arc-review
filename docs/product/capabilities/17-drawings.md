# Drawings and sheets

Drawings and sheets cover viewing, marking up, measuring, plotting and converting 2D drawing
sources: DWG and DXF layouts, PDF pages, raster images and, through an optional bridge, DWF and
DWFx sheets. Sheets can be collected into sheet sets and placed in the 3D federation so that a
reviewer moves between a drawing and the model it describes. 2D takeoff is specified in
[15-takeoff.md](15-takeoff.md) and sheet comparison in [16-comparison.md](16-comparison.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Drawing source | Path or Hub version reference, format, content hash, version, password state | Produces one or more sheets; referenced from the review document, never modified |
| Sheet | Layout name or page number, paper size, plot scale, drawing units, layers, named views, thumbnail | Belongs to one drawing source; may belong to sheet sets; may have a sheet calibration and a sheet alignment |
| Sheet set | Name, ordered sheet list, sheet numbers, subsets | References sheets from several drawing sources |
| Sheet calibration | Two picked points, known distance, unit, resulting scale | Belongs to one sheet; used by measurement and 2D takeoff |
| Sheet alignment | Sheet-to-model transform, target level or section plane, link status | Places one sheet in the federation; links sheet regions to scene items |
| Markup set | Name, author, status, markups, creation and modification dates | Belongs to one sheet or sheet set; contains markups and comment threads |
| Plot configuration | Output device or PDF, paper, orientation, area, scale, plot style table (color-dependent or named), stamp | Applied to sheets when printing or plotting; saved as named page setups |
| Conversion setup | Target DWG or DXF version, output folder, folder structure rule, suffix, audit flag, external reference handling, backup flag, log path | Applied to a list of drawing files by a conversion run |

## Viewing drawing sources

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SHEET-001 | Open DWG and DXF drawings | Opening a DWG or DXF file of any release the reader supports shows model space and every layout as separate sheets. Entities, layers, linetypes, hatches, text with substituted fonts, dimensions and blocks display as authored; unsupported entities are drawn from their stored proxy graphics and listed in a loss report. | T1 | 1 |
| RV-SHEET-002 | Open PDF sheets | Opening a PDF shows each page as a sheet with vector content kept as vectors, so zooming stays sharp and snapping finds line endpoints. Optional content groups appear as layers that can be switched on and off. | T1 | 1 |
| RV-SHEET-003 | Open raster images as sheets | PNG, JPEG and TIFF images open as sheets with their stored resolution; a sheet calibration gives them a real-world scale. | T2 | 1 |
| RV-SHEET-004 | Optional DWF and DWFx sheets | When the out-of-process bridge for DWF and DWFx is installed, their 2D sheets open as sheets with their embedded markups as a read-only markup set. Without the bridge the files are reported as unreadable with the reason. | T3 | 2 |
| RV-SHEET-005 | Sheet browser | A sheet browser lists all sheets of the review document with thumbnails, sheet numbers, source file and revision, and opens a sheet in a 2D view on selection. The list filters by text and sorts by number, name or source. | T1 | 1 |
| RV-SHEET-006 | 2D navigation | Sheets support pan, zoom with the wheel, zoom window, zoom extents and zoom to a selected entity, with keyboard equivalents for each action. A wireframe or shaded display of 3D content in model space can be orbited. | T1 | 1 |
| RV-SHEET-007 | Layers and layer states | A layer list shows each layer's name, color and visibility; toggling visibility updates the view immediately without changing the source. Layer states stored in the drawing can be applied, and the current layer visibility is saved with a viewpoint of the sheet. | T1 | 1 |
| RV-SHEET-008 | Named views | Named views stored in a drawing are listed and restore their stored window and visual style when chosen. | T2 | 1 |
| RV-SHEET-009 | External references and attached images | External references and attached images resolve relative to the host drawing, then through configured search folders; unresolved references are listed with their saved path and drawn as an outline. Nested references display to the depth the drawing stores. | T1 | 1 |
| RV-SHEET-010 | Annotation scale display | Annotative objects display at the annotation scale current in the layout or viewport; choosing another stored scale updates the display. | T2 | 1 |
| RV-SHEET-011 | Entity properties on sheets | Selecting an entity shows its type, layer, color, linetype, block name and attributes, and any extended data, in the properties panel. | T2 | 1 |
| RV-SHEET-012 | Read-only command line for sheets | A command line accepts the viewing, measuring and plotting commands available on sheets, with completion; editing commands are not registered. | T3 | 1 |

## Sheet sets and sheet-to-model links

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SHEET-013 | Create sheet sets | A sheet set collects sheets from several drawing sources in a chosen order with sheet numbers and named subsets. Adding a new revision of a drawing source keeps its sheets in place within the set. | T2 | 1 |
| RV-SHEET-014 | Align a sheet to the model | A sheet is placed in the federation by picking two or three points on the sheet and the matching points in the model, or by matching the sheet's stored coordinate system when present. The aligned sheet can be shown in the 3D view at its level or section plane with adjustable transparency. | T2 | 2 |
| RV-SHEET-015 | Sheet-to-model links | Selecting an entity on an aligned sheet selects the scene items whose item keys match the entity's linked identifier, or that lie under the picked point; selecting a scene item lists the sheets that show it and opens one at that location. | T2 | 3 |
| RV-SHEET-016 | Camera position on sheet | While navigating in 3D, an aligned plan sheet shows the current camera position and view direction; clicking a point on the sheet moves the camera there. | T3 | 3 |

## Markup and measurement on sheets

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SHEET-017 | Sheet markup tools | Markups on sheets include freehand, line, arrow, polyline, polygon, rectangle, ellipse, revision cloud, text, callout with leader, highlight, stamp and image. Each markup records author, date, color, line weight and the sheet coordinates in paper units. | T1 | 1 |
| RV-SHEET-018 | Markup sets | Markups are grouped into named markup sets per sheet or sheet set; a set can be shown, hidden, locked and exported. Markups are stored in the review document and never written into the drawing source. | T1 | 1 |
| RV-SHEET-019 | Markup status and replies | Each markup has a status (open, in progress, answered, closed) and a comment thread; a markup list filters by status, author, sheet and date and zooms to a markup on selection. | T2 | 1 |
| RV-SHEET-020 | Markups across revisions | When a drawing source is replaced by a newer revision, markups stay at their paper positions and are flagged for confirmation; markups on removed sheets are listed. | T2 | 2 |
| RV-SHEET-021 | Calibrate a sheet | A sheet calibration is set by picking two points and typing their real distance and unit; drawings with a stored scale calibrate automatically. Measurements and 2D takeoff on the sheet use the calibration. | T1 | 1 |
| RV-SHEET-022 | Measure on sheets | Sheet measurement provides distance, polyline length, area with perimeter, angle, radius and point coordinates, snapping to vector endpoints, midpoints, intersections and centers. Results show in the sheet's calibrated units and can be kept as markups. | T1 | 1 |
| RV-SHEET-023 | Return markups to authors | A markup set exports to PDF with the markups drawn over the sheet, to BCF with one issue per markup, and to documented JSON with paper coordinates, so a drafting application can locate each markup. | T2 | 2 |

## Printing, plotting and conversion

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SHEET-024 | Print and print to PDF | A sheet or the current view prints to a system printer or vector PDF with paper size, orientation, area (extents, display, window, layout), scale or fit, and optional markups. | T1 | 1 |
| RV-SHEET-025 | Plot styles | Plotting applies a color-dependent or named plot style table that maps colors or style names to output pen color, weight and screening. Tables stored with the drawing are found automatically; missing tables are reported and plotting uses object colors. | T2 | 2 |
| RV-SHEET-026 | Page setups | Named page setups stored in a drawing are listed and applied; new page setups are saved in the review document. | T2 | 2 |
| RV-SHEET-027 | Plot stamp | A plot stamp adds chosen fields (file name, sheet name, date, user, plot scale) at a chosen corner of every plotted page. | T3 | 2 |
| RV-SHEET-028 | Batch plotting | A batch plot takes a list of sheets or a sheet set with their page setups and writes them to a printer, one multi-page PDF or one PDF per sheet. It runs in the background, writes a log of every sheet with success or the failure reason and is available from the CLI. | T2 | 2 |
| RV-SHEET-029 | DWG and DXF version conversion | A conversion run applies a conversion setup to files and folders and writes each drawing as DWG or DXF of the chosen release. It keeps or flattens the folder structure, adds an optional suffix, writes backups when requested and never overwrites a source without that explicit choice. | T2 | 2 |
| RV-SHEET-030 | Conversion setups | Conversion setups are named and saved; each records target format and release, output location, folder rule, suffix, audit, external reference handling (keep, convert with host, bind) and backup. | T2 | 2 |
| RV-SHEET-031 | Audit during conversion | When audit is enabled, each drawing is checked for structural errors before writing; repairable errors are fixed in the output only and listed in the log, and unrepairable files are skipped with the reason. | T2 | 2 |
| RV-SHEET-032 | Conversion log | Every conversion run writes a log listing each input, output path, release written, warnings, losses for older releases and failures, viewable in the application and as a text file. | T2 | 2 |
| RV-SHEET-033 | Protected drawings | Drawings that require a password prompt once per file during viewing and conversion; files that are digitally signed show the signature status, and conversion records that the signature no longer applies to the output. | T3 | 2 |
| RV-SHEET-034 | Package a drawing with dependencies | Packaging collects a drawing with its external references, images, fonts, plot style tables and page setups into a folder or zip archive with paths rewritten to be relative, plus a manifest listing every file and anything that could not be found. | T2 | 2 |
| RV-SHEET-035 | Export sheets | Sheets export to vector PDF, SVG and PNG at a chosen resolution, with or without markups and with the current layer visibility. | T2 | 1 |
