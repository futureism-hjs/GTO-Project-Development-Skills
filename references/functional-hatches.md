# Functional Hatch Registration

Use this document only for `/功能性仓室注册`. This command covers non-input/output hatches such as parallel, acceleration, thread, and overclock hatches. This document covers only the four established hatch families listed below; unverified hatch families are outside its scope. Read [Shared workflow](shared-workflow.md) for registration stages and [Evidence and validation map](verified-source-map.md) for native part implementations.

## Five-Layer Contract

A functional hatch is complete only when all five layers are connected:

1. a `MachineDefinition` registered in the owning GTO data-class window;
2. the correct `PartAbility` membership;
3. a part machine implementing state and behavior;
4. a multiblock pattern accepting the ability with explicit limits;
5. a controller or recipe modifier that reads the part and applies its effect.

Do not stop after `.abilities(...)`. GTOCore's verified examples are:

| Part | Ability | Part behavior | Consumer |
| --- | --- | --- | --- |
| Parallel hatch | `PartAbility.PARALLEL_HATCH` | parallel value by tier | parallel-capable controller/modifier |
| Acceleration hatch | `GTOPartAbility.ACCELERATE_HATCH` | duration percentage and tier penalty | workable electric controller |
| Thread hatch | `GTOPartAbility.THREAD_HATCH` | configured thread count | thread-aware controller/modifier |
| Overclock hatch | `GTOPartAbility.OVERCLOCK_HATCH` | configured overclock policy | overclock-aware controller/modifier |

Reuse the existing part class and owning registration helper when the requested behavior matches. Native parallel hatches use `MachineRegisterUtils.registerTieredGTMMachines`; other tiered parts may use `registerTieredMachines` or a capability-specific helper. Attach the exact ability, use the existing tiered hull/overlay renderer, and derive tooltip values from the same tier formula as runtime behavior.

At pattern level, use a deliberate limit such as `setMaxGlobalLimited(1)`. Validate definition identity during registration and ability membership only after Registrate binds candidate blocks.

## New Effects

If the request introduces a new formula, state consumer, or ability rather than reusing one of the verified contracts, stop and audit the ABI first. A pattern that matches an existing hatch does not prove that a new effect is implemented. Do not create a parallel functional-hatch framework.
