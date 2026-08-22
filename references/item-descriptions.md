# Item Description Writing

Use this document only for `/物品介绍编写`. Read [Shared workflow](shared-workflow.md) for source-of-truth ordering and [Evidence and validation map](verified-source-map.md) for the stable tooltip example.

## Translation Keys

Put names and text in strict UTF-8 language JSON. For a GTO machine registered in the `gtocore` namespace, provide the keys actually read by the builder and compatibility surfaces:

```text
block.gtocore.<path>
item.gtocore.<path>
machine.gtocore.<path>
```

Use builder tooltips for definition-owned behavior:

```java
.tooltips(
    Component.translatable("mod.machine.example.capacity"),
    Component.translatable("mod.machine.example.function")
        .withStyle(ChatFormatting.AQUA))
```

Use an `ItemTooltipEvent` handler only for cross-cutting metadata or item-specific text that the machine builder does not own. For the verified vacuum-cover example, inspect only the `vacuum_cover` conditional and shared provenance line in `GTOHJSItemTooltipHandler`; do not copy its complete registry-ID allowlist or unrelated localization keys.

## Consistency and Validation

Keep technical claims synchronized with actual behavior. Translate placeholders identically across languages, preserve formatting arguments, and validate both JSON files with a structured parser. Do not hard-code visible user text in widgets when a translation key can be used. Check that tooltip claims match the machine's tier, capacity, abilities, vacuum level, or maintenance contract.
