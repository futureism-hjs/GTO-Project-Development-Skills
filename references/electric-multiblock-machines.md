# Electric Multiblock Machine Registration

Use this document only for `/电力多方块机器注册`. Read [Shared workflow](shared-workflow.md) for registration and validation stages, and [Evidence and validation map](verified-source-map.md) for the closest working definition.

## Definition Shape

The verified basic example is `OneStopRareEarthProcessingPlantRegistration`; active hyperdimensional machines and GTOCore GCYM machines provide additional references.

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

## Pattern Decisions

- Map the controller with `Predicates.controller(machine)`.
- Treat `setExactLimit`, `setMinGlobalLimited`, `setMaxGlobalLimited`, and `setPreviewCount` as distinct contracts.
- Use `Predicates.any()` only for deliberately ignored coordinates; it does not require air.
- Select `autoAbilities`, `autoAccelerateAbilities`, `autoGCYMAbilities`, or `autoLaserAbilities` from the native controller contract. They are not interchangeable.
- A part accepted by the pattern has no effect unless the controller or recipe modifier reads it.
- If a requested hatch changes the runtime formula or introduces a new consumer, stop and audit the ABI before adding an ability.

## Validation

Validate registry identity, recipe types, modifier, non-null renderer, preview flags, non-null pattern supplier, exact ability limits, and successful pattern construction. Verify client-only renderer behavior on the client and shared registration on a dedicated server without loading client classes.
