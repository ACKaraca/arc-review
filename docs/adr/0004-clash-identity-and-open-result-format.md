# 0004 Clash result identity across runs and an open result format

- Status: Accepted
- Scope: `review-analysis`, `review-model`, `review-io`; RV-CLASH, RV-CHECK, RV-ISSUE

## Context

Coordination is iterative: authors fix models, sources are refreshed and tests are run again. The
value of clash detection depends on recognizing the same problem across runs, so that status,
assignment, comments and linked issues stay with it, and on recognizing that a problem disappeared.
When identity is tied to result order or to unstable source identifiers, every run looks like a new
set of thousands of results, and teams fall back to spreadsheets. Results are also consumed outside
the application (dashboards, issue trackers, contract records), which requires a documented format.

## Decision

1. A clash result's identity is the unordered pair of item keys plus the clash test identifier.
   When the same pair is found in a later run, the existing result is updated (distance, contact
   point, last found run) and keeps its status, assignee, comments and issue links.
2. When an item key changed because the source re-keyed its elements, the key mapping of the
   refresh (RV-TREE-014) is applied before matching; unmatched pairs are matched a second time by a
   geometric signature (contact point within tolerance, item types and bounding boxes) and the match
   is marked as inferred.
3. A result not found in the latest run is not deleted; it receives the computed state "not found",
   and the status transition applied to it is defined by the test (RV-CLASH-031).
4. Every run is recorded with the revision of each model source, rules and settings, so results are
   traceable to the inputs that produced them. The same identity and run rules apply to check
   findings (RV-CHECK-027).
5. Clash tests, runs, results and groups are exchanged in a documented, versioned JSON format whose
   schema is published in this repository; BCF is used when results become issues. The format
   carries source element identifiers alongside item keys.

## Consequences

- Status and history survive any number of runs; dashboards show trends per run.
- Clash quality depends on item key quality per source, which Arc Review reports.
- Inferred matches can be wrong; they are flagged so a reviewer can split them.
- The JSON schema is a public contract with versioning and migration obligations.

## Alternatives considered

- **Identity by result name or order.** Rejected: unstable across runs.
- **Identity by location only.** Rejected: moving an element would break identity; kept only as the
  secondary inferred match.
- **Only proprietary report formats (HTML, spreadsheet).** Rejected: not machine-readable enough for
  integration; HTML and spreadsheet reports remain as presentation outputs.

## Reopen when

A published open standard for clash result exchange gains adoption; Arc Review then writes it
natively and keeps its own schema only for what the standard cannot carry.
