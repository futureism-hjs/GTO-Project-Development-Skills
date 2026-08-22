# Vacuum Level Integration

Use this document only for `/真空等级相关`. Read [Shared workflow](shared-workflow.md) for cover registration windows and [Evidence and validation map](verified-source-map.md) for the verified vacuum cover and pump implementations.

## Passive Cover Extension

The verified extension is a passive cover with vacuum tier 3:

1. register a `CoverDefinition` before `GTOCovers.<clinit>()V` returns;
2. constrain `canAttach()` to ordinary single-block machines and maintenance parts;
3. reject unrelated multiblock controllers and parts;
4. supplement `VacuumCondition.testCondition` only for supported tiers and hosts;
5. return to the original GTO condition logic when the extension does not satisfy the request;
6. mark recipe logic dirty and refresh tick subscription after attach/remove;
7. validate cover registry identity at load-complete.

Never replace the full native condition with a custom approximation. In the verified implementation, required tiers 1-3 may be satisfied by the cover, tier 4 is never satisfied by it, and a multiblock controller searches only its maintenance parts for the cover.

## Vacuum Machines

When creating a vacuum machine rather than a cover, reuse GTOCore's steam/electric vacuum-pump pattern: the specialized machine implements the vacuum interface, its definition advertises the supported tier, and recipes use the native vacuum condition. Do not infer a new tier formula from a renderer or tooltip.

## Validation

Validate cover identity, attachment host boundaries, native-condition fallback, recipe-logic invalidation, tick subscription, machine vacuum tier, and client/server loading. Keep vacuum tooltip text synchronized with the actual supported tier.
