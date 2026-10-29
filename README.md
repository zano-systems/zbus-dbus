# zbus-dbus-bridge

## Mission

Enable applications running on Zephyr and Linux in an asymmetric multiprocessing
(AMP) system to exchange messages through Zephyr zbus and Linux D-Bus, using
protobuf as the shared message contract.

The project will reuse Zephyr's zbus multidomain proxy agent and provide a
corresponding Linux proxy agent with an independent D-Bus adapter. Application
payloads and channel bindings will be defined separately, with one publishing
side per channel and generated code preserving origin and shadow-channel
ownership.

RPMsg will be the production transport, with transport-specific behavior kept
separate from channel semantics. Development will progress from contract
definition and feasibility validation to code generation, bridge implementation,
D-Bus integration, and deployment on selected i.MX8M or i.MX93 hardware.

The planned Zephyr implementation uses nanopb and custom code generation. Rust,
prost, and the `zbus` D-Bus library are proposed for Linux. The Linux stack,
D-Bus payload representation, delivery guarantees, and initial emulation and
hardware targets remain subject to confirmation.

Validation will cover generated contracts, cross-platform serialization, channel
ownership, connection recovery, D-Bus behavior, and bidirectional operation over
RPMsg.

Project-specific identifiers will use the `zdb` prefix.
