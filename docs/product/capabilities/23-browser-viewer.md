# Browser viewer

The browser viewer is the Arc Review engine compiled to WebAssembly as the `review-web` bundle and
embedded by Arc Hub's browser client. It streams scene caches from the organization's own Hub server,
so models are viewed in a browser without any external translation service (see
[delivery-targets.md](../../architecture/delivery-targets.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Viewer bundle | Version, supported Hub API version, feature flags | Loaded by the Hub page; renders into one canvas |
| Viewing session | Hub project, file versions, signed-in user, permissions | Fetches scene caches and review data through the Hub API |
| Streamed scene | Tile and level-of-detail requests, memory budget | Built from scene caches stored on Hub |

## Loading and rendering

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-WEB-001 | WebAssembly viewer bundle | The `review-web` bundle runs in current desktop and mobile browsers, rendering with WebGPU and falling back to WebGL2; the page that hosts it belongs to Arc Hub. | T1 | 2 |
| RV-WEB-002 | Streamed scene caches | The viewer requests scene cache tiles from Hub by visibility and level of detail and shows a first image before the whole model has loaded, within a browser memory budget. | T1 | 2 |
| RV-WEB-003 | View Hub file versions | Any model, drawing or review document version on Hub that has scene caches opens in the viewer from its Hub page; versions without caches show their conversion status. | T1 | 2 |
| RV-WEB-004 | Federated views | A review document stored on Hub opens with its full federation and placements. | T1 | 2 |
| RV-WEB-005 | Permissions | The viewer shows only what the signed-in user may see; property sets and items removed by a publish profile are never sent to the browser. | T1 | 2 |
| RV-WEB-006 | No external translation | All conversion happens on the organization's Hub server by headless Arc Review workers; model data never passes through a third-party service. | T1 | 2 |

## Viewing functions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-WEB-007 | Navigation | Orbit, pan, zoom, fit, walk, standard views and an orientation widget work with mouse, keyboard and touch gestures. | T1 | 2 |
| RV-WEB-008 | Tree and properties | The model tree and properties panel show the same structure and values as the desktop; quick search finds items by name and property value. | T1 | 2 |
| RV-WEB-009 | Hide, isolate and appearance | Items, sets and categories can be hidden, isolated and colored by stored appearance rules for the session. | T1 | 2 |
| RV-WEB-010 | Sections | Section planes and a section box can be placed and moved. | T1 | 2 |
| RV-WEB-011 | Measure | Point-to-point, shortest distance, angle and area measurements are available in 3D and calibrated measurement on sheets. | T1 | 2 |
| RV-WEB-012 | Viewpoints | Saved viewpoints open with their camera, visibility, section and markups; users with permission save new viewpoints to Hub. | T1 | 2 |
| RV-WEB-013 | Markups and comments | Users with permission add markups and comments in 3D and on sheets; they are stored on Hub and appear in the desktop application on the next synchronization. | T1 | 2 |
| RV-WEB-014 | Issues | Issues are listed, opened at their viewpoints, created and updated through the Hub BCF API. | T1 | 2 |
| RV-WEB-015 | Sheets | Sheets open with layers and named views and can be printed to PDF from the browser. | T1 | 2 |
| RV-WEB-016 | Version comparison | Two versions of a file can be compared in the viewer with added, removed and changed items colored from a comparison run computed on Hub. | T2 | 3 |
| RV-WEB-017 | Clash results | Clash results and groups stored on Hub are listed and opened at their viewpoints; status and assignee can be changed by users with permission. | T2 | 3 |
| RV-WEB-018 | Display settings | Display settings cover shading mode, edges, background, ambient occlusion and a dark theme. | T2 | 2 |
| RV-WEB-019 | Embedding | Hub can provide a link or embed code for a read-only view of a version, subject to Hub sharing permissions. | T2 | 2 |
| RV-WEB-020 | Accessibility | The viewer's controls are keyboard-operable and expose accessible names to the host page. | T1 | 2 |
| RV-WEB-021 | Viewer programming interface | The bundle exposes a documented interface for the host page to load versions, set the camera, select items, apply visibility and receive selection events. | T2 | 2 |
