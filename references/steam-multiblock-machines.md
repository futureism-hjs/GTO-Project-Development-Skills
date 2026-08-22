# Steam Multiblock Machine Registration

Use this document only for `/蒸汽类多方块机器注册`. Read [Shared workflow](shared-workflow.md) for registration-window and lifecycle rules, and [Evidence and validation map](verified-source-map.md) for the active steam examples.

## Controller Selection

Choose the controller before writing the pattern:

- `SteamMultiblockMachine` for low-level steam structures;
- `LargeSteamMultiblockMachine` for large structures and advanced steam I/O;
- a specialized steam controller only when its runtime behavior is required.

The active verified example is `UniversalSteamFactoryRegistration`. Its contract includes a native large-steam controller, explicit steam-hatch and item/fluid-I/O predicates, native recipe modes, a steam overclock declaration, a casing/overlay renderer, and load-complete pattern validation.

## Pattern and Runtime Rules

Low-level patterns normally accept exactly one `STEAM` hatch, matching steam item buses, and any required vent hatch. Large-steam patterns may accept GTO's advanced steam item/fluid abilities. Do not add energy, maintenance, parallel, acceleration, thread, or overclock parts unless the selected controller and product contract actually consume them.

`steamOverclock(tier)` is builder metadata, not a guaranteed hard input-power ceiling. If recipes must be rejected above a fixed EUt regardless of steam-hatch multipliers, enforce that rule in the existing recipe-conversion/runtime path and scope it by definition ID.

## Validation

Validate the controller class, steam and I/O predicates, exact ability limits, recipe modes, overclock metadata, casing/overlay renderer, and successful pattern construction. Verify the native static-initialization anchor before injecting registration. Do not treat a pattern match as proof that an ability has runtime effect; the controller or recipe modifier must read it.
