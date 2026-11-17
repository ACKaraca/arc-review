# Point clouds and reality data

Reality data places captured conditions next to designed models: laser scans, photogrammetry meshes,
tiled city and terrain context, and geolocated photos and panoramas. All reality data is converted
into streamable scene caches (see [large-models.md](../../architecture/large-models.md)) and is
reviewed with the same navigation, sectioning, measurement and clash tools as models.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Point cloud | Source files, format, point count, attributes (intensity, color, classification, normal, scan position), bounds, CRS record | Model source kind; converted to an octree scene cache |
| Scan station | Position, orientation, scan id, optional panorama | Belongs to a point cloud |
| Reality mesh | Format (3D Tiles, glTF, textured OBJ), tile tree, texture budget, CRS record | Model source kind; streamed by level of detail |
| Photo | Image, capture position and orientation, time, camera intrinsics | Placed in the federation; can be attached to issues and viewpoints |
| Panorama | Equirectangular image, position, heading | Viewable from its position; can be linked to a scan station |
| Point display settings | Color mode, point size, size mode, density budget, eye-dome lighting flag | Per point cloud or global |

## Point clouds

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-REAL-001 | Read point cloud formats | E57 (structured and unstructured, multiple scans), LAS and LAZ (all point formats of the published versions), PLY, PTS, PTX and ASCII XYZ with a column mapping are read as point cloud model sources. Unknown attributes are kept and listed. | T1 | 3 |
| RV-REAL-002 | Octree conversion | Each point cloud is converted once into a content-addressed octree scene cache with per-node level of detail; conversion runs in the background, shows progress and can be cancelled. Billions of points are converted out of core within a configured memory limit. | T1 | 3 |
| RV-REAL-003 | Streamed display | Point clouds draw from the octree within a point budget, refining nodes near the camera and on screen first and coarsening during motion; required items and models keep priority over points. | T1 | 3 |
| RV-REAL-004 | Point color modes | Points are colored by stored RGB, intensity, elevation ramp, classification, scan, or a single color, switchable per point cloud. | T1 | 3 |
| RV-REAL-005 | Point size and shading | Point size is set in pixels or world units with an adaptive mode that fills gaps; an eye-dome lighting option improves depth perception without normals. | T2 | 3 |
| RV-REAL-006 | Classification filter | Points of chosen classification codes or scans are shown or hidden without reconversion. | T2 | 3 |
| RV-REAL-007 | Placement and CRS | Point clouds carry their stored CRS record and are placed with the federation's placement rules; large coordinates keep precision through camera-relative rendering. | T1 | 3 |
| RV-REAL-008 | Section and crop | Section planes and section boxes clip point clouds exactly as they clip models; a crop region can limit a point cloud to a box or polygon prism stored in the review document. | T1 | 3 |
| RV-REAL-009 | Snap and measure on points | Measurements snap to the nearest displayed point within a pick radius, and to a fitted plane or line when chosen, reporting the fit residual. | T2 | 3 |
| RV-REAL-010 | Scan station view | Choosing a scan station moves the camera to the scanner position; when the scan has a panorama, a full-resolution spherical view from that position is shown. | T2 | 3 |
| RV-REAL-011 | Points in clash tests | Point clouds can be side A or B of a clash test as point geometry, with a minimum point-count threshold per result to suppress noise (see [10-clash-detection.md](10-clash-detection.md)). | T2 | 3 |
| RV-REAL-012 | Model-to-scan deviation | A deviation analysis colors model surfaces or points by the signed distance between selected scene items and the point cloud, with a legend and a tolerance band, and reports items outside tolerance as a selection set. | T3 | 3 |

## Reality meshes and context

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-REAL-013 | 3D Tiles context | 3D Tiles tilesets (batched, instanced and glTF content) from files or a URL are streamed by screen-space error and placed by their stored geolocation. | T2 | 3 |
| RV-REAL-014 | Photogrammetry meshes | Textured meshes from glTF or OBJ are read as reality meshes; large meshes are tiled into a level-of-detail scene cache on conversion. | T2 | 3 |
| RV-REAL-015 | City models | CityJSON city models are read with their objects, semantic surfaces and attributes, with a chosen level of detail. | T3 | 3 |
| RV-REAL-016 | Terrain from surfaces | LandXML surfaces and point clouds of ground classes can be shown as a terrain mesh beneath the federation. | T3 | 3 |
| RV-REAL-017 | Reality data opacity and clipping | Reality meshes and point clouds have per-source opacity, and models can clip the context mesh where they overlap so new work stays visible. | T2 | 3 |

## Photos and panoramas

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-REAL-018 | Geolocated photos | Photos with position and orientation metadata are placed in the federation as markers; choosing one aligns the camera to the photo and overlays it with adjustable opacity. | T3 | 3 |
| RV-REAL-019 | Panoramas | Equirectangular panoramas are placed at their positions; entering one shows the panorama with the model overlaid at adjustable opacity. | T3 | 3 |
| RV-REAL-020 | Raster underlays in 3D | A raster image or aerial photo can be placed as a ground plane with a scale and position or a CRS-aware georeference. | T2 | 3 |
