# Layered Material Rendering

Use this document only for `/组合类材质渲染`. This covers materials composed of multiple layers, such as a casing layer plus a surface/overlay layer on multiblock controllers, parallel hatches, acceleration hatches, thread hatches, and overclock hatches. Read [Shared workflow](shared-workflow.md) for distribution and lifecycle checks and [Evidence and validation map](verified-source-map.md) for active renderer examples.

## Tiered Parts and Single Blocks

```java
.renderer(() -> new OverlayTieredMachineRenderer(tier, frontOverlay))
```

This composes the tiered machine hull with the supplied surface overlay. It is used by the verified ME assemblies and infinite-intake hatches. Use `workableTieredHullRenderer(...)` when the overlay also follows working state. GTOCore's parallel, acceleration, thread, and overclock hatch registrations are authoritative part examples.

## Multiblock Controllers

```java
.workableCasingRenderer(casingTexture, frontOverlay)
```

The first resource is the casing layer and the second is the controller face/working overlay. Use an overload with an explicit emissive flag only when the selected native renderer requires it. The active rare-earth plant, universal steam factory, and native GTO/GCYM machines use this composition.

## Custom Layered Renderers

Create a custom renderer only when the established renderer cannot represent required per-side, per-state, animated, or dynamic behavior. Keep it client-only, reuse existing resource IDs when licensing and runtime ownership permit, and verify dedicated-server class loading.

For a layered part with custom animated or stateful casing behavior, inspect the active `MESuperPatternBufferRegistration` and `AmprosiumPatternBufferRenderer` path first. Reuse that renderer or its narrowest established extension when its layer contract matches; create a new renderer only when the required behavior still cannot be expressed.

For every renderer, verify casing tier and overlay ID independently, inspect inactive/active/emissive/orientation states that actually exist, prevent accidental loading of co-located legacy textures, validate a non-null renderer and client resource bake, and do not copy dependency resources when referencing their IDs is sufficient.
