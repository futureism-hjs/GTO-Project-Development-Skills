# Hatches and Maintenance

## Functional Hatches

A functional hatch requires five connected layers:

1. a `MachineDefinition` registered in the owning GTO data-class window;
2. the correct `PartAbility` membership;
3. a part machine implementing the state and behavior;
4. a multiblock pattern accepting the ability with explicit limits;
5. a controller or recipe modifier that reads the part and applies its effect.

Do not stop after `.abilities(...)`. GTOCore's verified examples are:

| Part | Ability | Part behavior | Consumer |
| --- | --- | --- | --- |
| Parallel hatch | `PartAbility.PARALLEL_HATCH` | parallel value by tier | parallel-capable controller/modifier |
| Acceleration hatch | `GTOPartAbility.ACCELERATE_HATCH` | duration percentage and tier penalty | workable electric controller |
| Thread hatch | `GTOPartAbility.THREAD_HATCH` | configured thread count | thread-aware controller/modifier |
| Overclock hatch | `GTOPartAbility.OVERCLOCK_HATCH` | configured overclock policy | overclock-aware controller/modifier |

Reuse the existing part class and registration helper when the requested behavior matches. Register tier arrays with `registerTieredMachines`, attach the exact ability, use the existing tiered hull/overlay renderer, and keep tooltip values derived from the same tier formula as runtime behavior.

At pattern level, use a deliberate limit such as `setMaxGlobalLimited(1)`. Validate definition identity during registration and ability membership only after Registrate binds candidate blocks.

## Input and Output Parts

Distinguish ability labels from actual storage and recipe handlers.

- `IMPORT_ITEMS`, `EXPORT_ITEMS`, `IMPORT_FLUIDS`, and `EXPORT_FLUIDS` describe controller-facing capability.
- `DUAL_INPUT` and `DUAL_OUTPUT` are GTO combined-part abilities.
- GTO's `ITEMS_INPUT_BUS`, `ITEMS_OUTPUT_BUS`, and advanced steam fluid abilities are collections populated with concrete blocks.
- The part must create correctly directed `NotifiableContentHandler` or tank instances and expose them through the controller contract.

The stable infinite-intake implementation deliberately constructs its recipe tank as `IO.IN` while exposing an external `IO.BOTH` fluid capability. It can export generated gas without matching a recipe-output-hatch predicate, and it overrides `swapIO()` to remain an input part.

The stable ME input assemblies declare item import, fluid import, and dual input, then create real item and fluid handlers backed by one AE node. Three ability names alone would not provide inventory.

When one part has input and output behavior, keep opposite directions in separate `RecipeHandlerUnit` instances. GTCEu requires handlers within a unit to use one direction.

## Maintenance Integration

Use the native maintenance contract instead of creating a parallel fault system.

- Choose a maintenance-aware controller class.
- Add `Predicates.abilities(PartAbility.MAINTENANCE)` to an appropriate casing position.
- Use `setExactLimit(1)` when the machine requires exactly one maintenance hatch; use optional limits only when the product contract explicitly says maintenance is optional.
- Keep tooltip claims consistent with the pattern and controller.
- Verify that the attached part implements the maintenance interface and is collected by the controller.
- Preserve original maintenance problems, recovery tools, perfect-maintenance behavior, and recipe penalties unless the requested feature explicitly changes them.

The active one-stop rare-earth plant and many native electric/GCYM machines are verified exact-one patterns. Steam multiblocks should not gain maintenance by default; first select a controller that supports it.


