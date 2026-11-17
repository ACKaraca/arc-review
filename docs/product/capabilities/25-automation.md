# Automation and extensibility

Every Arc Review action is a command in one registry, exposed through menus, the command line and
palette, macro recording, the Arc Automation Protocol, an MCP endpoint and the headless CLI. Python
scripts, node graphs and sandboxed WebAssembly plug-ins extend the product through the same commands
(see [overview.md](../../architecture/overview.md)).

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Command | Stable id, label, parameter schema, enabled predicate, non-interactive run, optional prompt machine | Registered in the command registry |
| Macro | Recorded command sequence with parameters | Replays commands |
| Automation session | Transport (stdio or local socket), client, granted scope | Calls commands and queries over JSON-RPC |
| Plug-in package | Manifest (id, version, requested capabilities), WebAssembly module, assets | Installed per user; trust assigned by the host |
| Node graph | Nodes, connections, inputs, outputs | Evaluated by arc-nodes; may call commands |

## Command surface

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-AUTO-001 | One command registry | Every action in every area is a registered command with a machine-readable parameter schema; the same command runs from the UI, command line, scripts, automation, plug-ins and the CLI. | T1 | 1 |
| RV-AUTO-002 | Command line and palette | A command line and a searchable command palette run any command by name or alias with completion and parameter prompts. | T1 | 1 |
| RV-AUTO-003 | Macro recording | Command sequences are recorded with their parameters into macros that replay, edit and export as Python scripts. | T2 | 2 |
| RV-AUTO-004 | No dialogs in programmatic runs | Commands run programmatically never open a dialog; missing parameters return a structured error. | T1 | 1 |

## Protocols and CLI

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-AUTO-005 | Arc Automation Protocol | A running Arc Review serves JSON-RPC over stdio or a local socket for opening documents, running commands, querying the model tree and properties, and subscribing to selection and document events. | T1 | 1 |
| RV-AUTO-006 | MCP endpoint | A Model Context Protocol endpoint derived from the command registry lets assistant clients call commands with preview and explicit apply for changes. | T3 | 5 |
| RV-AUTO-007 | Headless CLI | `arc-review-cli` runs on Windows, macOS and Linux without a display, an account or a license server, with subcommands to convert, validate, append, refresh, run clash tests and checks, export reports and images, publish packages, plot sheets and run batch job definitions. | T1 | 1 |
| RV-AUTO-008 | CLI exit codes and output | The CLI returns documented exit codes and can write machine-readable JSON results to standard output for use in CI pipelines. | T1 | 1 |
| RV-AUTO-009 | CI result formats | Headless runs of checks, clash tests and comparisons (RV-CHECK-032, RV-CLASH-026) can write their outcome as JUnit XML and as a JSON summary, so continuous integration systems display failures as test results and keep the report as a build artifact. | T2 | 3 |
| RV-AUTO-010 | Selection exchange with other Arc applications | Arc Review answers protocol requests to read or set its selection by item key and can send the same request to a running Arc product or to a connector in an authoring application (see [27-application-connectors.md](27-application-connectors.md)); the request carries source element identifiers so the receiver resolves them without Arc Review types. Clash results use it through RV-CLASH-054. | T2 | 3 |

## Scripting, graphs and plug-ins

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-AUTO-011 | Python client | An official Python client of the automation protocol, running out of process, provides typed access to commands, the model tree, properties, sets, viewpoints, clash results, issues and takeoff. | T2 | 2 |
| RV-AUTO-012 | Script panel | A panel runs Python scripts through the client, showing output and errors; scripts never block the user interface. | T3 | 3 |
| RV-AUTO-013 | Node graphs | Node graphs on arc-nodes build queries, appearance rules, clash and check workflows and report pipelines from typed nodes, and are saved in the review document. | T3 | 4 |
| RV-AUTO-014 | WebAssembly plug-ins | Plug-ins are WebAssembly modules with a manifest; they have no ambient authority, run with memory and time limits, call only the commands their granted capabilities allow, and apply changes transactionally. | T2 | 3 |
| RV-AUTO-015 | Plug-in panels and tools | Plug-ins can add commands, panels built from host widgets, viewport overlays and custom property sets computed from items. | T3 | 3 |
| RV-AUTO-016 | Custom readers as plug-ins | A plug-in can provide a reader that converts a file format into a scene cache through a defined interface. | T3 | 3 |
| RV-AUTO-017 | Custom schedule and cost sources | A plug-in can provide a schedule source or cost source that supplies tasks or rates through a defined interface. | T3 | 3 |
| RV-AUTO-018 | Plug-in manager | A plug-in manager lists installed plug-ins with version, granted capabilities and trust, and enables, disables, updates and removes them. | T2 | 3 |
| RV-AUTO-019 | Shared settings | Application settings, including reader options, display options and quick-property definitions, export to and import from a documented file so teams share one configuration; a project folder setting is read on start. | T2 | 1 |
| RV-AUTO-020 | Environment variables in paths | Configured folder paths may contain environment variables, expanded when used. | T3 | 1 |
| RV-AUTO-021 | Assistant panel | An optional assistant panel, using a user-configured provider including local models, proposes commands for preview and explicit apply; no document content leaves the machine without explicit action. | T3 | 5 |
| RV-AUTO-022 | Localized interface | All interface strings come from message catalogues; English and Turkish are complete, and right-to-left layouts are supported. | T1 | 1 |
