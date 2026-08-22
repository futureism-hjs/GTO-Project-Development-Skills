# Steam Single-Block Machine Registration

Use this document only for `/蒸汽单方块机器注册`. Read [Shared workflow](shared-workflow.md) when the registration is injected into a native static-initialization window, and use [Evidence and validation map](verified-source-map.md) to select the closest verified machine.

## Native Contract

Steam single-block machines are a low-pressure/high-pressure pair, not ordinary electric tiers. The verified GTOCore example is `GTOMachines.STEAM_VACUUM_PUMP`; GTCEu also provides the steam macerator and the `registerSimpleSteamMachines` family.

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

Keep the pressure boolean only in the factory, renderer, throughput or recipe modifier, and tooltip paths that the selected machine class actually consumes. Do not infer pressure-specific tank-capacity scaling; follow the native tank contract unless a specialized controller explicitly changes it. Use a specialized machine class when the recipe condition needs additional state, as the native steam vacuum pump does.

## Registration and Validation

- Register both pair members in the owning native window and keep one idempotent registration state.
- Reject duplicate or ambiguous injection anchors instead of falling back to ordinary Forge setup.
- Validate both `MachineDefinition` objects and both final registry IDs before later lifecycle callbacks can observe them.
- At load-complete, verify the recipe type, renderer contract, pressure-specific behavior, and definition identity.

If the requested behavior needs a new steam consumer, tank rule, or pressure formula, stop and audit the specialized controller ABI before adding another implementation path.
