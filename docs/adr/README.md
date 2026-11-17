# Architecture decision records

Decisions specific to Arc Review. Suite-wide decisions (repository topology, UI toolkit, document
model, document package, command model, interoperability policy, rendering contract, collaboration
stages, extensibility, licensing, robustness, connectors and licensed bridges, web and mobile targets) are recorded once in
[`arc-suite/docs/adr`](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr) and are not
restated here.

## Index

| ADR | Title | Status |
|---|---|---|
| [0001](0001-review-data-separate-from-sources.md) | Review data is stored apart from source models, which are never edited | Accepted |
| [0002](0002-one-engine-three-delivery-targets.md) | One engine, three delivery targets | Accepted |
| [0003](0003-content-addressed-scene-cache.md) | Content-addressed scene cache per model source | Accepted |
| [0004](0004-clash-identity-and-open-result-format.md) | Clash result identity across runs and an open result format | Accepted |
| [0005](0005-bcf-native-issue-model.md) | Issues use the BCF model natively | Accepted |
| [0006](0006-review-sessions-synchronize-state-not-geometry.md) | Live review sessions synchronize review state, not geometry | Accepted |
| [0007](0007-mobile-is-a-fourth-target-of-the-same-engine.md) | Mobile is a fourth target of the same engine | Accepted |
| [0008](0008-switch-back-requests-carry-identifiers-only.md) | Switch-back requests carry identifiers only | Accepted |

## Template

```markdown
# NNNN Title in the imperative or as a statement of the decision

- Status: Proposed | Accepted | Superseded by NNNN
- Scope: crates, capabilities or documents affected

## Context

The forces at play: requirements, constraints, what makes the decision necessary now.

## Decision

What is decided, stated so that a reviewer can check an implementation against it.

## Consequences

What becomes easier, what becomes harder, what must now be built or maintained.

## Alternatives considered

Each rejected option with the reason it was rejected.

## Reopen when

The observable condition that would make this decision worth revisiting.
```

Numbers are never reused. A superseded record stays in place with its status updated and a link to
the record that replaces it.
