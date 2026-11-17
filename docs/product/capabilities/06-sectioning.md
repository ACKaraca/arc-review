# Sectioning

Sectioning cuts the federated scene with planes or a box to expose interiors, and is saved into
viewpoints and issues. Cuts apply to meshes, lines, point clouds and reality meshes alike and can be
filled so cut solids read clearly.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Section state | Mode (planes or box), enabled flag, linked flag, cap settings | Per view; saved in viewpoints, issues and animation clips |
| Section plane | Name, enabled flag, origin, normal, offset | Up to six per section state |
| Section box | Center, orientation, size along three axes | One per section state in box mode |
| Cap settings | Fill on or off, fill color, outline color and width | Applied to cut surfaces |

## Planes

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SECT-001 | Enable sectioning | Sectioning toggles on and off per view without losing the defined planes or box. | T1 | 1 |
| RV-SECT-002 | Section planes | Up to six section planes are active at once; each has a name, an enabled flag and a direction, and geometry on the outside of any enabled plane is removed. | T1 | 1 |
| RV-SECT-003 | Align a plane | A plane can be aligned to a standard direction (top, bottom, front, back, left, right), to the view direction, to a picked face, or perpendicular to a picked edge. | T1 | 1 |
| RV-SECT-004 | Move and rotate planes | A plane is moved and rotated with an on-screen manipulator, by dragging its indicator, or numerically by offset and angles in display units. | T1 | 1 |
| RV-SECT-005 | Link planes | Linked planes move together as one rigid group, for example to slide a slice of fixed thickness through a building. | T2 | 1 |
| RV-SECT-006 | Section at level | A command places a horizontal plane at a chosen level plus a cut height, producing a plan cut. | T2 | 1 |
| RV-SECT-007 | Flip plane | Flipping a plane keeps its position and removes the opposite side. | T1 | 1 |

## Box

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SECT-008 | Section box | Box mode keeps only geometry inside an oriented box; each face can be dragged independently in the view. | T1 | 1 |
| RV-SECT-009 | Fit box to selection | Fits the box to the bounding box of the selected items plus a margin. | T1 | 1 |
| RV-SECT-010 | Transform the box | The box is moved, rotated and scaled with a manipulator or numeric values; rotation about the vertical axis aligns it with a skewed building grid. | T2 | 1 |

## Display and use

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-SECT-011 | Section caps | Closed solids cut by a plane or box show a filled cap in a configurable color with an outline; open meshes are cut without a cap. | T2 | 1 |
| RV-SECT-012 | Plane indicators | Plane and box outlines are shown or hidden by a setting, with configurable colors per plane. | T2 | 1 |
| RV-SECT-013 | Section point clouds and reality meshes | Section planes and boxes cut point clouds and reality meshes in the same way as model geometry. | T2 | 3 |
| RV-SECT-014 | Save section in viewpoints | A viewpoint stores the current section state; restoring the viewpoint restores it exactly. Section planes export to BCF clipping planes. | T1 | 1 |
| RV-SECT-015 | Section scoped to sets | Sectioning can be limited to chosen model sources or sets so context items such as a site or a point cloud stay uncut. | T3 | 2 |
| RV-SECT-016 | Section outline export | The cut outline of the current section plane can be exported as DXF or SVG for reference in drawings. | T3 | 3 |
| RV-SECT-017 | Animate sections | Section planes and boxes can be keyframed in animation clips (see RV-VIEW-021). | T2 | 4 |
