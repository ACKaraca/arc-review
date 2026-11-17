# 0006 Live review sessions synchronize review state, not geometry

- Status: Accepted
- Scope: `review-model`, `review-xr`, `review-ui`; RV-XR, RV-HUB

## Context

Multi-user review, in a headset or at a desk, needs every participant to see the same thing at the
same time: who is where, what is selected, what is hidden or sectioned, what is being pointed at,
which viewpoint is active. Federated models are gigabytes in size. Sending geometry through a
session would require a heavy relay server, make sessions slow to start and expose model content
to whoever runs the relay.

## Decision

1. Every participant loads the federation locally from the same model source versions (from Hub or
   a shared location) through the normal scene cache path. A session only starts when all
   participants have the same content keys or explicitly accept a mismatch (RV-XR-021).
2. The session exchanges small, typed messages: participant pose and avatar, pointer rays,
   selection, visibility state, section state, active viewpoint, measurements, markups in progress
   and newly created issues. Messages carry the session revision; a stale message is dropped.
3. Persistent results (issues, viewpoints, comments) are written through the normal commands into
   each participant's review document or synchronized through Hub; the session itself stores
   nothing durable except an optional session record (RV-XR-020).
4. A session is hosted either by Arc Hub (relay over an authenticated connection) or by a
   participant on the local network. The host holds the authoritative session state and its
   controls (RV-XR-017).
5. Voice is optional and travels outside the review state channel.

## Consequences

- Sessions start quickly and scale with participants, not with model size.
- No model geometry passes through the relay; access control for content stays with Hub
  permissions and source locations.
- Participants need the source versions locally or through Hub streaming before joining.
- The same session protocol serves desktop and headset participants.

## Alternatives considered

- **Stream geometry from the host.** Rejected: heavy, slow and exposes content to the relay.
- **Render on a server and stream video to headsets.** Rejected for the first releases: needs GPU
  servers and adds latency that harms comfort in headsets.
- **Shared editable document (live co-editing).** Rejected: review sessions need shared viewing
  state, not concurrent edits of the review document; element-level multi-user editing follows the
  suite's staged collaboration plan.

## Reopen when

Participants routinely join from devices that cannot load the federation locally, making
server-side rendering or geometry streaming necessary.
