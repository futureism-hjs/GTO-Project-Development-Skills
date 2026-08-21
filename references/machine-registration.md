# Machine Registration

## Steam Single-Block Machines

Use the native low-pressure/high-pressure pair contract. GTOCore's verified example is `STEAM_VACUUM_PUMP`; GTCEu's examples include the steam macerator and the `registerSimpleSteamMachines` family.

```java
Pair<MachineDefinition, MachineDefinition> definitions =
        MachineRegisterUtils.registerSteamMachines(
                "steam_example",
                "Example",
                SimpleSteamMachine::new,
                (highPressure, builder) -> builder
                        .nonYAxisRotation()
                        .recipeType(recipeType)
                        .recipeModifier(SimpleSteamMachine::recipeModifier)
                        .renderer(() -> new WorkableSteamMachineRenderer(
                                highPressure, overlay))
                        .register());
```

Preserve the pressure boolean in the machine factory, renderer, throughput, tank, tooltip, and recipe modifier. Do not model the pair as ordinary electric tiers. Use a specialized machine class when the recipe condition needs additional state, as the native steam vacuum pump does.

Validate both definitions and both final IDs.

## Electric Single-Block Machines

Prefer `MachineRegisterUtils.registerSimpleMachines(...)` when the standard `SimpleTieredMachine` contract is sufficient. It already supplies `GTORecipeModifiers.UPGRADE_OVERCLOCK`, editable UI, rotation, recipe type, tiered renderer, and standard capacity/power tooltips.

Use `registerTieredMachines(...)` only when the factory or builder differs per tier. GTOCore's `VACUUM_PUMP` is the verified specialized example:

- factory: `VacuumPumpMachine::new`;
- tiers: LV, MV, HV;
- editable UI and `VACUUM_PUMP_RECIPES`;
- `workableTieredHullRenderer`;
- vacuum-tier and machine-function tooltips.

Keep the definition's tier, voltage, tank scaling, recipe modifier, recipe type, editable UI, and renderer consistent. A loop that changes only the registry name is not a valid tiered registration.

## Steam Multiblock Machines

Choose the controller first:

- `SteamMultiblockMachine` for low-level steam structures;
- `LargeSteamMultiblockMachine` for large steam structures and advanced steam I/O;
- a specialized steam controller only when its runtime behavior is required.

The active verified example is `UniversalSteamFactoryRegistration`. Its contract includes a native large-steam controller, explicit steam hatch and item/fluid I/O predicates, native recipe modes, a steam overclock declaration, a casing/overlay renderer, and load-complete pattern validation.

Low-level patterns normally accept exactly one `STEAM` hatch, the matching steam item buses, and any required vent hatch. Large-steam patterns may accept GTO's advanced steam item/fluid abilities. Do not add energy, maintenance, parallel, acceleration, thread, or overclock parts unless the chosen native controller and product contract actually consume them.

`steamOverclock(tier)` is builder metadata, not a guaranteed hard input-power ceiling. If a machine must reject recipes above a fixed EUt regardless of steam hatch multiplier, enforce that rule in the existing recipe-conversion/runtime path and scope it by definition ID.

## Electric Multiblock Machines

The verified basic example is `OneStopRareEarthProcessingPlantRegistration`; additional working examples include the active hyperdimensional machines and GTOCore's GCYM machines.

Keep these pieces aligned:

```java
definition = MachineRegisterUtils.multiblock(path, localizedName, controllerFactory)
        .recipeTypes(recipeType)
        .recipeModifier(modifier)
        .block(primaryCasing)
        .multiblockPreviewRenderer(true, true)
        .pattern(machine -> FactoryBlockPattern.start(machine)
                .where('S', Predicates.controller(machine))
                .where('X', Predicates.blocks(primaryCasing.get())
                        .or(requiredAbilityPredicates))
                .build())
        .workableCasingRenderer(casingTexture, frontOverlay)
        .register();
```

Pattern rules:

- Map the controller with `Predicates.controller(machine)`.
- Treat `setExactLimit`, `setMinGlobalLimited`, `setMaxGlobalLimited`, and `setPreviewCount` as distinct contracts.
- Use `Predicates.any()` only for deliberately ignored coordinates; it does not require air.
- Select `autoAbilities`, `autoAccelerateAbilities`, `autoGCYMAbilities`, or `autoLaserAbilities` from the native controller contract. They are not interchangeable.
- A part accepted by the pattern has no effect unless the controller or recipe modifier reads it.

Validate registry identity, recipe types, non-null renderer, preview flags, non-null pattern suppliers, and successful pattern construction.

