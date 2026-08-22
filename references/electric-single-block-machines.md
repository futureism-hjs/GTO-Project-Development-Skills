# Electric Single-Block Machine Registration

Use this document only for `/电力单方块机器注册`. Read [Shared workflow](shared-workflow.md) for native registration windows and [Evidence and validation map](verified-source-map.md) for verified examples.

## Standard Registration

Prefer `MachineRegisterUtils.registerSimpleMachines(...)` when the standard `SimpleTieredMachine` contract is sufficient. It supplies `GTORecipeModifiers.UPGRADE_OVERCLOCK`, editable UI, rotation, recipe type, tiered rendering, and standard capacity/power tooltips.

Use `registerTieredMachines(...)` only when the factory or builder differs per tier. GTOCore's verified specialized example is `GTOMachines.VACUUM_PUMP`:

- factory: `VacuumPumpMachine::new`;
- tiers: LV, MV, HV;
- editable UI and `VACUUM_PUMP_RECIPES`;
- `workableTieredHullRenderer`;
- vacuum-tier and machine-function tooltips.

## Tier Contract

Keep the definition's tier, voltage, tank scaling, recipe modifier, recipe type, editable UI, and renderer consistent. A loop that changes only the registry name is not a valid tiered registration. Do not create a second helper for a behavior already covered by `registerSimpleMachines` or the native specialized path.

## Validation

Validate every final ID, tier, recipe type, modifier, renderer, UI factory, and capacity formula. Check client renderer state only on the client distribution; a dedicated server must retain the expected server-side renderer placeholder and must not load client classes. If a requested machine needs a new runtime consumer, stop at the contract boundary and audit the consumer before changing the registration path.
