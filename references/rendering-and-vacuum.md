# Rendering and Vacuum

## Layered Rendering

Use the narrowest existing renderer that expresses the required layers.

### Tiered Parts and Single Blocks

```java
.renderer(() -> new OverlayTieredMachineRenderer(tier, frontOverlay))
```

This composes the tiered machine hull with the supplied surface overlay. It is used by the verified ME assemblies and infinite intake hatches. `workableTieredHullRenderer(...)` is appropriate when the overlay also follows working state. GTOCore's parallel, acceleration, thread, and overclock hatch registrations are the authoritative part examples.

### Multiblock Controllers

```java
.workableCasingRenderer(casingTexture, frontOverlay)
```

The first resource is the casing layer and the second is the controller face/working overlay. Use the overload with an explicit emissive flag only when the selected native renderer requires it. The active rare-earth plant, universal steam factory, and native GTO/GCYM machines use this composition.

### Custom Renderers

Create a custom renderer only when the established renderer cannot represent required per-side, per-state, or dynamic behavior. Keep it client-only, reuse existing resource IDs when licensing and runtime ownership permit, and verify dedicated-server class loading.

For every renderer:

- verify the casing tier and overlay ID independently;
- check inactive, active, emissive, and orientation states that actually exist;
- ensure co-located legacy textures are not loaded accidentally;
- validate a non-null renderer and client resource bake;
- do not copy or modify dependency resources when referencing their IDs is sufficient.

## Vacuum Level Integration

The verified extension is a passive cover with vacuum tier 3:

1. register a `CoverDefinition` before `GTOCovers.<clinit>()V` returns;
2. constrain `canAttach()` to ordinary single-block machines and maintenance parts;
3. reject unrelated multiblock controllers and parts;
4. supplement `VacuumCondition.testCondition` only for supported tiers and hosts;
5. return to the original GTO condition logic when the extension does not satisfy the request;
6. mark recipe logic dirty and refresh tick subscription after attach/remove;
7. validate cover registry identity at load-complete.

Never replace the full native condition with a custom approximation. In the verified implementation, required tiers 1-3 may be satisfied by the cover, tier 4 is never satisfied by it, and a multiblock controller searches only its maintenance parts for the cover.

When creating a vacuum machine rather than a cover, reuse GTOCore's steam/electric vacuum pump pattern: the specialized machine implements the vacuum interface, its definition advertises the supported tier, and recipes use the native vacuum condition.

