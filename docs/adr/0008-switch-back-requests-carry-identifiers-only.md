# 0008 Switch-back requests carry identifiers only

- Status: Accepted
- Scope: [application connectors](../product/capabilities/27-application-connectors.md),
  RV-AUTO-010; see [interoperability](../architecture/interoperability.md)

## Context

Connectors ([suite ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md))
let a reviewer jump from an element, clash result or issue back to the authoring application. The
channel connects a review application to another vendor's application on the same machine, and
Arc Review must not become a path by which review data, geometry or edits reach it.

## Decision

1. A switch-back request carries source element identifiers and, for a view, camera, section and
   hidden element identifiers. It carries no geometry, no review data and no property values.
2. The only effects a connector produces from a request are selection and a temporary view in the
   host application. A request never creates, changes or deletes a model element.
3. The endpoint is local to the machine, bound to the loopback interface, and requires a
   per-session token that the connector shows to the author on first use.
4. Arc Review does not start a host application without confirmation, and works without any
   connector through identifier lists and BCF.

## Consequences

- Source ownership ([ADR 0001](0001-review-data-separate-from-sources.md)) holds across the
  connector boundary.
- A connector is small and auditable: identifiers in, selection and view out.
- Features that need more, such as writing review results into the host model, are not possible
  through this channel and need a new decision.

## Alternatives considered

- **Send review data to the connector so it can show markups in the host.** Rejected: widens the
  channel and puts review data in another vendor's application.
- **Drive the host through its project file.** Rejected: unsupported without a published
  specification and a violation of ADR 0001.

## Reopen when

A host application's public API cannot express selection and temporary views, or authors need review
results shown inside the host.
