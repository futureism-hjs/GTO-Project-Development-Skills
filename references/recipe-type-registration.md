# Recipe Type Registration

Use this document only for `/配方类型注册`. Read [Shared workflow](shared-workflow.md) for the native static-initialization window and [Evidence and validation map](verified-source-map.md) for existing recipe types.

## Native Definition

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

For a vanilla or other-mod recipe proxy, override both conversion and representative-category construction so custom fields are preserved. GTOCore 0.5.6 caches the recipe manager after initial load; require a full restart for proxy additions or changes.
