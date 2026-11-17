# Rendering and presentation

Presentation turns the federated scene into images and walkthroughs for clients and stakeholders.
Photoreal frames come from RenderBuilder through the Arc Scene contract: Arc Review exports the
scene and streams updates, and frames return over local IPC; Arc Review never links the path
tracer (see the suite ADRs at <https://github.com/ACKaraca/arc-suite/tree/main/docs/adr>).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Presentation material | Name, OpenPBR-aligned parameters, textures | Assigned to scene items by query or selection; stored in the review document |
| Light | Kind (point, spot, directional, area), position, direction, color, intensity in photometric units | Stored in the review document; exported in the Arc Scene |
| Sun and sky | Location (latitude, longitude, time zone), date, time, sky model, intensity | One per review document; can be animated over time |
| Render job | Arc Scene snapshot, camera, resolution, quality preset, output | Sent to a local RenderBuilder process or a Hub render worker |
| Render preset | Name, samples or time limit, denoise flag, resolution scale | Used by render jobs |

## Materials and lighting

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-REND-001 | Presentation materials | Presentation materials are assigned to scene items by selection, set or query, overriding source materials for presentation only. Assignments are stored in the review document and show in the shaded viewport as an approximation. | T2 | 4 |
| RV-REND-002 | Material library | A material library of OpenPBR-aligned materials ships with the application; user materials are saved as files that other Arc products read. | T2 | 4 |
| RV-REND-003 | Material rules | Material assignment rules map property values (for example material name or category) to presentation materials and reapply when model sources refresh. | T3 | 4 |
| RV-REND-004 | Lights | Point, spot, directional and area lights can be placed, moved and edited with photometric intensity; lights are stored in the review document. | T2 | 4 |
| RV-REND-005 | Sun and sky | The sun position follows the location, date and time, using the federation's CRS record when present; the sky model and intensity are adjustable. | T2 | 4 |
| RV-REND-006 | Sun lighting in the viewport | The real-time viewport can be lit by the sun and sky of RV-REND-005, casting shadows that update as the date and time change, so shadow studies run without a render job. Other viewport lighting modes are specified in RV-LOOK-021. | T2 | 4 |

## Rendering through RenderBuilder

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-REND-007 | Interactive render view | When RenderBuilder is installed, a render view shows a progressively refining photoreal frame of the current camera; camera, visibility, section and material changes stream to the renderer and restart refinement. | T2 | 4 |
| RV-REND-008 | Render a still | A render job renders the current view or a viewpoint at a chosen resolution with a render preset and saves PNG, TIFF or EXR. Without RenderBuilder installed, the command explains how to install it. | T2 | 4 |
| RV-REND-009 | Render region | A rectangular region of the view can be rendered alone for checking materials and lighting. | T3 | 4 |
| RV-REND-010 | Render presets | Named render presets range from a fast draft to a final quality and can be edited and shared as files. | T2 | 4 |
| RV-REND-011 | Rendered animation frames | Camera paths, animation clips and simulations can be rendered frame by frame by RenderBuilder for image sequence and video export. | T3 | 4 |
| RV-REND-012 | Hub render workers | A render job can be submitted to a self-hosted Arc Hub render worker instead of a local process, and its frames are downloaded when complete. | T3 | 4 |
| RV-REND-013 | Arc Scene export | The federation, with visibility, presentation materials, lights and cameras, exports as an Arc Scene file that RenderBuilder and other Arc products open. | T2 | 4 |

## Presentation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-REND-014 | Full-screen presentation mode | Presentation mode shows the view full screen without panels; keys step through viewpoints of a chosen folder or play camera paths, and a key returns to the workspace. | T2 | 1 |
| RV-REND-015 | Presenter on a second display | Presentation mode can run on a second display while the workspace stays on the first, showing the next viewpoint and notes to the presenter. | T3 | 4 |
| RV-REND-016 | High-quality viewport capture | A capture renders the viewport off-screen at up to several times the display resolution with supersampling, ambient occlusion and soft shadows, without RenderBuilder. | T2 | 2 |
| RV-REND-017 | Environment image | An equirectangular high-dynamic-range image can be set as the presentation environment; it provides background, reflections and image-based lighting in the viewport and is passed to RenderBuilder with the Arc Scene. Plain backgrounds are specified in RV-LOOK-022. | T2 | 4 |
