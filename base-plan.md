# zbus-dbus-bridge — Base Plan

## Objective

Develop a bridge that enables applications running on Zephyr and Linux in an
asymmetric multiprocessing (AMP) system to exchange messages through Zephyr zbus
and Linux D-Bus.

Protobuf shall provide the shared message contract from which the required
integration code is generated.

This document records the planning baseline. Proposed technologies and outstanding
decisions shall be confirmed before the affected implementation stages.

## Architecture

The bridge shall consist of three principal components:

1. **Zephyr integration:** reuse the existing zbus multidomain proxy agent to
   forward origin-channel messages and receive messages through shadow channels.
2. **Linux proxy agent:** implement corresponding channel ownership, message
   exchange, and local publication/subscription functionality.
3. **D-Bus adapter:** expose the Linux proxy agent to applications through D-Bus,
   independently of the interprocessor transport.

RPMsg shall be the production transport. Transport-specific behavior shall remain
separate from channel semantics.

## Message Contract

Application payloads shall be defined in protobuf files. Separate channel-binding
files shall identify each channel, its payload type, and its publishing side.

Each channel shall have one origin and a corresponding shadow on the receiving
side. Generated code shall preserve the ownership rules provided by the Zephyr
proxy agent.

Code generation shall produce compatible message bindings and channel metadata
for both platforms, including validation of message bounds and contract
consistency.

## Technology Direction

- **Zephyr:** nanopb with custom channel-binding generation.
- **Linux:** Rust with prost and the `zbus` D-Bus library is the proposed stack,
  pending confirmation.
- **Repository:** a monorepo using west for Zephyr and Cargo if Rust is retained.
- **Project name:** `zbus-dbus-bridge`.
- **Project identifiers:** `zdb`.

## Implementation Stages

1. **Contract definition:** establish the channel model, D-Bus representation,
   delivery guarantees, and compatibility policy.
2. **Feasibility validation:** verify the Zephyr proxy-agent extension and identify
   a working emulation environment.
3. **Core implementation:** develop code generation, the Zephyr backend, and the
   Linux proxy agent.
4. **D-Bus integration:** implement and validate the application-facing interface.
5. **System integration:** validate bidirectional exchange over RPMsg.
6. **Deployment:** support the selected i.MX8M or i.MX93 target and provide build,
   configuration, and usage documentation.

## Validation

Validation shall cover generated contracts, cross-platform serialization, channel
ownership, connection recovery, D-Bus behavior, and bidirectional operation over
RPMsg.

Emulated or test transports may support development, but shall not replace
validation of the production transport.

## Outstanding Decisions

- Native typed D-Bus interfaces or protobuf byte-array payloads.
- Final Linux language and library selection.
- Best-effort forwarding or acknowledged remote acceptance.
- Emulation platform, initial hardware target, and Linux build environment.
