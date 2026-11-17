# 0007 Mobile is a fourth target of the same engine

- Status: Accepted
- Scope: `apps/`, `review-commands` registries, `review-ar` (created in phase 5); extends
  [ADR 0002](0002-one-engine-three-delivery-targets.md); see
  [delivery targets](../architecture/delivery-targets.md) and
  [mobile capabilities](../product/capabilities/28-mobile.md)

## Context

Site staff, inspectors and owners review and mark up on tablets and phones, often without a
connection. Suite
[ADR-0018](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0018-web-and-mobile-targets.md)
allows tablet and phone builds of the same Rust crates on iOS and Android. Arc Review has to decide
what a mobile build contains: the read-only set is too little for field markups and issues, and the
analysis engines are too heavy and not needed on a device.

## Decision

1. Mobile is a fourth delivery target built from the engine crates, `review-ui` and `arc-app`. It
   adds `apps/arc-review-mobile`, which is wiring only.
2. Its command registry is the read-only registry plus the review-data commands for markups,
   comments, issues, measurements saved in viewpoints, progress records and photo attachments. It
   contains no command that runs an analysis engine, edits federation placement or writes a source,
   and a registry test asserts this.
3. `review-analysis` is not linked. Stored results of clash tests and checks are shown; runs happen
   on the desktop, in the CLI or as Hub jobs.
4. The augmented-reality overlay needs device camera and motion tracking. It lives in a new crate,
   `review-ar`, created in phase 5, so that platform tracking stays behind one boundary and the
   engine crates remain free of platform dependencies.
5. Offline use is by downloaded packages and the existing offline queue of the Hub integration; no
   mobile-specific sync protocol is introduced.

## Consequences

- A behavior is still implemented once; mobile differs from the desktop only by registration and
  input mapping.
- The engine must run within mobile memory and thermal limits, which tightens the streaming budgets
  that every target shares (see [large models](../architecture/large-models.md)).
- Releasing on mobile involves platform store rules; those are a release matter outside these
  documents.

## Alternatives considered

- **Reuse the read-only registry unchanged.** Rejected: field markups and issues are the reason for
  a mobile build.
- **Include the analysis engines.** Rejected: memory and battery cost for functions used at a desk.
- **A separate native mobile application per platform.** Rejected: duplicates the engine and lets
  behavior drift, the reason for ADR 0002.

## Reopen when

Field users need an analysis function on a device, or a platform's rules prevent shipping the shared
engine.
