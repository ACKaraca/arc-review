# Appearance and visibility

Appearance controls what is visible and how it looks during review: hiding and isolating, items that
must always draw, color and transparency overrides, temporary transforms, rule-based coloring and
display modes. All of it lives in the review document as a layer over the sources and is saved into
viewpoints.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Visibility state | Per item: visible, hidden or required | Saved in viewpoints; evaluated with section state |
| Appearance override | Item keys, color, transparency, optional material | Saved in viewpoints; applied above appearance rules |
| Transform override | Item keys, translation, rotation, scale, pivot | Review-only displacement; saved in viewpoints and animation clips |
| Appearance rule set | Name, ordered rules, enabled flag | Stored in the review document; exportable |
| Appearance rule | Input (query, set or property), color, transparency, legend label | Belongs to a rule set; later rules win |
| Display settings | Shading mode, lighting, background, primitive toggles, edges, textures | Per view; saved in viewpoints |

## Visibility

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-LOOK-001 | Hide and show | Hide makes the selected items invisible and non-pickable; show makes them visible again. Hidden items still take part in clash, check and takeoff unless a scope excludes hidden items. | T1 | 1 |
| RV-LOOK-002 | Isolate | Isolate hides every item that is not selected; a variant makes the unselected items faint and non-pickable instead of hiding them. | T1 | 1 |
| RV-LOOK-003 | Show all | Show all clears every hidden state in the current view. | T1 | 1 |
| RV-LOOK-004 | Required items | Items marked as required are always drawn, even while the renderer drops detail to keep the frame rate during motion; the mark is cleared by a command and saved in viewpoints. | T2 | 1 |
| RV-LOOK-005 | Visibility by model source and folder | Model sources, federation folders, layers and levels can be hidden or shown from the federation list and the tree as a whole. | T1 | 1 |
| RV-LOOK-006 | Visibility by category | A category panel lists the distinct element categories or classes in the federation with counts and toggles visibility of each. | T2 | 1 |
| RV-LOOK-007 | Lock items | Locked items remain visible but cannot be picked or moved by transform overrides, so context models do not get in the way. | T2 | 1 |

## Overrides

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-LOOK-008 | Color override | Assigns a color to the selected items; the override replaces the source color until reset. | T1 | 1 |
| RV-LOOK-009 | Transparency override | Assigns a transparency from opaque to fully transparent to the selected items; transparent items remain pickable unless locked. | T1 | 1 |
| RV-LOOK-010 | Transform override | Moves, rotates or scales the selected items with an on-screen manipulator or numeric input about a chosen pivot, for example to test an alternative equipment position. The source is not changed and the override is marked in the tree and inspector. | T2 | 1 |
| RV-LOOK-011 | Move by measured vector | Moves the selected items by the vector of the last point-to-point measurement as a transform override. | T2 | 1 |
| RV-LOOK-012 | Reset overrides | Commands reset color, transparency, transform or all overrides for the selected items or for the whole document. | T1 | 1 |
| RV-LOOK-013 | Override precedence | The displayed appearance is resolved in a fixed order: source appearance, then appearance rules, then simulation appearance from 4D or animation, then manual overrides, then selection highlight. The inspector shows which layer set the current color. | T1 | 1 |

## Appearance rules

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-LOOK-014 | Rule-based appearance | An appearance rule set applies colors and transparency to items matched by queries or sets, evaluated in order so later rules override earlier ones. Rules re-evaluate automatically after a refresh. | T2 | 1 |
| RV-LOOK-015 | Color by property value | A rule can color by every distinct value of a property, generating one color per value from a palette; numeric and quantity values can instead use a continuous color ramp between chosen limits. | T2 | 1 |
| RV-LOOK-016 | Apply, clear and switch rule sets | A document holds several rule sets; one or none is active per view, switching is instant and the active rule set is saved in viewpoints. | T2 | 1 |
| RV-LOOK-017 | Legend | The active rule set shows a legend of colors, labels and matched item counts; the legend can be shown as an overlay in the view and in exported images. | T2 | 1 |
| RV-LOOK-018 | Exchange rule sets | Rule sets can be exported to and imported from an open rule file for reuse between projects. | T2 | 1 |
| RV-LOOK-019 | Unmatched items | A rule set can make items not matched by any rule faint, hidden or unchanged. | T2 | 1 |

## Display

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-LOOK-020 | Shading modes | Views draw in shaded, shaded with edges, wireframe, hidden line and textured realistic modes. | T1 | 1 |
| RV-LOOK-021 | Lighting | Lighting uses a camera headlight, scene lights from sources, or image-based environment lighting; ambient and headlight intensity are adjustable. | T1 | 1 |
| RV-LOOK-022 | Background | The background is a solid color, a vertical gradient, or a sky and ground horizon. | T1 | 1 |
| RV-LOOK-023 | Primitive toggles | Surfaces, lines, points, snap points and text can be shown or hidden independently. | T1 | 1 |
| RV-LOOK-024 | Textures and materials | Source textures and materials can be turned off to show flat colors; texture resolution is capped by a memory setting. | T1 | 1 |
| RV-LOOK-025 | Edges and ambient occlusion | Feature edges, silhouette edges and screen-space ambient occlusion can be enabled to improve shape reading. | T2 | 1 |
| RV-LOOK-026 | Ground plane and shadows | An optional ground plane at the lowest level or a chosen elevation receives soft shadows from a directional light. | T3 | 2 |
| RV-LOOK-027 | Explode view | Explode displaces items away from the center of the selection or the scene by a slider amount, by assembly level for STEP sources and by storey for IFC sources, without changing placements. | T3 | 2 |
| RV-LOOK-028 | High-contrast and color-safe palettes | Selection highlight, clash colors and generated rule palettes have high-contrast and color-vision-safe variants that follow the application theme. | T1 | 1 |
