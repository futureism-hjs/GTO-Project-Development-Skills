# Recipe Registration

Use this document only for `/配方注册`. Read [Shared workflow](shared-workflow.md) when injecting into `Data.commonInit()`, and consult [Evidence and validation map](verified-source-map.md) for the active recipe implementations.

## Native Lifecycle

The verified GTOLib 26.7.4 lifecycle is:

```text
recipe type registered
  -> Data.commonInit()
  -> the single RecipeFilter.init() call
  -> inline RecipeType.recipeBuilder(rawId)
  -> inputs, outputs, conditions, EUt, duration
  -> RecipeBuilder.save()
  -> GTO finalization
  -> final-table validation
```

Keep builder/save bytecode inline in the native window. Calling a Java wrapper that performs `save()` outside that context has produced `gtceu:default` dummy recipes.

## Construction Rules

- Duration is in ticks; 20 ticks equal one second.
- EUt is a Java `long`; ASM descriptors use `(J)`.
- Fluids are in mB.
- Use `ChemicalHelper` with the correct `TagPrefix` and material registry for material items.
- Stay within the recipe type's maximum I/O sizes.
- Compute final IDs with `RecipeBuilder.getTypeID(rawId, recipeType)`.
- For crafting-table recipes, use `VanillaRecipeHelper` in the same native window. `ShapedRecipeBuilder` resolves raw `namespace:path` to `namespace:shaped/path`.

The receiver must reject null, dummy, wrong-type, wrong-I/O, wrong-EUt, and wrong-duration results. After finalization, verify that the global builder table and recipe-type table retain the same definition object. Never patch EMI to compensate for an invalid recipe.
