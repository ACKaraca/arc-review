# AGENTS.md

Arc Review is the model and drawing review application of the Arc suite: federation, clash
detection, model checking, 4D and 5D, quantity takeoff, markup, sheets and immersive review.
It reads source models and never edits them; review data lives in its own `.arcrev` document.

## Status and where to start

Design phase: no code and no builds exist. Read in this order: [README.md](README.md),
[docs/product/vision.md](docs/product/vision.md), [docs/architecture/overview.md](docs/architecture/overview.md),
[docs/architecture/domain-model.md](docs/architecture/domain-model.md), then the capability file of
the area you work on under [docs/product/capabilities](docs/product/capabilities/README.md).

## Repository layout (target)

```
crates/
  review-model/      review document schema, queries, rules (headless, native and wasm32)
  review-scene/      scene caches, spatial hierarchy, level of detail, streaming (headless, wasm32)
  review-analysis/   clash, checking, comparison, takeoff, simulation engines (headless, native)
  review-io/         format mapping, BCF, IDS, schedules, reports (headless)
  review-commands/   read-only and mutating command registries (headless)
  review-ui/         panels and tools on arc-ui
  review-web/        WebAssembly viewer bundle for Arc Hub
  review-xr/         OpenXR immersive review (created in phase 5)
apps/
  arc-review/        desktop application (wiring only)
  arc-review-viewer/ read-only viewer (wiring only)
  arc-review-cli/    headless CLI and automation protocol server
docs/  xtask/
```

## Commands

No build exists yet. Once code lands, these apply:

```
cargo fmt --check
cargo clippy --workspace --all-targets -- -D warnings
cargo nextest run --workspace
cargo xtask layers
```

## Suite rules

These rules apply in every Arc repository. The canonical text is
[`docs/engineering/agent-rules.md`](https://github.com/ACKaraca/arc-suite/blob/main/docs/engineering/agent-rules.md)
in `arc-suite`; change it there first.

1. **A person is accountable.** Whoever runs an agent answers for what it submits.
2. **Start from an issue.** Search issues and pull requests, check that nobody else is assigned,
   assign yourself and set the status to *In Progress* before changing files. When you finish,
   comment with what changed, the commit or pull request link and open items, then update the status.
   *Done* requires met and verified acceptance criteria.
3. **Verify, do not guess.** Open a definition before using it. Confirm a cause in the code.
4. **The document is the authority and everything is a command.** No change reaches a document
   except through a validated command inside a transaction. Programmatic paths never open dialogs.
5. **Respect the layers.** A crate depends only on lower layers. Nothing below the UI layer depends
   on windowing, GPU or UI crates. Products never depend on other products.
6. **Never crash, never lose work.** No `unwrap`, `expect`, `panic!`, `todo!` or `unreachable!` in
   production code. No `unsafe` outside the FFI crates named in an ADR. Treat every file and message
   as hostile: bound recursion, allocation and counts. A crash fix ships with a regression test.
7. **Clean room.** Implement from published specifications, observed behavior and original
   reasoning. Never copy or consult another product's code, decompiled output, file-format reverse
   engineering, documentation text, icons, fonts, templates or sample files. Do not name our
   features after another company's features.
8. **Licenses.** Add a dependency only if its license is on the allowlist in
   `docs/engineering/dependency-policy.md` in `arc-suite`. Record every asset's origin and license.
9. **Rust only in desktop applications.** No web view, no JavaScript or TypeScript.
10. **Small, tested changes.** One concern per pull request, with the test that fails without it.
    Update the documents that describe the behavior you change in the same pull request.
11. **Report only what ran.** Say what you tested and what you could not run.
12. **Ask a human before** adding a license outside the allowlist, publishing or releasing,
    changing a file format or public protocol, adding a new repository or remote, spending money,
    handling secrets, or any destructive git operation. Otherwise keep working.
13. **Keep secrets and personal data out** of repositories, logs and requests.

## Repository invariants

- No command writes to a source model; exports always create new files
  ([ADR 0001](docs/adr/0001-review-data-separate-from-sources.md)).
- Review data references scene content only by item key (model source id plus source element id),
  never by cache index; unresolved references are reported, never deleted silently.
- Scene caches are derived and content-addressed; deleting one must never lose data
  ([ADR 0003](docs/adr/0003-content-addressed-scene-cache.md)).
- `review-model`, `review-scene`, `review-io` and the read-only part of `review-commands` build for
  `wasm32-unknown-unknown` and never depend on winit, egui, wgpu or OpenXR
  ([ADR 0002](docs/adr/0002-one-engine-three-delivery-targets.md)).
- The read-only viewer and browser registries contain no mutating command; a test asserts it.
- `review-analysis` is not linked by the viewer targets.
- A clash result is identified by its test plus the unordered item key pair; re-runs update, never
  recreate ([ADR 0004](docs/adr/0004-clash-identity-and-open-result-format.md)).
- Issues are BCF topics; extensions go through BCF's extension mechanism
  ([ADR 0005](docs/adr/0005-bcf-native-issue-model.md)).
- Background results carry the revision they were computed from; stale results are never applied.

## Documentation rules

- Public files never mention internal planning, research or the process that produced them.
- Other companies' names appear only as formats and standards, in factual interoperability
  statements, once in the positioning paragraph of `docs/product/vision.md`, and in the README
  trademark notice. Never describe a feature by naming another product's feature.
- Use Arc's own descriptive vocabulary (clash detection, 4D sequencing, quantity takeoff).
- Each fact once; link suite-wide rules in `arc-suite` instead of copying them.
- Clean room: original wording, no copied documentation structure, dialog text or option lists.
- Capability identifiers `RV-<AREA>-<NNN>` are permanent; status lives in issues, not tables.

## Issue workflow

Work starts from an issue: search issues and pull requests, confirm nobody is assigned, assign
yourself, set *In Progress*. Statuses: Backlog, Ready, In Progress, In Review, Blocked, Done (only
with met and verified acceptance criteria). On finishing, comment with what changed, the commit or
pull request link and open items; create follow-up issues after searching for duplicates. Labels:
`type:`, `tier:`, `phase:`, `area: <token>`. Conventional Commits with DCO sign-off.

## Verification

Run the narrowest relevant check first (one crate's tests, one capability file's link check) and
broaden only when it fails for a relevant reason or shared code changed. Report only what ran.

## Ask a human about

Changes to the `.arcrev` schema, the scene cache format or the JSON review-data schemas; new
readers that need a bridge or a license outside the allowlist; anything touching package encryption
or publishing restrictions; changes to the browser bundle's programming interface consumed by
Arc Hub; and every item listed in rule 12 above.
