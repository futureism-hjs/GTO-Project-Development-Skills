# Input and Output Part Registration

Use this document only for `/输入输出类仓室注册`. Read [Shared workflow](shared-workflow.md) for registration windows and [Evidence and validation map](verified-source-map.md) for stable input-part examples.

## Ability Versus Storage

Distinguish ability labels from actual storage and recipe handlers:

- `IMPORT_ITEMS`, `EXPORT_ITEMS`, `IMPORT_FLUIDS`, and `EXPORT_FLUIDS` describe controller-facing capability.
- `DUAL_INPUT` and `DUAL_OUTPUT` are GTO combined-part abilities.
- GTO's `ITEMS_INPUT_BUS`, `ITEMS_OUTPUT_BUS`, and advanced steam fluid abilities are collections populated with concrete blocks.
- The part must create correctly directed `NotifiableContentHandler` or tank instances and expose them through the controller contract.

When one part has input and output behavior, keep opposite directions in separate `RecipeHandlerUnit` instances; GTCEu requires handlers within one unit to use one direction.

## Verified Patterns

The stable infinite-intake implementation constructs its recipe tank as `IO.IN` while exposing an external `IO.BOTH` fluid capability. It can export generated gas without matching a recipe-output-hatch predicate and overrides `swapIO()` to remain an input part.

The stable ME input assemblies declare item import, fluid import, and dual input, then create real item and fluid handlers backed by one AE node. Three ability names alone do not provide inventory.

Reuse the existing registration owner, machine class, handler direction, renderer, and synchronization path. If a requested part needs a new consumer or storage protocol, stop at the ABI boundary and audit it before editing the pattern.

## Validation

Verify definition identity, ability membership after Registrate binding, handler direction, inventory/tank creation, controller collection, recipe I/O, persistence, and client/server loading. Pattern acceptance without a live handler is an incomplete implementation.
