# Navigation

Navigation moves the camera through federated scenes from a whole campus down to a single valve,
with modes for inspection from outside and for moving through buildings at human scale. All modes
work from mouse, keyboard, touch and game controllers, and keep precision at georeferenced
coordinates.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Camera | Position, orientation, up vector, projection (perspective or orthographic), field of view or view width, near and far planes, focal distance | Owned by a view; stored in viewpoints |
| Navigation mode | Orbit, constrained orbit, free orbit, turntable, look around, pan, zoom, walk, fly | Active per view |
| Motion settings | Linear speed, angular speed, acceleration | Per view; stored in viewpoints |
| Avatar | Model, height, radius, eye height, third-person distance and angle | Used by walk and fly; shown in third person and XR |
| Realism settings | Collision, gravity, auto crouch, third person | Per view; stored in viewpoints |

## Inspection modes

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-NAV-001 | Orbit | Dragging rotates the camera around a pivot with the up axis kept vertical. The pivot is the point under the cursor when the drag starts, or the center of the selection when a setting asks for it. | T1 | 1 |
| RV-NAV-002 | Free orbit | Free orbit rotates around the pivot without keeping the up axis, allowing roll; a level-camera command restores a vertical up axis. | T2 | 1 |
| RV-NAV-003 | Turntable | Turntable rotates the model about its own center and vertical axis while the camera stays fixed, for inspecting an object. | T2 | 1 |
| RV-NAV-004 | Look around | Look around rotates the view about the camera position without moving it. | T1 | 1 |
| RV-NAV-005 | Pan | Pan moves the camera parallel to the view plane so the point under the cursor follows the cursor. | T1 | 1 |
| RV-NAV-006 | Zoom | Wheel and drag zoom move toward the point under the cursor; in orthographic projection they change the view width. Zoom speed adapts to the distance to the nearest surface so movement stays controllable near small objects. | T1 | 1 |
| RV-NAV-007 | Zoom to extents, window and selection | Commands frame the whole visible scene, a rectangle drawn by the user, or the selected items with a margin. | T1 | 1 |
| RV-NAV-008 | Focus and look at | Focus moves the pivot to a picked point and centers it; look at turns the camera to face a picked point without moving its position. | T2 | 1 |
| RV-NAV-009 | Default mouse bindings | Middle-drag pans, wheel zooms and a modified middle-drag orbits, without entering a mode; holding a modifier temporarily switches to orbit from any mode. Bindings are configurable. | T1 | 1 |

## Moving through the model

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-NAV-010 | Walk | Walk moves the camera on a horizontal plane with forward, back, strafe and turn from keys or mouse, and up or down by separate keys. Speed is shown and changes with the wheel or keys. | T1 | 1 |
| RV-NAV-011 | Fly | Fly moves freely in the direction the camera faces, with pitch and yaw from the mouse, for flying over sites and through large spaces. | T1 | 1 |
| RV-NAV-012 | Collision | With collision on, the camera is treated as an avatar volume and stops or slides along geometry instead of passing through it. Items marked as non-colliding by a set are ignored. | T1 | 1 |
| RV-NAV-013 | Gravity | With gravity on in walk mode, the avatar falls until it stands on a surface and follows floors, ramps and stairs up to a configurable step height. | T1 | 1 |
| RV-NAV-014 | Auto crouch | With auto crouch on, the avatar lowers its height to pass under obstacles lower than its standing height and stands again when clear. | T2 | 1 |
| RV-NAV-015 | Third person | Third person shows the avatar model and places the camera behind it at a configurable distance and angle, so the user judges clearances at human scale. | T2 | 1 |
| RV-NAV-016 | Avatar settings | Avatar height, radius, eye height and appearance are configurable; a set of avatar models (person, wheelchair user, small vehicle) is provided and custom glTF avatars can be loaded. | T2 | 1 |
| RV-NAV-017 | Teleport | Double-clicking a floor surface in walk mode moves the avatar there and keeps its eye height. | T2 | 1 |
| RV-NAV-018 | Motion speed | Linear and angular speed are set per view, scaled automatically to the scene size on first open, and stored in viewpoints. | T1 | 1 |
| RV-NAV-019 | Game controller and 3D mouse | Game controllers and six-degree-of-freedom input devices drive walk, fly and orbit with configurable axis mapping and dead zones. | T3 | 2 |
| RV-NAV-020 | Touch navigation | On touch screens, one-finger drag orbits, two-finger drag pans, pinch zooms and twist rotates. | T2 | 2 |

## Orientation aids

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-NAV-021 | Orientation cube | An orientation cube in a view corner shows the current orientation; clicking a face, edge or corner animates to that standard view, and dragging it orbits. It includes a compass ring showing the federation's north. | T1 | 1 |
| RV-NAV-022 | Standard views | Commands set the camera to top, bottom, front, back, left, right and isometric views relative to the project orientation or to true north. | T1 | 1 |
| RV-NAV-023 | Home view | A home view stored in the review document can be set from the current camera and restored by one command; a reset command returns to the extents view. | T1 | 1 |
| RV-NAV-024 | Navigation toolbar and radial menu | A navigation toolbar collects the navigation modes, and a radial menu at the cursor offers orbit, pan, zoom, look, walk, rewind and center without moving the pointer away from the work. | T2 | 1 |
| RV-NAV-025 | Rewind | Rewind steps back through recent camera positions shown as thumbnails, so the user can return to an earlier view. | T2 | 1 |
| RV-NAV-026 | Plan and section minimap | A minimap pane shows a plan and a section of the scene with the camera position and view direction; clicking or dragging in it moves the camera. It follows the current level when grids and levels exist. | T2 | 1 |
| RV-NAV-027 | Jump to level | Choosing a level from the level list moves the camera to that level at eye height above its elevation, keeping the plan position. | T2 | 1 |
| RV-NAV-028 | Overlays | Optional overlays show the world axes, camera coordinates in project or survey coordinates, the current level and nearest grid intersection, a reference grid, frame rate and rendering statistics. | T2 | 1 |

## Camera

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-NAV-029 | Projection | The camera switches between perspective and orthographic projection while keeping the framed area. | T1 | 1 |
| RV-NAV-030 | Camera values | The camera position, target, up vector, field of view and focal distance can be read and entered numerically in project or survey coordinates. | T2 | 1 |
| RV-NAV-031 | Align camera | Commands level the camera, look straight up or down, and align the view direction to the X, Y or Z axis or to a picked face normal. | T2 | 1 |
| RV-NAV-032 | Clip planes | Near and far clip distances are automatic by default, with options to constrain or fix them for very large or very small scenes. | T2 | 1 |
| RV-NAV-033 | Precision at large coordinates | Rendering, picking and navigation are camera-relative, so models placed at georeferenced coordinates far from the origin show no jitter or picking error. | T1 | 1 |
| RV-NAV-034 | Guaranteed frame rate during motion | While the camera moves, the renderer keeps a target frame rate by drawing small and distant items last and dropping them if needed, then completes the full frame when motion stops. Required items are always drawn (see RV-LOOK-004). | T1 | 1 |
| RV-NAV-035 | Keyboard-complete navigation | Every navigation mode and orientation command is available from the keyboard, with shortcuts listed in the command palette. | T1 | 1 |
