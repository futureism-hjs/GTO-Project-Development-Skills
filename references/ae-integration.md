# AE Integration

Use the target runtime's customized AE2 API and GTO AE registration window. Do not assume upstream AE2 behavior is byte-for-byte identical.

## ME Multiblock Parts

Register GTO ME parts before `GTAEMachines.<clinit>()V` returns. The stable input assemblies use:

```java
.abilities(
    PartAbility.IMPORT_ITEMS,
    PartAbility.IMPORT_FLUIDS,
    GTOPartAbility.DUAL_INPUT)
.renderer(() -> new OverlayTieredMachineRenderer(
    tier, GTCEu.id("block/machine/part/me_pattern_buffer")))
```

Definition registration and ability binding occur at different times. Check registry identity in the static-init window, then check ability-table membership at load-complete.

An ME part must create real item/fluid handlers and bind them to the same node. For network-backed recipe extraction, use `Actionable.SIMULATE` for planning and `Actionable.MODULATE` for committed consumption. Keep automatic and manual stocking state distinct, reject duplicate keys across parts attached to one controller, and clear only generated automatic state when a network disconnects.

## Pattern Buffers

The stable super pattern buffers provide all six abilities:

```text
IMPORT_ITEMS, IMPORT_FLUIDS, EXPORT_ITEMS, EXPORT_FLUIDS, DUAL_INPUT, DUAL_OUTPUT
```

Keep every input slot in its own `IO.IN` unit and add a separate `IO.OUT` unit. Persist pending item/fluid outputs by exact AE key, including NBT, and `long` quantity before attempting network insertion. Retry after reconnection or power recovery; never discard output because the network is temporarily unavailable.

Use a stable linear slot index for configurable grids. Expansion preserves complete per-slot state; shrinking deletes overflow slots and must be described as destructive. A resized loader reads only persisted entries that both exist and fit the new capacity.

For wildcard routing, retain exact-object and stable equality/hash routes. If a copied detail cannot be mapped to exactly one source slot, reject the push and let AE re-plan; never fall back to slot zero or probe unrelated slots.

## Native ME Part Recipes

Register recipes for GTO ME parts in the same native recipe window used by other GTO recipes. `MEInputAssemblyRecipeRegistration` is the verified example:

1. inject immediately after the single `RecipeFilter.init()` call in `Data.commonInit()`;
2. keep `GTORecipeTypes.ASSEMBLER_RECIPES.recipeBuilder(...)` and `RecipeBuilder.save()` inline in that injected window;
3. pass each returned definition to a receiver that rejects null, dummy, wrong-ID, wrong-type, wrong-I/O, wrong-EUt, or wrong-duration results;
4. validate again immediately after `RecipeBuilder.finish()` and at load-complete, requiring the global and recipe-type tables to retain the same definition objects.

Do not move the builder/save work into an ordinary Forge callback or a Java wrapper outside the native bytecode window. The verified assembly recipes use existing registered ME parts and add no recipe type or EMI integration.

The active examples resolve as follows:

| Raw ID | Final ID | EUt | Duration |
| --- | --- | ---: | ---: |
| `gtohjs:me_input_assembly` | `gtohjs:assembler/me_input_assembly` | 480 | 300t |
| `gtohjs:me_stocking_input_assembly` | `gtohjs:assembler/me_stocking_input_assembly` | 30720 | 300t |

## Preloaded AE Component Packs

The three verified component packs are ordinary Forge items, not GTO machine definitions. `GTOHJSItems` registers their `PreloadedPortableCellItem` instances through a `DeferredRegister<Item>` on the mod event bus, adds them to the tools-and-utilities creative tab, and verifies their `RegistryObject`s at load-complete. Do not inject them into `GTAEMachines.<clinit>()`.

| Item ID | Required type count |
| --- | ---: |
| `gtohjs:basic_ae_component_pack` | 123 |
| `gtohjs:ae_machine_component_pack` | 42 |
| `gtohjs:advanced_ae_hatch_component_pack` | 21 |

Each source key is preloaded with `16L * 1024L * 1024L`, or `16,777,216`, and a new pack starts with `20,000 AE` power. Keep the amount in the single `AEComponentPackContents.AMOUNT_PER_TYPE` constant and the full-power value in the single `PreloadedPortableCellItem.FULL_POWER` constant. Validate the exact type count for every immutable content list.

Their contents are initialized once on the logical server through GTO's authoritative external `CellDataStorage`. Follow these invariants:

- resolve the exact immutable registry-ID list and fail if any required item is absent;
- allocate a fresh storage UUID for every new stack; never copy a source cell UUID or share one between packs;
- write exact `AEKey` entries and `long` quantities to the authoritative external map, then let the real storage-cell API persist and recalculate summary fields;
- when the product explicitly requires an over-capacity preload, use the verified direct external-store path rather than the capacity-limited interactive insertion path;
- write the initialized/content-version markers only after UUID, key count, amounts, and byte/type summaries validate and after setting maximum/current power to the full-power constant; verify power by readback after save and restart as an acceptance check;
- keep initialization server-only and idempotent across inventory ticks, menu opens, saves, and restarts.

For content-version migration, preserve the original UUID and every existing quantity. Merge only absent keys, persist the external store while the stack still advertises the old version, and advance the version marker only after a synchronous save proves the storage is no longer dirty. On any failure, restore the original key map, byte count, and summaries and durably save that rollback. `PreloadedPortableCellItem`, `AEComponentPackContents`, and `docs/28_preloaded_ae_component_packs.md` are the verified implementation and contract.

On the client, register the three item color handlers through `GTOHJSItemColorRegistration` and reuse `AbstractPortableCell.getColor`. Their item models inherit `ae2:item/portable_item_cell_256k`; do not copy or override AE2 textures merely to preserve portable-cell tint and transforms.

## AE UI

Use GTO's existing mode configurator contract. The active super buffers replace only their own configurator through an ID/class-scoped bridge and leave native buffers unchanged. Long mode lists use a five-row, row-snapping scroll viewport. Server setters remain authoritative and client packets contain only selected indexes.

## Network Placement (Build/Load Verified; Hands-On Placement Pending)

The ME Placement Tool GTO port has passed Java integration build, GTO client loading, recipe-table, and resource-cache validation. Actual placement, rollback, multiblock undo, cable modes, and NBT-sensitive behavior still require hands-on player verification. Its current implementation uses these transaction rules:

1. create the action source with both player and wireless access point;
2. read cached inventory matches without mutating or retaining shared counters;
3. perform `SIMULATE -> MODULATE -> placement`;
4. retain the original `AEKey` before extraction;
5. on placement failure, reinsert by that key and return a newly created stack to the player if reinsertion fails;
6. never use a zero-count temporary `ItemStack` as rollback identity;
7. place AE parts through the target runtime's `PartPlacement` path.

Keep the standalone tool's namespace, network channel, configuration lifecycle, resources, and license notices independent from GTOHJS. Recipe IDs still resolve through GTO's native crafting window.

## Validation

- registry identity in the native AE machine window;
- delayed ability membership;
- item and fluid configuration/consumption from one node;
- exact simulation/commit amounts and rollback on failure;
- offline persistence and reconnect retry;
- NBT-sensitive item keys and `long` quantities;
- native ME-part recipe identity in both finalized recipe tables;
- unique external-storage UUIDs, exactly-once component-pack initialization, readback, migration, and durable rollback;
- client UI synchronization and dedicated-server loading;
- no changes to EMI, AE2, GTOCore, or GTOLib dependency files.
