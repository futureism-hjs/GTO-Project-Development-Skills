# Recipes and Recipe Types

## Recipe Type Registration

A new GTO recipe page is a `RecipeType`, not a hand-written EMI category. Register it before `GTORecipeTypes.<clinit>()V` returns.

```java
definition = RecipeTypeRegisterUtils.register(
                path,
                localizedName,
                GTRecipeTypes.MULTIBLOCK)
        .setEUIO(IO.IN)
        .setMaxIOSize(itemInputs, itemOutputs, fluidInputs, fluidOutputs)
        .setProgressBar(progressTexture, fillDirection)
        .setSound(sound);
```

Use the same object from the machine builder and every recipe builder. `setMaxIOSize` is ordered as item input, item output, fluid input, fluid output and constrains both UI and recipe content.

Do not force `GTORecipeTypes` initialization from the mod constructor. Validate definition identity and UI creation during the native window, then perform final checks at load-complete.

For a vanilla or other-mod recipe proxy, override both conversion and representative-category construction. Preserve custom fields that generic GT conversion cannot see. GTOCore 0.5.6 caches the recipe manager after initial load; require a full restart for proxy recipe additions or changes.

## Recipe Registration

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

Keep the builder/save bytecode inline in the native window. Calling a Java wrapper that performs `save()` outside that context has produced `gtceu:default` dummy recipes.

Rules:

- duration is in ticks; 20 ticks equal one second;
- EUt is a Java `long`; ASM descriptors use `(J)`;
- fluids are in mB;
- use `ChemicalHelper` with the correct `TagPrefix` and material registry for material items;
- stay within the recipe type's maximum I/O sizes;
- compute final IDs with `RecipeBuilder.getTypeID(rawId, recipeType)`.

The receiver must reject null, dummy, wrong-type, wrong-I/O, wrong-EUt, and wrong-duration results. After finalization, verify that the global builder table and the recipe type table retain the same definition.

Crafting-table recipes use `VanillaRecipeHelper` in the same native `Data.commonInit()` window. `ShapedRecipeBuilder` resolves raw `namespace:path` IDs to `namespace:shaped/path`; validate the resolved ID, crafting type, and output in the server's final `RecipeManager`.

Never patch EMI to compensate for a bad machine definition, recipe type, or final recipe table.


