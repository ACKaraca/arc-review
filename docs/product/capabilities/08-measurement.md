# Measurement

Measurement takes distances, angles, areas and clearances in the federated 3D scene with exact
snapping to geometry, point clouds and reality meshes. Measurements can be kept, attached to
viewpoints and converted to markups; measuring on sheets is specified in the drawings area.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Measurement | Kind, picked points with snap kind, result values with units, label position | Temporary or saved in a viewpoint |
| Snap target | Vertex, edge, edge midpoint, edge nearest point, face, circle center, line end, point cloud point | Resolved from the pick |
| Constraint | Axis X, Y or Z, parallel, perpendicular, none | Applied while picking the next point |
| Measurement settings | Color, line width, display unit, precision, coordinate reference | User and document settings |

## Measure tools

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MEAS-001 | Point to point | Picking two points shows the straight distance and its X, Y and Z components in display units. | T1 | 1 |
| RV-MEAS-002 | Point to many points | Picking a base point and then several end points shows the distance from the base to each. | T2 | 1 |
| RV-MEAS-003 | Polyline length | Picking a chain of points shows each segment length and the total. | T1 | 1 |
| RV-MEAS-004 | Accumulated distance | Successive separate point-to-point measurements are added to a running total. | T2 | 1 |
| RV-MEAS-005 | Angle | Picking three points, or two edges or faces, shows the angle between them. | T1 | 1 |
| RV-MEAS-006 | Area | Picking a closed polygon of points shows its area and perimeter; picking a face shows the face area. | T1 | 1 |
| RV-MEAS-007 | Shortest distance between items | With two items or sets selected, shows the minimum distance between their geometry, the two closest points and the connecting vector. | T1 | 1 |
| RV-MEAS-008 | Point coordinates | Picking a point shows its coordinates relative to the project base point, survey point or world origin, and in the project CRS when one is defined. | T1 | 1 |
| RV-MEAS-009 | Radius and diameter | Picking a circular edge or a cylindrical face shows its radius, diameter and center. | T2 | 1 |
| RV-MEAS-010 | Item dimensions and volume | With an item selected, shows its bounding box dimensions in project axes and, for closed geometry, its volume and surface area. | T2 | 1 |
| RV-MEAS-011 | Vertical clearance | Picking a point measures the vertical distance to the surface above and below it, for headroom and clearance checks. | T2 | 1 |

## Snapping and constraints

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MEAS-012 | Snapping | Picks snap to vertices, edges, edge midpoints, nearest points on edges, faces, circle centers and line ends; the snap kind is shown by a marker and each kind can be turned on or off. | T1 | 1 |
| RV-MEAS-013 | Point cloud snapping | Picks snap to the nearest point cloud point within a screen tolerance, or to a plane fitted to nearby points. | T2 | 3 |
| RV-MEAS-014 | Axis and direction constraints | While picking, the measurement can be constrained to the X, Y or Z axis, or parallel or perpendicular to a picked edge or face. | T2 | 1 |
| RV-MEAS-015 | Precision at large coordinates | Measurements are computed in double precision in model coordinates, so results at georeferenced coordinates are exact to the source precision. | T1 | 1 |

## Results

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-MEAS-016 | Measurement display | Measurements are drawn in the view with dimension lines and labels whose color, line width, unit and decimal places are settings. | T1 | 1 |
| RV-MEAS-017 | Keep measurements | Measurements remain until cleared; a clear command removes all temporary measurements. | T1 | 1 |
| RV-MEAS-018 | Save with viewpoint | Converting measurements to markup stores them on the current viewpoint as dimension markups that keep their values. | T2 | 1 |
| RV-MEAS-019 | Copy results | Measurement results can be copied as text or a table. | T2 | 1 |
| RV-MEAS-020 | Move items by measurement | The last point-to-point vector can be applied as a transform override to the selected items (see RV-LOOK-011). | T2 | 1 |
| RV-MEAS-021 | Measure in the viewer mode | All measure tools are available in the read-only viewer mode; results can be shown but not saved to the document. | T1 | 1 |
