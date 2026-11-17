# Vision

Why Arc Review exists, whom it serves, the principles it follows, its scope and non-goals, and what
distinguishes it.

## Problem and purpose

Buildings and infrastructure are designed by many teams in many applications. Before anything is
built, someone has to bring those models together, look at them as one, find where they conflict,
check that they carry the information the project requires, plan how they will be built and priced,
and send precise, traceable questions back to the people who can fix them. Today that work is spread
over separate tools for viewing, coordinating, checking, scheduling, takeoff, sheet review and
immersive review, each with its own data and rarely with open exchange between them.

Arc Review is one application for that work. It reads models and drawings of many formats without
changing them, federates them with correct units and placement, and keeps all review results in its
own open document: viewpoints, sets, markups, clash tests and results, check findings, issues,
construction sequences, quantities and costs. It runs on Windows, macOS and Linux, works fully
offline, connects to Arc Hub for shared work, and reaches recipients through a read-only viewer and
a browser viewer built from the same engine.

## Primary users and their deliverables

| User | Deliverables |
|---|---|
| BIM and VDC coordinators | Federated models, clash reports, coordination issues, coordination meeting viewpoints |
| Discipline coordinators (structure, building services, civil) | Resolved clash groups, issue responses, discipline clearance checks |
| Information managers | Information requirement checks, IDS audit reports, classification mappings |
| Construction planners | 4D sequences, simulation videos, schedule coverage reports |
| Estimators and quantity surveyors | Takeoff workbooks from models and sheets, cost breakdowns, change analyses |
| Project managers and owners | Reviewed packages, issue status, model comparisons between milestones |
| Reviewers, clients and site staff | Markups, comments and issues in the read-only viewer, the browser or a headset |
| Automation engineers | Nightly coordination jobs, checks in continuous integration, custom rules |

## Product principles

1. **Read, never write sources.** Review data lives in the review document; sources stay as their
   authors made them ([ADR 0001](../adr/0001-review-data-separate-from-sources.md)).
2. **Open data in and out.** IFC, BCF and IDS are first-class; clash results, takeoff and all review
   data use documented formats.
3. **Identity is everything.** Every reference uses stable item keys, so sets, results, issues and
   quantities survive model updates.
4. **One query language.** Sets, appearance rules, clash rules, check rules, task attachment and
   takeoff share one way of describing which items they mean.
5. **Scale by streaming.** Opening cost follows what is visible, not what is federated.
6. **Issue-centric.** Clashes, findings, markups and immersive sessions converge on one issue model
   synchronized with Hub.
7. **One engine everywhere.** Desktop, read-only viewer and browser show the same thing for the
   same document.
8. **Automatable.** Everything that can be clicked can be run headless and in continuous
   integration.

## Scope

Federation of 3D models and 2D drawings; model tree and properties; selection, search and sets;
navigation; appearance and visibility; sectioning; viewpoints and animation; measurement; markup and
comments; clash detection; rule-based model checking; issues and BCF; 4D sequencing; 5D cost;
2D and 3D quantity takeoff; model and drawing comparison; sheet viewing, plotting and DWG version
conversion; batch processing and publishing; point clouds and reality data; rendering and
presentation through RenderBuilder; immersive multi-user review through OpenXR; the read-only
viewer; the browser viewer; Hub integration; automation and extensibility. The full list is in the
[capability index](capabilities/README.md).

## Non-goals

| Non-goal | Reason |
|---|---|
| Editing source models or drawings | Authoring belongs to the Arc authoring products and to each author's own tools; editing would break source ownership ([ADR 0001](../adr/0001-review-data-separate-from-sources.md)). |
| Reading formats without a published specification (for example RVT) | Suite interoperability policy forbids reverse engineering; such models arrive through IFC, STEP, glTF or OpenUSD. |
| Plug-ins inside other vendors' authoring applications | Round trips use identifiers through BCF, exported lists and the Arc Automation Protocol instead of in-process add-ins for third-party tools. |
| A built-in photoreal path tracer | Rendering is provided by RenderBuilder through the Arc Scene contract. |
| Full project scheduling (critical path calculation, resource leveling) | Schedules are authored in scheduling tools and imported; Arc Review visualizes, validates coverage and records progress. |
| Full estimating and bid management | Arc Review produces quantities and cost breakdowns; pricing databases and bids stay in estimating tools fed by its exports. |
| A hosted cloud service | Collaboration is through self-hostable Arc Hub; no feature depends on a vendor cloud. |
| License management, seat limits or expiring trials | The product is open source; the read-only viewer exists for simplicity, not for licensing. |

## Positioning

Arc Review covers the professional scope of model aggregation, coordination and review tools such
as Autodesk Navisworks Manage, Simulate and Freedom, Autodesk Design Review and DWG TrueView,
Autodesk Workshop XR, Solibri, Revizto, BIMcollab, Trimble Connect and Bentley SYNCHRO, together
with the open IfcOpenShell checking and clash tools.

## Differentiators

Each item below is a capability of Arc Review.

- **BCF and IDS native.** Issues are BCF topics internally and synchronize through the BCF API;
  information requirements are IDS specifications validated with audit reports
  ([issues](capabilities/12-issues-bcf.md), [model checking](capabilities/11-model-checking.md)).
- **Rule-based model checking beside geometric clashes.** Property, naming, relationship,
  clearance, space, model-quality and classification rules, plus bSDD lookup and COBie validation,
  in the same application as clash detection.
- **Clash identity across revisions.** Results keep status, assignment and history through any
  number of runs; incremental re-runs, duplicate suppression, element-centric and spatial grouping,
  discipline-pair matrices and dashboards reduce noise
  ([clash detection](capabilities/10-clash-detection.md)).
- **Issue-centric workflow synchronized with Hub.** Clash groups, findings, sheet markups and
  immersive sessions become issues with one model, offline queueing and conflict-preserving merge.
- **Streaming of very large models.** Content-addressed scene caches, cluster level of detail and
  GPU-driven culling on desktop and in the browser ([large models](../architecture/large-models.md)).
- **Open clash and takeoff data.** Documented JSON formats for clash tests and results, takeoff
  workbooks and all review data, plus CSV, spreadsheet and HTML outputs.
- **One application for viewing, checking, planning and XR.** Coordination, checking, 4D and 5D,
  takeoff, sheet review, presentation and immersive review share one document and one engine.
- **Stable identity and alignment help.** Item key quality is measured per source, keys are mapped
  across refreshes, and CRS-aware placement with alignment suggestions reduces manual transforms.
- **Planning depth.** Schedule coverage validation, planned-versus-actual delay views, progress
  recording, scenarios and cost curves linked to takeoff.
- **Every platform, no license server.** Windows, macOS and Linux desktop and viewer; a browser
  viewer in self-hosted Hub with no external translation service; a headless CLI for automation and
  continuous integration.
- **Live multi-user review.** Desktop and headset participants share one session through OpenXR on
  any conformant runtime, synchronizing review state rather than geometry
  ([ADR 0006](../adr/0006-review-sessions-synchronize-state-not-geometry.md)).
