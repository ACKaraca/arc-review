# Roadmap

Arc Review milestones mapped onto the suite phases. Scope is given as capability identifiers; a
milestone includes the rows of the listed areas whose phase column matches the milestone's phase,
plus any explicitly listed rows. Exit criteria are acceptance scenarios from
[workflows](product/workflows.md). Suite phases are defined in
[`arc-suite`](https://github.com/ACKaraca/arc-suite). No dates are given;
milestones complete when their exit criteria pass.

| Milestone | Suite phase | Theme |
|---|---|---|
| M1 | 1 | Viewer |
| M2 | 2 | Connected review |
| M3 | 3 | Coordination and checking |
| M4 | 3 | Planning and quantities |
| M5 | 4 | Presentation |
| M6 | 5 | Immersive review and depth |
| M7 | 5 | Connectors, mobile and field review |

## M1 Viewer (phase 1)

**Goal.** Open, federate and review models and drawings on three desktop platforms, with a
read-only viewer for recipients and a headless CLI.

**Scope.**
- Federation: RV-FED-001 to RV-FED-006, RV-FED-008 to RV-FED-011, RV-FED-013, RV-FED-014, RV-FED-016, RV-FED-018 to RV-FED-022,
  RV-FED-025 to RV-FED-030, RV-FED-033 to RV-FED-043.
- Phase-1 rows of RV-TREE, RV-SET, RV-NAV, RV-LOOK, RV-SECT, RV-MEAS and RV-MARK.
- Viewpoints: RV-VIEW-001 to RV-VIEW-008, RV-VIEW-010, RV-VIEW-011.
- Sheets: RV-SHEET-001 to RV-SHEET-003, RV-SHEET-005 to RV-SHEET-013, RV-SHEET-017 to
  RV-SHEET-019, RV-SHEET-021, RV-SHEET-022, RV-SHEET-024, RV-SHEET-035.
- Publishing: RV-PUB-001 to RV-PUB-003, RV-PUB-005, RV-PUB-007, RV-PUB-009 to RV-PUB-014,
  RV-PUB-019, RV-PUB-020, RV-PUB-024, RV-PUB-025; RV-REND-014.
- Read-only viewer: RV-READ-001 to RV-READ-016.
- Automation: RV-AUTO-001, RV-AUTO-002, RV-AUTO-004, RV-AUTO-005, RV-AUTO-007, RV-AUTO-008,
  RV-AUTO-019, RV-AUTO-020, RV-AUTO-022.
- Exchange: RV-XCHG-001, RV-XCHG-007, RV-XCHG-008, RV-XCHG-016, RV-XCHG-017, RV-XCHG-019,
  RV-XCHG-021, RV-XCHG-022.

**Dependencies.** `arc-core` phase 0 (L0 to L3, L5, L6) plus `arc-io-ifc`, `arc-io-dxf`,
`arc-io-dwg` (read), `arc-io-step`, `arc-io-mesh`, `arc-io-pdf`, `arc-draft`; `arc-brep`
tessellation for STEP. No other Arc product is required.

**Exit criteria.**
- Workflow 1 passes for IFC, DWG and STEP sources, excluding the CRS alignment and alignment
  suggestions of step 3.
- Workflow 2 passes.
- Workflow 10 steps 1 to 4 pass.
- Workflow 11 steps 1 to 3 pass (without encryption).
- The read-only viewer registry contains no mutating command (registry test).
- First frame with warm caches and navigation frame rate meet the targets in
  [architecture overview](architecture/overview.md).

## M2 Connected review (phase 2)

**Goal.** Work with Arc Hub: Hub versions as sources, BCF-native issues, browser viewing, and the
drawing output functions that need DWG write.

**Scope.**
- Hub: RV-HUB-001 to RV-HUB-010, RV-HUB-012, RV-HUB-015.
- Issues: RV-ISSUE-001 to RV-ISSUE-020, RV-ISSUE-025.
- Browser viewer: phase-2 rows of RV-WEB.
- Federation additions: RV-FED-012, RV-FED-015, RV-FED-017, RV-FED-023, RV-FED-024.
- Viewpoints and camera paths: RV-VIEW-009, RV-VIEW-012 to RV-VIEW-014.
- Sheets: RV-SHEET-004, RV-SHEET-014, RV-SHEET-020, RV-SHEET-023, RV-SHEET-025 to RV-SHEET-034.
- Publishing: RV-PUB-006, RV-PUB-008, RV-PUB-015 to RV-PUB-018, RV-PUB-021; RV-REND-016;
  RV-READ-017.
- Remaining phase-2 rows of RV-NAV, RV-LOOK, RV-SECT, RV-MARK, RV-AUTO and RV-XCHG.
- RV-ASSIST-001 to RV-ASSIST-005, RV-ASSIST-015, offered as each passes its task suite and never
  gating this milestone.

**Dependencies.** Arc Hub Documents module with BCF API and web client; `arc-hub-api`;
`arc-io-bcf`; `arc-io-dwg` write; `arc-gpu` WebGPU and WebGL2 backends on `wasm32`.

**Exit criteria.**
- Workflow 1 passes with Hub versions as sources.
- Workflow 5 passes.
- Workflow 10 passes completely.
- Workflow 11 passes completely, including the browser step.
- The engine crates build for `wasm32-unknown-unknown` in CI.

## M3 Coordination and checking (phase 3)

**Goal.** Clash detection, rule-based model checking, comparison, point clouds and large-model
streaming for production coordination.

**Scope.**
- RV-CLASH-001 to RV-CLASH-013, RV-CLASH-015 to RV-CLASH-027, RV-CLASH-029 to RV-CLASH-055.
- RV-CHECK-001 to RV-CHECK-010, RV-CHECK-012 to RV-CHECK-034.
- RV-ISSUE-021 to RV-ISSUE-024.
- RV-COMP-001 to RV-COMP-015.
- RV-FED-007.
- Phase-3 rows of RV-REAL, RV-TREE (key mapping, classification, external data links), RV-SET,
  RV-SECT, RV-MEAS, RV-FED, RV-WEB, RV-HUB, RV-AUTO and RV-XCHG.
- RV-ASSIST-006 to RV-ASSIST-011, offered as each passes its task suite and never gating this
  milestone.

**Dependencies.** `arc-io-pointcloud`, `arc-io-usd`, `arc-io-geo`; `arc-gpu` cluster culling and
streaming; Arc Hub jobs for headless runs.

**Exit criteria.**
- Workflow 3 passes on a federation of at least 20 sources.
- Workflow 4 passes, including the published IDS test cases in CI.
- Workflow 6 passes on the CLI and as a Hub job.
- Workflow 9 passes.
- Large-model targets in [large models](architecture/large-models.md) pass on the generated
  reference federation.

## M4 Planning and quantities (phase 3)

**Goal.** 4D sequencing, 5D cost and 2D and 3D quantity takeoff linked to each other.

**Scope.**
- RV-SEQ-001 to RV-SEQ-019, RV-SEQ-021 to RV-SEQ-024.
- RV-COST-001 to RV-COST-010, RV-COST-012 to RV-COST-014.
- RV-QTO-001 to RV-QTO-024.
- RV-ASSIST-012, RV-ASSIST-013, offered as each passes its task suite.

**Dependencies.** M3 query and comparison engines; `arc-solve` expression engine; schedule
importers in `review-io`.

**Exit criteria.**
- Workflow 7 passes with MSPDI and P6 sources.
- Workflow 8 passes with model and PDF takeoff.

## M5 Presentation (phase 4)

**Goal.** Presentation-quality output through RenderBuilder, object animation and video.

**Scope.**
- RV-REND-001 to RV-REND-013, RV-REND-015, RV-REND-017.
- RV-VIEW-015 to RV-VIEW-028; RV-SECT-017.
- RV-PUB-022, RV-PUB-023; RV-SEQ-020; RV-XCHG-002, RV-XCHG-005; RV-AUTO-013.

**Dependencies.** RenderBuilder interactive and batch rendering through the Arc Scene contract;
`arc-scene`; `arc-nodes`.

**Exit criteria.**
- A camera path and a 4D simulation render to an image sequence and a video through RenderBuilder.
- A rendered still matches the viewport framing and visibility of its viewpoint.
- An Arc Scene export opens in RenderBuilder and MeshBuilder.

## M6 Immersive review and depth (phase 5)

**Goal.** Immersive multi-user review and the advanced analysis rows.

**Scope.**
- RV-XR-001 to RV-XR-021; RV-ISSUE-026.
- RV-CLASH-014, RV-CLASH-028; RV-CHECK-011; RV-SEQ-025 to RV-SEQ-027; RV-COST-011.
- RV-ASSIST-014.
- Schedule logic and estimating: RV-SEQ-028 to RV-SEQ-033; RV-COST-015 to RV-COST-019.

**Dependencies.** OpenXR runtimes; Arc Hub session relay; suite assistant framework; `arc-brep`
exact intersection; schedule importers that read links and calendars.

**Exit criteria.**
- Workflow 12 passes with at least two headsets and one desktop participant.
- An assistant client lists clash results and creates an issue through the MCP endpoint, with
  preview and explicit apply.
- Workflow 15 passes with MSPDI and P6 sources.

## M7 Connectors, mobile and field review (phase 5)

**Goal.** Bring models from authoring applications through connectors and open readers, and take
review to tablets and phones, including the augmented-reality overlay on site.

**Scope.**
- Application connectors: RV-CONNECT-001 to RV-CONNECT-020.
- Open readers and connector sources: RV-FED-044 to RV-FED-048.
- Mobile: RV-MOBILE-001 to RV-MOBILE-022.

**Dependencies.** The Arc exchange package specification in `arc-core`; the `arc-connectors`
repository with at least one connector; iOS and Android builds of the engine crates, `review-ui` and
`arc-app`; Hub offline queue; `review-ar`; open reader crates (`arc-io-mesh` for FBX, and format
readers for 3DM, DGN V7 and JT) that pass the license allowlist.

**Exit criteria.**
- Workflow 13 passes with at least one connector, including incremental re-export and switch-back.
- Workflow 14 passes on one tablet and one phone for each supported operating system.
- 3DM, FBX, DGN V7 and JT corpus files open with the expected item counts and keys.
- The mobile registry contains no analysis command (registry test), and the engine crates still
  build for `wasm32-unknown-unknown`.
