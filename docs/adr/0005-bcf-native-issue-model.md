# 0005 Issues use the BCF model natively

- Status: Accepted
- Scope: `review-model`, `review-io`; RV-ISSUE, RV-HUB, RV-XR, RV-WEB

## Context

Issues travel between Arc Review, Arc Hub, authoring applications and other teams' tools. The BIM
Collaboration Format (BCF) is the open standard for this exchange, both as files and as a REST API.
If Arc Review kept its own issue model and converted to BCF on export, every round trip would risk
loss and every field added internally would diverge from what other tools understand.

## Decision

1. The issue object in the review document is a BCF topic: identifiers, fields, comments,
   viewpoints (camera, components, visibility, coloring, clipping planes, snapshot), related topics
   and document references follow the BCF 3.0 data model. Arc Review additions (links to clash
   results and check findings, markup geometry, sheet placement) are stored as extension data that
   is written to BCF in its documented extension mechanism and ignored by readers that do not know
   it.
2. BCF 2.1 is supported for reading and writing; content that 2.1 cannot carry is listed in the
   loss report.
3. Viewpoints of issues reference components by IFC GlobalId when the source provides one, and by
   source element identifier otherwise, so authoring tools can find the elements.
4. Arc Hub exposes the BCF API; Arc Review synchronizes issues through it. Offline changes are
   queued and merged on reconnection; a conflicting field keeps both values in the issue history and
   is shown for resolution.
5. Issues created from clash results, check findings, sheet markups or immersive sessions are
   ordinary BCF topics with references back to their origin.

## Consequences

- Issues round-trip with any BCF-capable tool without conversion layers.
- Arc Review's issue workflow (fields, statuses, labels) is bounded by what BCF expresses; project
  extensions (allowed values) come from BCF project settings.
- The BCF schema version becomes a compatibility dependency that is tracked.

## Alternatives considered

- **Own issue model with BCF import and export.** Rejected: lossy round trips and two models to
  maintain.
- **Hub-specific issue API only.** Rejected: excludes non-Arc tools and violates the open-formats
  principle.

## Reopen when

A successor standard to BCF replaces it in practice, or BCF cannot represent a required workflow
even through extensions.
