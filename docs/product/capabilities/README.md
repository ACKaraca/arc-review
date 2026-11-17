# Capabilities

The capability specification of Arc Review, one file per functional area. Each row states a
behavior precisely enough to implement and test it.

## Legend

- **Identifier.** `RV-<AREA>-<NNN>`: `RV` is the Arc Review prefix, `AREA` the area token below,
  `NNN` a number unique within the area. Identifiers are permanent; a withdrawn capability keeps its
  number and its row is marked withdrawn.
- **Tier.** `T1` Core: required for first production use. `T2` Professional: required for parity
  with established professional tools. `T3` Advanced: specialized or differentiating.
- **Phase.** The suite phase (0 to 5) in which the capability is planned; see the
  [roadmap](../../roadmap.md).
- Each file opens with its purpose and the domain objects it introduces; the full schema is in the
  [domain model](../../architecture/domain-model.md).

## Areas

| File | Token | Scope | Rows |
|---|---|---|---|
| [01-federation.md](01-federation.md) | FED | Opening, federating, refreshing; units, placement and alignment; readers; scene caches | 48 |
| [02-model-tree.md](02-model-tree.md) | TREE | Model tree, item keys, properties, user properties, external data links | 32 |
| [03-selection-search.md](03-selection-search.md) | SET | Picking, queries, quick find, selection sets and search sets | 28 |
| [04-navigation.md](04-navigation.md) | NAV | Orbit, walk, fly, collision, gravity, avatars, camera, overlays | 35 |
| [05-appearance.md](05-appearance.md) | LOOK | Hide, isolate, required items, overrides, appearance rules, display modes | 28 |
| [06-sectioning.md](06-sectioning.md) | SECT | Section planes and box, caps, saved sections | 17 |
| [07-viewpoints-animation.md](07-viewpoints-animation.md) | VIEW | Viewpoints, camera paths, animation clips, interaction scripts | 28 |
| [08-measurement.md](08-measurement.md) | MEAS | Distances, angles, areas, clearances, snapping | 21 |
| [09-markup.md](09-markup.md) | MARK | Redlines, tags, comment threads, attachment links | 18 |
| [10-clash-detection.md](10-clash-detection.md) | CLASH | Tests, types, rules, runs, result identity and workflow, grouping, reports | 55 |
| [11-model-checking.md](11-model-checking.md) | CHECK | Rule sets, IDS validation, bSDD, classification, COBie, findings | 34 |
| [12-issues-bcf.md](12-issues-bcf.md) | ISSUE | BCF-native issues, lists, reports, BCF files and API | 26 |
| [13-sequencing.md](13-sequencing.md) | SEQ | 4D tasks, schedule import and logic, attachment, simulation, progress, critical path and what-if scenarios | 33 |
| [14-cost.md](14-cost.md) | COST | 5D cost fields, breakdowns, rates, curves, baselines, unit-rate databases, estimate versions | 19 |
| [15-takeoff.md](15-takeoff.md) | QTO | Takeoff catalogs, 3D and 2D takeoff, change analysis, export | 24 |
| [16-comparison.md](16-comparison.md) | COMP | Model version comparison and sheet comparison | 15 |
| [17-drawings.md](17-drawings.md) | SHEET | Sheets and sheet sets, sheet markup and measurement, plotting, DWG conversion | 35 |
| [18-batch-publishing.md](18-batch-publishing.md) | PUB | Batch processing, packages and restrictions, images, video, print | 25 |
| [19-reality-data.md](19-reality-data.md) | REAL | Point clouds, reality meshes, 3D Tiles, city models, photos | 20 |
| [20-presentation.md](20-presentation.md) | REND | Presentation materials, lights, rendering through RenderBuilder, presentation mode | 17 |
| [21-immersive-review.md](21-immersive-review.md) | XR | OpenXR immersive mode and multi-user sessions | 21 |
| [22-read-only-viewer.md](22-read-only-viewer.md) | READ | The read-only viewer target | 17 |
| [23-browser-viewer.md](23-browser-viewer.md) | WEB | The WebAssembly viewer bundle in Arc Hub | 21 |
| [24-hub-integration.md](24-hub-integration.md) | HUB | Hub connection, versions, synchronization, Hub jobs | 17 |
| [25-automation.md](25-automation.md) | AUTO | Commands, automation protocol, CLI, Python, node graphs, plug-ins | 22 |
| [26-interoperability.md](26-interoperability.md) | XCHG | Export formats, review data exchange, conformance, identifiers | 22 |
| [27-application-connectors.md](27-application-connectors.md) | CONNECT | Connector export from authoring applications, re-export, batch, switch-back, licensed bridges | 20 |
| [28-mobile.md](28-mobile.md) | MOBILE | Tablet and phone builds, field markup and issues, offline packages, augmented-reality overlay | 22 |

Total: 720 capabilities.

## Capabilities and issues

Status is never tracked in these tables. Work is tracked in GitHub issues: an epic per milestone
area lists the capability identifiers it delivers in its scope, and feature issues reference the
identifiers they implement. A pull request that changes a described behavior updates the row in the
same pull request. A new capability gets the next free number in its area and is added before the
issue that implements it is marked *Ready*.
