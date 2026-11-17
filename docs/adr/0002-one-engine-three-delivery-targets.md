# 0002 One engine, three delivery targets

- Status: Accepted; extended by [0007](0007-mobile-is-a-fourth-target-of-the-same-engine.md) with a
  mobile target
- Scope: crate layering, `apps/`, `review-web`; see [delivery targets](../architecture/delivery-targets.md)

## Context

Arc Review serves three audiences. Coordinators need the complete desktop application. Recipients
of review results (clients, site staff, consultants) need to open what they are sent without
learning or installing a full coordination tool. Arc Hub users need to view models and issues in a
browser. Building three viewers would triple rendering, picking, property and markup code and let
their behavior drift apart; recipients would then see something different from what the
coordinator published.

## Decision

Arc Review is one engine delivered as three targets:

1. **Desktop application** (`apps/arc-review`): all commands, all panels, the analysis engines.
2. **Read-only viewer** (`apps/arc-review-viewer`): a separate, smaller executable built from the
   same engine and UI crates. It registers only commands that do not change a document, does not
   link the analysis crate, cannot save review data and honors package restrictions.
3. **WebAssembly viewer bundle** (`crates/review-web`, compiled for `wasm32-unknown-unknown`):
   consumed by Arc Hub's browser client. It renders with `arc-gpu` through wgpu on WebGPU with a
   WebGL2 fallback, streams scene caches from Hub and exposes a narrow, versioned programming
   interface to the host page.

The engine crates (`review-model`, `review-scene`, `review-io` and the read-only part of
`review-commands`) depend on no windowing, UI or GPU crate and build for native and WebAssembly
targets. Windowing and input come from `arc-app` and `winit` on the desktop and from the host page
in the browser. The browser host page and its JavaScript belong to Arc Hub; this repository ships
only Rust compiled to WebAssembly plus the generated bindings.

## Consequences

- A behavior is implemented once; the read-only viewer and the browser show exactly what the
  desktop shows for the same document revision.
- The layering gate must also verify that the engine crates compile for `wasm32-unknown-unknown`;
  threads, file system access and clocks are abstracted behind small traits in the engine.
- Feature differences between targets are expressed as command registration, never as forks of
  code paths; a test asserts the viewer registry contains no mutating command.
- Browser memory limits shape the streaming design for every target (see
  [large models](../architecture/large-models.md)).

## Alternatives considered

- **Viewer mode as a switch inside the desktop executable.** Rejected as the only form: recipients
  would install the full application with analysis engines, and a switch is easier to defeat than a
  registry that never contained the commands. The desktop application still offers a read-only
  window mode for convenience.
- **A separate web viewer written in TypeScript.** Rejected: duplicates the engine, violates the
  suite rule that viewers share one implementation, and drifts.
- **Server-side rendering streamed as video to the browser.** Rejected for the first releases:
  needs GPU servers for every viewer, adds latency and conflicts with self-hosting on modest
  hardware. Can be added later for very weak clients.

## Reopen when

A target needs a behavior that cannot be expressed through command registration and feature flags
of the shared crates, or WebAssembly limits (memory, threads) make the shared engine unable to meet
the browser performance targets.
