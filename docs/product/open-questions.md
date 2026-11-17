# Open questions

Behaviors that need verification or a design decision before implementation. Each entry names the
affected capabilities and what closes it. Resolved questions are recorded in the capability text or
an ADR and marked closed here.

| ID | Question | Capabilities | Closed by |
|---|---|---|---|
| OQ-01 | When merging another review document, how are geometry-bearing partial models of the same source combined, and which review objects match by identifier versus by name? | RV-FED-006, RV-FED-007 | A merge specification with test cases for duplicate viewpoints, sets and tests, decided before phase 1 feature freeze. |
| OQ-02 | Which open readers are viable under the dependency license allowlist for 3DM, FBX, DGN V7 and JT, and which of DWF and DWFx still need the out-of-process bridge? | RV-FED-031, RV-FED-032, RV-FED-044 to RV-FED-047, RV-SHEET-004 | License review of candidate libraries and published specifications per format. |
| OQ-03 | Should Arc Review read other Arc products' document packages directly through a documented review projection, instead of invoking each product's CLI to export IFC or an Arc Scene? | RV-FED-012 | A suite decision on a cross-product review projection in the document package. |
| OQ-04 | What set of pick levels does a click resolve to, and how do they map onto hierarchies of different formats? | RV-SET-002, RV-CLASH-005 | Usability tests with IFC, DWG and STEP federations. |
| OQ-05 | Can a set contain other sets, and how are cycles and mixed static and dynamic membership evaluated? | RV-SET-023 | Query engine specification with cycle detection rules. |
| OQ-06 | Is item locking (preventing selection) needed beyond hiding and required state? | RV-LOOK-007 | Evidence from coordinator workflows. |
| OQ-07 | What event, condition and action set do interaction scripts need, and should they be expressed as node graphs on `arc-nodes` instead of a dedicated script model? | RV-VIEW-024 to RV-VIEW-028, RV-AUTO-013 | A decision once node graphs exist in phase 4. |
| OQ-08 | Which data sources do external data links support natively (CSV, XLSX, SQLite, PostgreSQL), and is a generic database bridge needed? | RV-TREE-028 to RV-TREE-032 | Survey of project data sources and allowed-license drivers. |
| OQ-09 | Which comment statuses are offered, and how do they relate to BCF topic statuses? | RV-MARK-009, RV-ISSUE-002 | Status model decision aligned with BCF project settings. |
| OQ-10 | Which built-in clash exclusion templates are provided by default? | RV-CLASH-016 to RV-CLASH-019 | Analysis of exclusion needs on corpus federations. |
| OQ-11 | Which status does a result take when it is found again after being approved or resolved, and when it is no longer found? | RV-CLASH-031 | A configurable transition table with defaults, tested on multi-run fixtures. |
| OQ-12 | What exactly does the conservative hard test report beyond the standard test (coincident faces, closed volumes, tangent surfaces), and what false-positive rate is acceptable? | RV-CLASH-010 | Benchmark on synthetic cases with known answers. |
| OQ-13 | Which report formats are required at first release (PDF, CSV, XLSX, HTML), and what layouts? | RV-CLASH-050, RV-CLASH-051, RV-CHECK-017, RV-CHECK-031 | User feedback on report templates during phase 3. |
| OQ-14 | Which versions of MSPDI and P6 formats are supported, and how does synchronization treat locally edited tasks? | RV-SEQ-005, RV-SEQ-007 | Corpus of schedule exports and a synchronization rule table. |
| OQ-15 | Does cost come primarily from schedule sources, from takeoff with rates, or both, and how are conflicts resolved? | RV-COST-001 to RV-COST-005 | A cost model decision with estimators. |
| OQ-16 | Which formula language is used for takeoff and cost (the `arc-solve` expression engine or a restricted subset), and how are units checked? | RV-QTO-004, RV-COST-004 | Prototype of formulas on `arc-solve` with unit errors reported. |
| OQ-17 | How does change analysis classify takeoff records after a refresh (changed geometry, changed properties, re-keyed, removed)? | RV-QTO-018, RV-QTO-019 | Rules aligned with the comparison engine's difference kinds. |
| OQ-18 | What render presets and controls does presentation expose, given that rendering is provided by RenderBuilder? | RV-REND-007 to RV-REND-012 | RenderBuilder's interactive and batch interfaces in phase 4. |
| OQ-19 | What option set does the batch CLI expose, and how are job definitions versioned? | RV-PUB-001 to RV-PUB-008 | CLI specification review. |
| OQ-20 | Expiry dates and hidden properties cannot be enforced against modified open-source readers unless content is encrypted; which restrictions are offered and how is their limit communicated? | RV-PUB-015 to RV-PUB-017, RV-READ-005 | A publishing security note and an encryption design based on an allowed cryptography library. |
| OQ-21 | Which OpenXR runtimes and devices are tested, how do standalone headsets participate, and can browser users join sessions? | RV-XR-001, RV-XR-012 | Device matrix decision before phase 5. |
| OQ-22 | What WebAssembly memory and thread limits apply across target browsers, and what is the largest federation the browser bundle supports? | RV-WEB-002 | Measurements on target browsers with generated federations. |
| OQ-23 | Which review and approval workflows run in Hub versus in Arc Review? | RV-HUB-012 | Arc Hub Documents module specification. |
| OQ-24 | Which DWG and DXF versions can conversion target, and which options are needed (audit, references, digital signatures)? | RV-SHEET-029 to RV-SHEET-033 | `arc-io-dwg` write coverage in phase 2. |
| OQ-25 | How do alignment suggestions decide that a source is misplaced (unit factor patterns, bounds distance, shared grid names)? | RV-FED-024 | Heuristics evaluated on corpus federations with known misplacements. |
| OQ-26 | Which video encoding is used for exports, given codec licensing (AV1 in WebM proposed)? | RV-PUB-022, RV-SEQ-019 | Encoder library license review. |
| OQ-27 | How deep is IFC4.3 infrastructure support at first release (alignments, linear placement, geotechnical entities)? | RV-FED-027 | `arc-io-ifc` coverage plan. |
| OQ-28 | How is voice carried in immersive sessions (system audio, Hub relay, peer to peer), and is it in scope for the first XR release? | RV-XR-014 | Session protocol design in phase 5. |
| OQ-29 | Which host applications and versions get a connector first, and which items of a host model (families, linked models, design options, phases) does the exchange package carry at first release? | RV-CONNECT-001, RV-CONNECT-004, RV-CONNECT-007 | A coverage plan agreed with the `arc-connectors` repository and the exchange package specification. |
| OQ-30 | How does the incremental export express moved, re-keyed and deleted elements, and what revision rule triggers a full export? | RV-CONNECT-009 | A change-set specification with test cases from connector corpus exports. |
| OQ-31 | Which host APIs allow a temporary view with camera, section and hidden elements, and what is the fallback when one does not? | RV-CONNECT-014 | Survey of host public APIs when each connector is planned. |
| OQ-32 | Which device classes and operating system versions are supported on mobile, and what is the largest federation a device streams? | RV-MOBILE-001, RV-MOBILE-016 | Device matrix and measurements with generated federations. |
| OQ-33 | Which tracking methods are accepted for augmented-reality alignment at first release (point pairs, markers, device position), and what accuracy is stated for each? | RV-MOBILE-018, RV-MOBILE-019 | Field trials with surveyed targets. |
| OQ-34 | Which calendar types, constraints and link types of MSPDI and P6 does critical path calculation support, and how do results differ from the scheduling tool's when it uses rules outside that set? | RV-SEQ-028, RV-SEQ-029 | A calculation specification with schedule corpus comparisons. |
| OQ-35 | How are effective dates, regions and superseded entries resolved when several unit-rate databases are used in one estimate? | RV-COST-015, RV-COST-017 | A resolution rule table tested with estimators. |
