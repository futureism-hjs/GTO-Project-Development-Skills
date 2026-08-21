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

## AE UI

Use GTO's existing mode configurator contract. The active super buffers replace only their own configurator through an ID/class-scoped bridge and leave native buffers unchanged. Long mode lists use a five-row, row-snapping scroll viewport. Server setters remain authoritative and client packets contain only selected indexes.

## Network Placement

The verified ME Placement Tool GTO port uses these transaction rules:

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
- client UI synchronization and dedicated-server loading;
- no changes to EMI, AE2, GTOCore, or GTOLib dependency files.


