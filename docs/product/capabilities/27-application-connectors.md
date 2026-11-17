# Application connectors

Connectors bring models from authoring applications whose file formats have no published
specification into Arc Review, and let a reviewer jump back to the element or view in the authoring
application. A connector is a small add-in that runs inside the user's own licensed copy of the other
application, uses only that application's official public API and writes the Arc exchange package or
a scene cache. The decision is recorded in suite
[ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md);
the package is specified in
[`interchange.md`](https://github.com/ACKaraca/arc-core/blob/main/docs/architecture/interchange.md).

Connectors are developed in the separate `arc-connectors` repository in the language each host
requires; they are not part of this repository and not Arc desktop applications. This file specifies
what Arc Review expects from them and what it does with their output. A connector never modifies the
model of the application it runs in: it reads the model, and its switch-back requests change only
selection and view. Reverse engineering of undocumented formats stays excluded; see
[interoperability](../../architecture/interoperability.md).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Connector | Id, version, host kind and supported host versions, exchange package version, capabilities (export, re-export on save, batch, switch-back) | Runs inside a host application; produces exchange packages or scene caches |
| Exchange package | Open container with geometry, semantics, parameters, types, identifiers and relationships; declared unit and origin | Read as one model source |
| Connector link | Model source id, host kind, project file reference, connector id, last export revision and time, switch-back endpoint (when the host is running) | Stored with a model source whose origin is a connector |
| Export scope | Whole model, selected elements, a named view, a phase or design option | Chosen in the host; recorded in the package |
| Switch-back request | Kind (select, view), source element identifiers, optional camera, section and hidden elements | Sent from Arc Review to a connector over a local endpoint |
| Licensed bridge | Library kind, user-supplied path, detected version, enabled flag | Converts bytes to an exchange package or scene cache in a separate process |

## Export from the authoring application

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CONNECT-001 | Export through the public API | A connector exports the open project of its host application through that application's official public API to an Arc exchange package, carrying geometry, element semantics, parameters, types, identifiers and relationships. Nothing is read from the host's project file on disk. | T3 | 5 |
| RV-CONNECT-002 | Exchange package as model source | Arc Review opens an exchange package as a model source at data fidelity. The item key is the model source id plus the host element identifier recorded in the package; key quality is reported as for any source (RV-FED-041). | T3 | 5 |
| RV-CONNECT-003 | Direct scene cache | A connector may write a scene cache instead of an exchange package. The cache is content addressed like any other ([ADR 0003](../../adr/0003-content-addressed-scene-cache.md)), declares its origin and writer version, and is validated on load; a malformed cache is rejected with a reason and the source is reported as unreadable. | T3 | 5 |
| RV-CONNECT-004 | Export scope | The user exports the whole model, the current selection, one named view or one phase or design option. The scope is recorded in the package and shown in the source information. | T3 | 5 |
| RV-CONNECT-005 | Host identifiers as item keys | Persistent element identifiers of the host become source element identifiers. Elements without a persistent identifier get a derived key flagged as such (see the identity rules in the [domain model](../../architecture/domain-model.md)). | T3 | 5 |
| RV-CONNECT-006 | Export loss report | Every export lists elements, parameters and geometry the package cannot carry, with counts and examples, in the same loss report format as other conversions (RV-FED-035). | T3 | 5 |
| RV-CONNECT-007 | Version compatibility | A connector declares the host versions and the exchange package version it supports. Arc Review reads every earlier minor version of the package, and refuses a newer major version with a message naming the connector to update. | T3 | 5 |

## Keeping the federation current

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CONNECT-008 | Re-export on save | When enabled in the connector, each save of the host project triggers a background export to the same package location, so the model source becomes changed without any manual step. The host stays responsive during the export. | T3 | 5 |
| RV-CONNECT-009 | Incremental export | A connector that can see which elements changed since its last export writes only those elements and removals, with the export revision they apply to; Arc Review applies them to the previous scene cache and reports them in the refresh report (RV-FED-016). A gap in revisions triggers a full export. | T3 | 5 |
| RV-CONNECT-010 | Refresh from connector exports | A package re-exported by a connector follows the watch and refresh policies of RV-FED-017 and RV-FED-015; item keys survive the refresh through host identifiers. | T3 | 5 |
| RV-CONNECT-011 | Batch export | A connector can export a list of project files in sequence through the host's own batch facilities, using a job definition that names the files, scope and output folder. The run produces one package per project and a result table with success, duration and loss report per file. | T3 | 5 |
| RV-CONNECT-012 | Batch export from the CLI | `arc-review-cli` can start a batch export on a machine where the host application is installed and licensed, then convert, refresh and run checks on the results in one job (see [25-automation.md](25-automation.md)). | T3 | 5 |

## Switch-back to the authoring application

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CONNECT-013 | Select in the authoring application | For an item, a selection, a clash result or an issue, Arc Review asks the connector of the owning model source to select the elements by their source identifiers and bring them into view. The request uses the selection exchange of RV-AUTO-010. | T3 | 5 |
| RV-CONNECT-014 | Match the view | A request can carry the camera, section and hidden elements of a viewpoint; the connector shows them in a temporary three-dimensional view of the host, so the author sees what the reviewer saw. The connector never changes the host's saved views unless the author saves one. | T3 | 5 |
| RV-CONNECT-015 | Clash and issue switch-back | A clash result selects both elements, each in the host that owns it. An issue selects the referenced components and applies its viewpoint. When the two elements belong to different hosts, each connector receives only its own element. | T3 | 5 |
| RV-CONNECT-016 | Local, authenticated endpoint | A connector in a running host listens on a loopback endpoint only. Arc Review authenticates to it with a per-session token that the connector shows to the author on first use. Requests carry identifiers and view parameters only, never geometry or review data. | T3 | 5 |
| RV-CONNECT-017 | Availability and fallback | When the host is not running, the switch-back actions are disabled with the reason. Arc Review never starts a host application without confirmation. Without a connector the actions fall back to copying the source identifiers and exporting a BCF topic. | T3 | 5 |

## Licensed bridges

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-CONNECT-018 | Licensed bridge registry | The user registers a conversion library that they have licensed from its vendor or consortium by giving its install path. Arc Review detects the version and runs it as a separate process behind the byte-level bridge of RV-FED-032. The library is never distributed with Arc Review and no core workflow requires it. | T3 | 5 |
| RV-CONNECT-019 | Bridge output and limits | A bridge returns an exchange package or a scene cache; the loss report records the bridge and its version. The process runs with memory and time limits, and a failure fails that source only. | T3 | 5 |
| RV-CONNECT-020 | Connector and bridge inventory | A panel lists installed connectors, detected hosts and registered bridges with versions and the formats each provides, and states for any unread file which of them could open it. | T3 | 5 |
