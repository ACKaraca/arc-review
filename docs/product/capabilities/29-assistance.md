# Assistance

This area specifies what the optional assistant does in Arc Review: answering questions about a
federated model, turning requests into searches, summarizing issues and markups, turning meeting
notes into issue drafts, grouping clashes by root cause, drafting clash tests and checking rules,
explaining findings and version changes, and preparing takeoff, sequencing and coordination reports,
always as previewed commands checked by the product's clash, check, comparison and takeoff engines.
The suite rules for levels, grounding, data handling, provenance and evaluation are in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md); the
panel is [RV-AUTO-021](25-automation.md) and the protocol endpoints are
[RV-AUTO-005 and RV-AUTO-006](25-automation.md). Every capability here also has a manual and
scripted path.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Assistance request | Text, level, scope (models, sets, clash tests, issues), base revision of the federation | Produces answers or proposals |
| Proposal | Items, command invocations, change summary, checker findings, citations | Applied as one undo step |
| Root-cause group | Name, explanation, member results, suggested responsible party, evidence | Proposed over clash results; becomes a manual group after acceptance |
| Issue draft | Title, description, assignee, due date, viewpoint, linked elements, source text | Becomes an issue only when the user creates it |
| Provenance mark | Origin, proposal, revision | On issues, groups, sets and rules created by an applied proposal until reviewed |

## Questions and search

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ASSIST-001 | Federation questions | The assistant answers questions about the federated model ("which models were refreshed this week", "which doors on level 3 lack a fire rating", "how many open clashes involve the sprinkler model") from model tree, property, clash and issue queries, citing elements, results and issues that open on request. | T2 | 2 |
| RV-ASSIST-002 | Search by intent | A request such as "all ducts above the corridor ceilings on levels 2 to 4" becomes a query in the query builder (RV-SET-012 to RV-SET-016) shown with its conditions and result count; the user runs, edits or saves it as a search set (RV-SET-020). | T2 | 2 |

## Issues and markups

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ASSIST-003 | Issue summaries | For a selection of issues or one issue thread, the assistant summarizes status, open questions, decisions and next steps as a draft comment or a short report, citing each issue and comment. | T3 | 2 |
| RV-ASSIST-004 | Meeting notes to issues | From pasted meeting notes or an attached minutes document, the assistant proposes issue drafts with title, description, assignee, due date and, where the notes name them, linked elements and viewpoints, using the project's issue templates (RV-ISSUE-008); each draft cites its source line and is created only by the user. | T3 | 2 |
| RV-ASSIST-005 | Markup consolidation | After markups from several reviewers are merged (RV-MARK-013), the assistant finds duplicate and conflicting comments, proposes merging duplicates and lists conflicts for a decision, citing the markups. | T3 | 2 |

## Coordination

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ASSIST-006 | Clash test setup | From a request such as "ducts against structure with 50 mm clearance on levels 2 to 4", the assistant proposes clash tests with selection sets, tolerance and custom rules (RV-CLASH-017), shown as definitions the user runs. | T3 | 3 |
| RV-ASSIST-007 | Root-cause grouping | Starting from automatic, element-centric and spatial grouping (RV-CLASH-037 to RV-CLASH-039), the assistant proposes groups by likely cause (one run of ducts against many beams, a model with a level offset, an outdated model, a type that never fits), names and explains each group with its evidence, and proposes a responsible party and priority (RV-CLASH-033); groups apply as manual groups and keep their members across re-runs (RV-CLASH-031). | T3 | 3 |
| RV-ASSIST-008 | Findings explained | For clash results and check findings (RV-CHECK-026), the assistant explains each group with the rule or IDS requirement, its source clause and the elements involved, and proposes promotion to issues (RV-CLASH-049, RV-CHECK-030); it never states that a model complies. | T3 | 3 |
| RV-ASSIST-009 | Rule and IDS drafting | From written information requirements or an attached requirements document, the assistant drafts rule sets (RV-CHECK-004 to RV-CHECK-009) or IDS specifications (RV-CHECK-018), each rule citing its clause; the draft runs against the federation and its findings are shown before the user saves it. | T3 | 3 |
| RV-ASSIST-010 | Version changes in words | After a version comparison (RV-COMP-001 to RV-COMP-004), the assistant describes the changes by model, level, discipline and kind, each statement citing the compared elements and their property or geometry differences. | T3 | 3 |
| RV-ASSIST-011 | Coordination report | The assistant drafts a coordination report from clash run summaries, check reports and issue statistics (RV-CLASH-023, RV-CHECK-031, RV-ISSUE-015) with trends since the previous run, every figure taken from those reports and every statement citing them. | T3 | 3 |

## Planning and quantities

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ASSIST-012 | Task attachment proposals | For an imported schedule, the assistant proposes attachment rules (RV-SEQ-011) from task names, zones and element properties; coverage validation (RV-SEQ-012) reports unattached elements and double attachments before the rules apply. | T3 | 3 |
| RV-ASSIST-013 | Takeoff mapping | The assistant proposes mappings from model properties to takeoff items and resources (RV-QTO-008), listing assumptions such as units and waste factors; quantities come from the takeoff engine, and a coverage check lists elements that no item takes off. | T3 | 3 |
| RV-ASSIST-014 | Sequence risk summary | After a sequencing simulation, the assistant summarizes tasks with clashes between temporary and permanent works, tasks without elements and out-of-order installations, citing tasks and elements. | T3 | 5 |

## Review

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RV-ASSIST-015 | Assisted content review | Issues, groups, sets and rules carrying a provenance mark show a distinct marker and filter; the user reviews and clears marks, and publishing issues to Arc Hub or BCF lists unreviewed items first. | T3 | 2 |

## Readiness

Readiness classes are defined in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md#readiness-classes).

| Class | ID | Checked by | Depends on |
|---|---|---|---|
| R1 | RV-ASSIST-001 | Model tree, property, clash and issue queries | RV-AUTO-005, RV-AUTO-021 |
| R1 | RV-ASSIST-002 | Query builder | RV-SET-012 to RV-SET-016, RV-SET-020 |
| R1 | RV-ASSIST-003 | Issue queries | RV-ISSUE-009, RV-ISSUE-010 |
| R1 | RV-ASSIST-004 | Issue templates and field validation | RV-ISSUE-001, RV-ISSUE-008 |
| R1 | RV-ASSIST-005 | Markup queries | RV-MARK-013 |
| R1 | RV-ASSIST-006 | Clash test validation | RV-CLASH-017 |
| R1 | RV-ASSIST-007 | Group membership, re-run status transitions | RV-CLASH-031, RV-CLASH-033, RV-CLASH-037 to RV-CLASH-039 |
| R2 | RV-ASSIST-008 | Rule sets, IDS validation | RV-CHECK-016, RV-CHECK-026, encoded rule data |
| R1 | RV-ASSIST-009 | Check run on the federation | RV-CHECK-001, RV-CHECK-018 |
| R1 | RV-ASSIST-010 | Version comparison | RV-COMP-001 to RV-COMP-004 |
| R1 | RV-ASSIST-011 | Report figures | RV-CLASH-023, RV-CHECK-031, RV-ISSUE-015 |
| R1 | RV-ASSIST-012 | Coverage validation | RV-SEQ-011, RV-SEQ-012 |
| R2 | RV-ASSIST-013 | Takeoff engine, coverage check | RV-QTO-007, RV-QTO-008, project item catalogs |
| R2 | RV-ASSIST-014 | Sequencing simulation, clash results per task | RV-SEQ-014, temporary works models |
| R1 | RV-ASSIST-015 | Provenance query | Provenance marks in `arc-core` |

## Evaluation

The assistance task suite is built from the [reference workflows](../workflows.md): workflow 1
(RV-ASSIST-001, RV-ASSIST-002), 3 (RV-ASSIST-006, RV-ASSIST-007, RV-ASSIST-011), 4 (RV-ASSIST-008,
RV-ASSIST-009), 5 (RV-ASSIST-003, RV-ASSIST-004), 7 (RV-ASSIST-012), 8 (RV-ASSIST-013), 9
(RV-ASSIST-010) and 10 (RV-ASSIST-005). Thresholds are settled under
[OQ-36](../open-questions.md).
