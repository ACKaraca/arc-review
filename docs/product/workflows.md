# Reference workflows

Twelve end-to-end workflows used as acceptance scenarios for the roadmap. Each step names the
capabilities it exercises; each workflow ends with observable acceptance results.

## 1. Federate discipline models

1. Open the architectural IFC model (RV-FED-001, RV-FED-027).
2. Append the structural IFC, the building-services IFC and a site DWG (RV-FED-005, RV-FED-028), or
   open a file list naming all of them (RV-FED-008).
3. Review detected units and placements; correct a source exported in millimeters without a unit
   record (RV-FED-018, RV-FED-019); align the site drawing by two point pairs or by its CRS record
   (RV-FED-022, RV-FED-023); accept or dismiss alignment suggestions (RV-FED-024).
4. Check scene statistics and source information for outlying bounds and key quality
   (RV-FED-040, RV-FED-041, RV-TREE-013).
5. Save as a linked review document (RV-FED-003).
6. After authors deliver new versions, refresh and read the refresh report (RV-FED-013, RV-FED-014,
   RV-FED-016).

**Acceptance**
- All four sources appear as separate roots in the model tree with their own units and placements.
- A known point measured in two sources gives the same coordinates within tolerance (RV-MEAS-008).
- Reopening the saved document with warm caches shows the first frame without reconverting sources.
- After refresh, the report lists added, removed, changed and re-keyed items per source, and
  existing viewpoints and sets still resolve.

## 2. Establish set and appearance standards

1. Build queries by property (discipline, system, level) in the query builder (RV-SET-012,
   RV-SET-013) and save them as search sets in folders (RV-SET-020, RV-SET-022).
2. Generate one set per value of the level property (RV-SET-027).
3. Create a rule set that colors items by system and makes architecture transparent (RV-LOOK-014,
   RV-LOOK-015) and show its legend (RV-LOOK-017).
4. Save standard viewpoints with visibility and appearance in their state bundles (RV-VIEW-001,
   RV-VIEW-002).
5. Export sets and rule sets as documented JSON and import them into another project
   (RV-SET-026, RV-LOOK-018, RV-XCHG-016).

**Acceptance**
- Search set membership updates after a refresh without user action.
- The imported rule set colors the second project identically for matching property values, and
  unmatched items are listed (RV-LOOK-019).
- Restoring a viewpoint reproduces the camera, visibility and colors exactly.

## 3. Clash coordination cycle

1. Create a test with side A from the structure search set and side B from the duct search set
   (RV-CLASH-001, RV-CLASH-002); choose a hard test with a small tolerance (RV-CLASH-009,
   RV-CLASH-013); a second test checks clearance for pipes (RV-CLASH-011).
2. Apply built-in exclusion templates and a custom query rule (RV-CLASH-016, RV-CLASH-017).
3. Run all tests (RV-CLASH-022) and read the run summary (RV-CLASH-023).
4. Group results automatically by level and element (RV-CLASH-037, RV-CLASH-038); review them step
   by step with result viewpoints (RV-CLASH-043, RV-CLASH-044); assign groups and comment
   (RV-CLASH-033, RV-CLASH-034).
5. Promote groups to issues (RV-CLASH-049) and export a report (RV-CLASH-050, RV-CLASH-051).
6. After authors fix models, refresh and re-run incrementally (RV-CLASH-024); check status
   transitions and history (RV-CLASH-031, RV-CLASH-032) and the dashboard trend (RV-CLASH-048).

**Acceptance**
- A result found again keeps its status, assignee, comments and issue link.
- A result no longer found is marked not found and moves to the status configured for the test.
- The incremental run tests only pairs involving changed items, as stated in its summary.
- The JSON result export validates against the published schema (RV-CLASH-053).

## 4. Check information requirements

1. Import the project's IDS file (RV-CHECK-015) and a library rule set for model quality and
   naming (RV-CHECK-002, RV-CHECK-005, RV-CHECK-012).
2. Map the project classification to bSDD entries (RV-CHECK-019, RV-CHECK-021, RV-CHECK-022).
3. Run checks (RV-CHECK-025) and review findings grouped by rule and source (RV-CHECK-026,
   RV-CHECK-029).
4. Accept justified exceptions and promote the rest to issues (RV-CHECK-028, RV-CHECK-030).
5. Produce the IDS audit report and the check report (RV-CHECK-017, RV-CHECK-031).

**Acceptance**
- Every IDS specification reports pass, fail or not applicable per item, with failing values shown.
- Re-running after a refresh keeps accepted findings accepted and closes findings that pass.
- Classification checks list items whose classification has no mapping.

## 5. Issue round trip with BCF and Hub

1. Connect to Arc Hub and open a project (RV-HUB-001, RV-HUB-002).
2. Create an issue from a viewpoint with a pin and markup (RV-ISSUE-001, RV-ISSUE-003,
   RV-ISSUE-005); add fields from the server's project settings (RV-ISSUE-020).
3. Synchronize through the BCF API (RV-ISSUE-019, RV-HUB-008); a web user comments in the browser
   viewer (RV-WEB-014, RV-HUB-009).
4. Work offline, change the issue status, reconnect (RV-HUB-007).
5. Export the issues as a BCF 3.0 file for a consultant whose tools read BCF (RV-ISSUE-017).
6. From an issue, ask a running Arc authoring product to select the referenced elements
   (RV-AUTO-010, RV-ISSUE-024).

**Acceptance**
- The web comment appears in the desktop issue after synchronization.
- Offline changes reach Hub on reconnection; a conflicting field keeps both values in history.
- The BCF file validates against the published schema and its components carry IFC GlobalIds.
- The authoring product selects exactly the elements named by the issue.

## 6. Nightly headless coordination

1. A scheduled job refreshes the review document's sources from Hub versions (RV-PUB-004,
   RV-HUB-003).
2. The CLI runs all clash tests and check rule sets without a window (RV-CLASH-026, RV-CHECK-032,
   RV-AUTO-007).
3. Results are written as HTML, JSON and JUnit XML; the exit code reflects thresholds
   (RV-AUTO-008, RV-AUTO-009).
4. The updated document is saved to Hub as a new version and a package is published
   (RV-HUB-005, RV-PUB-009).
5. Alternatively, Hub runs the same job when a new source version reaches a chosen state
   (RV-HUB-013).

**Acceptance**
- The job runs on a machine without a display and opens no dialog.
- The CI system shows failing tests when new clash or finding counts exceed thresholds.
- The Hub version history shows the updated document with the run recorded.

## 7. 4D sequencing

1. Import a P6 XML schedule with field mapping (RV-SEQ-005, RV-SEQ-006).
2. Define task types and appearances (RV-SEQ-003).
3. Attach items with attachment rules matching task codes to a property (RV-SEQ-011) and fix
   gaps found by coverage validation (RV-SEQ-012).
4. Play the simulation in planned-versus-actual mode with overlay text (RV-SEQ-014, RV-SEQ-015,
   RV-SEQ-016).
5. Link a clash test to the simulation to find temporary works conflicting in time (RV-SEQ-021,
   RV-CLASH-027).
6. Export the simulation video (RV-SEQ-019); later synchronize an updated schedule (RV-SEQ-007).

**Acceptance**
- Coverage validation lists tasks without items and items without tasks; both lists reach zero
  after fixing.
- At any date, item states match the task dates and task types.
- Synchronization updates dates and keeps local attachments marked as kept.

## 8. Quantity takeoff and 5D cost

1. Import a takeoff catalog with formulas and resources (RV-QTO-002, RV-QTO-003, RV-QTO-005).
2. Take off model items by search set (RV-QTO-007, RV-QTO-010) with property mapping
   (RV-QTO-008); check double counting (RV-QTO-012).
3. Calibrate a PDF sheet and take off counts, lengths and areas (RV-SHEET-021, RV-QTO-013).
4. Add virtual records for allowances (RV-QTO-017).
5. Link takeoff to cost items and tasks; view the cumulative cost curve (RV-COST-005,
   RV-COST-007, RV-COST-008).
6. After a refresh, review change detection and update quantities (RV-QTO-018, RV-QTO-019).
7. Export the workbook (RV-QTO-021).

**Acceptance**
- Each takeoff record highlights its items or sheet geometry.
- Changed source items mark their records changed with old and new quantities.
- The cost curve total equals the sum of cost items assigned to tasks.

## 9. Compare model versions

1. Compare the current and previous version of a model source (RV-COMP-001) with match settings
   (RV-COMP-002).
2. Color added, removed and changed items (RV-COMP-005) and inspect property-level differences
   (RV-COMP-003).
3. Save differences as sets and viewpoints (RV-COMP-006) and export a report (RV-COMP-008).
4. Overlay two revisions of a sheet and list change regions (RV-COMP-011, RV-COMP-013).

**Acceptance**
- Items are matched by item key; moved items are reported as moved, not removed and added.
- Property differences show old and new values with units.
- The sheet overlay shows additions and deletions in distinct colors.

## 10. Sheet review, conversion and plotting

1. Open a DWG with layouts and a PDF set; build a sheet set (RV-SHEET-001, RV-SHEET-002,
   RV-SHEET-013).
2. Turn layers on and off and jump to named views (RV-SHEET-007, RV-SHEET-008).
3. Mark up with clouds and text in a markup set; set statuses and reply (RV-SHEET-017 to
   RV-SHEET-019).
4. Calibrate the PDF and measure (RV-SHEET-021, RV-SHEET-022).
5. Align a plan sheet to the federation and jump from sheet to model (RV-SHEET-014,
   RV-SHEET-015).
6. Convert a folder of drawings to an older DWG version with a conversion setup and log
   (RV-SHEET-029 to RV-SHEET-032).
7. Batch plot the sheet set to PDF with a plot style table (RV-SHEET-025, RV-SHEET-028).

**Acceptance**
- Measurements on the calibrated PDF match drawn dimensions within tolerance.
- Converted drawings open in the target version and the log lists every file with its result.
- The batch plot produces one PDF per sheet, or one multi-page PDF, with plot styles applied.

## 11. Publish for read-only and browser recipients

1. Save tour viewpoints and set a start viewpoint (RV-VIEW-001, RV-PUB-011).
2. Publish a package with metadata, excluded hidden items and hidden property sets
   (RV-PUB-009, RV-PUB-010, RV-PUB-013, RV-PUB-014), optionally encrypted (RV-PUB-016).
3. A recipient opens it in the read-only viewer, navigates, measures and reads comments
   (RV-READ-002, RV-READ-005, RV-READ-010, RV-READ-012).
4. Publish the package to Hub (RV-HUB-006); a stakeholder opens it in the browser viewer
   (RV-WEB-003, RV-WEB-012).

**Acceptance**
- The viewer opens at the start viewpoint and offers no command that changes the document.
- Excluded items and hidden property sets are absent in both viewer and browser.
- The browser renders the federation without any external conversion service.

## 12. Immersive multi-user review

1. The coordinator hosts a session from the desktop application through Hub (RV-XR-011).
2. Participants join from headsets on OpenXR runtimes and from desktops (RV-XR-001,
   RV-XR-012); they see avatars and hear voice (RV-XR-013, RV-XR-014).
3. The group switches between full scale and tabletop (RV-XR-004); the presenter leads and others
   follow (RV-XR-016).
4. Participants point, select, section and measure; state is shared (RV-XR-005, RV-XR-007,
   RV-XR-008, RV-XR-018).
5. Issues are created in the headset with location and snapshot (RV-XR-019, RV-ISSUE-026).
6. The session ends; issues appear in the Hub issue list.

**Acceptance**
- All participants see the same visibility, section and active viewpoint within one second.
- No model geometry passes through the session relay; participants load caches locally or from Hub.
- Headset frame rate holds the runtime rate during the session.
- Issues created in the session are BCF topics with viewpoints and snapshots.
