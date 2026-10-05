# Frame transaction methods

Methods for sidecar services, such as bundlers, that serve frame transactions (type `0x06`) through
an execution client.

The namespace has two surfaces with different trust levels:

| Method | Surface | Authentication | Changes state |
|---|---|---|---|
| `frame_simulateTransactionV1` | Public RPC | None | No |
| `frame_sendPrivilegedTransactionV1` | Authenticated endpoint | JWT | Queues a transaction for block building |
| `frame_capabilitiesV1` | Authenticated endpoint | JWT | No |

Authenticated methods follow the Engine API JWT scheme in
[authentication.md](../engine/authentication.md). They are served on a separate port from public RPC.
Clients MUST NOT expose them on public RPC endpoints.

## Versioning

Methods carry a `V1` suffix. A breaking change to a method's parameters or result requires a new
method name, for example `frame_simulateTransactionV2`, not an edit to V1. Callers discover the
supported versions with `frame_capabilitiesV1`.

## Reason codes

Errors in this namespace use the `FrameErrors` group in
[frame-errors.yaml](../error-groups/frame-errors.yaml). Callers MUST tolerate codes they do not
recognize, because new codes can be added without a version change.

## Trace schema

`frame_simulateTransactionV1` returns a `FrameSimulationTrace` when called with the `frameTrace`
tracer. The schema is defined in [frame.yaml](../schemas/frame.yaml). The trace contains observed
facts only. Rules that judge a trace are defined by consumers.
