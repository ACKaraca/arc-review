# Glossary

Arc Review vocabulary, one line per term. Suite-wide terms are defined in
[`arc-suite`](https://github.com/ACKaraca/arc-suite).

| Term | Meaning |
|---|---|
| Alignment suggestion | A proposed placement correction for a model source that appears misplaced or wrongly scaled. |
| Animation clip | Keyframed transforms, colors and transparency applied to groups of items over time. |
| Appearance override | A color, transparency or transform applied to items for review, without changing the source. |
| Appearance rule set | An ordered list of rules that color items by query or by property value. |
| Attachment link | A URL, file, viewpoint or issue attached to a scene item and shown as an icon. |
| Attachment rule | A rule that attaches items to 4D tasks by matching a task field to item properties. |
| BCF | BIM Collaboration Format; the open standard Arc Review uses natively for issues. |
| Camera path | An animated camera built from viewpoints or recorded navigation. |
| Check finding | One rule violation reported by a check rule set, with stable identity across runs. |
| Check rule set | A named collection of parameterized model-checking rules run together. |
| Clash group | Clash results combined manually or automatically into one unit of work. |
| Clash result | One detected conflict between two items, identified by its test and item key pair. |
| Clash rule | A rule that excludes item pairs from a clash test. |
| Clash run | One execution of a clash test, recording the source versions and settings used. |
| Clash test | Definition of a clash check: sides A and B, type, tolerance, rules and options. |
| Comment thread | Ordered comments with a status, attached to a review object. |
| Comparison run | A comparison between two versions of a model source or two sheets, producing difference records. |
| Content key | The hash of source bytes, reader version and reader options that identifies a scene cache. |
| Conversion setup | Saved settings for converting drawings to another DWG or DXF version. |
| Coverage validation | The check that every task has items and every relevant item has a task. |
| Derived key | An item key computed from path and geometry when a source provides no element identifier. |
| Difference record | One added, removed, changed or moved item, or one changed region of a sheet. |
| Drawing source | A DWG, DXF, PDF or raster file read as sheets. |
| External data link | A connection that adds read-only properties from a table matched by a key column. |
| Federation | The ordered set of model sources combined into one scene in a review document. |
| File list | A plain-text file naming model paths, one per line. |
| Frame governor | The renderer policy that lowers detail during motion to hold the frame rate. |
| IDS | Information Delivery Specification; machine-readable information requirements. |
| Information requirement | An IDS specification held in the review document and validated as check rules. |
| Interaction script | Events, conditions and actions that make a review document respond to the viewer. |
| Issue | A BCF topic with fields, comments and viewpoints, synchronized with Arc Hub. |
| Item key | The stable identity of a scene item: model source identifier plus source element identifier. |
| Key quality | The share of a source's items that carry source identifiers, and the duplicates found. |
| Linked review document | A review document that references its sources and stores only review data. |
| Markup | Redline geometry drawn on a viewpoint or a sheet. |
| Markup set | A named collection of sheet markups with author and status. |
| Model source | One source file or Hub version in a federation, with unit, placement and reader options. |
| Model tree | The hierarchy of scene items under each model source. |
| Packaged review document | A review document with the scene caches of its sources embedded for hand-off. |
| Pick level | The level of the hierarchy that a click in the scene selects. |
| Placement | Unit scale, rigid transform and optional CRS alignment of a model source. |
| Plot configuration | Device, paper, scale, plot style table and stamp used to print sheets. |
| Publish profile | Saved settings that produce a packaged review document with metadata and restrictions. |
| Query | Condition rows grouped with AND and OR that describe a set of items. |
| Read-only viewer | The `arc-review-viewer` executable that opens and plays review data without changing it. |
| Reality mesh | Textured mesh from photogrammetry or 3D Tiles shown as context. |
| Required item | An item that is never dropped by the frame governor. |
| Review document | The `.arcrev` document holding the federation manifest and all review data. |
| Scene cache | The derived, content-addressed conversion of one model source used for display and analysis. |
| Scene item | A node of the model tree: source root, container, object or geometry. |
| Search set | A saved query whose membership is re-evaluated when the federation changes. |
| Section state | Up to six section planes or one section box, with caps. |
| Selection set | A saved, fixed list of item keys. |
| Sheet | One layout or page of a drawing source, with calibration and optional placement in 3D. |
| Sheet calibration | A two-point scale definition that makes measurement on PDF and raster sheets exact. |
| Sheet set | An ordered, numbered list of sheets from several drawing sources. |
| Simulation | Playback of task states over time in planned, actual or comparison mode. |
| Tag | A numbered leader note with a comment thread, placed in a viewpoint. |
| Takeoff catalog | Takeoff items with units and formulas, and resources with rates. |
| Takeoff record | One measured quantity from scene items, sheet geometry or a virtual entry. |
| Task type | A 4D task category defining item appearance at start, during and after a task. |
| Viewpoint | A saved camera with an optional bundle of visibility, appearance, section and markup state. |
| Visibility state | Per-item hidden, visible or required state. |
| XR session | A live review session shared by headset and desktop participants. |
