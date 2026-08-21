# UI and Item Descriptions

## Left-Side Tabs

Use the machine's established Fancy UI path. For a part that already inherits standard tabs:

```java
@Override
public void attachSideTabs(TabsWidget sideTabs) {
    super.attachSideTabs(sideTabs);
    sideTabs.attachSubTab(new FeatureConfigurator(this));
}
```

The verified filter-tab example is `AdvancedInfiniteIntakeHatchPartMachine`. The verified scrollable mode-tab examples are `PatternBufferModeSupport`, `ScrollablePatternBufferModeFancyConfigurator`, and `UniversalSteamFactoryModeSupport`.

Requirements:

- call `super.attachSideTabs` unless intentionally replacing the complete inherited contract;
- implement the tab as `IFancyUIProvider` and give it a translated title, icon, tooltip, and stable page dimensions;
- keep server authority in the machine setter; client actions send only the requested value;
- synchronize initial state and changes with the existing data-sync annotations or `FriendlyByteBuf` path;
- constrain selection indexes on the server;
- keep client-only widget/render code behind the client distribution boundary;
- use the inherited standard working-enable tab instead of duplicating a start/stop button inside an unrelated filter page.

For long mode lists, use a fixed-height scroll viewport. The active row-snapping configurators show at most five 20-pixel rows, use the native vertical scrollbar, and move one row per wheel step.

## Item Descriptions

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

Use an `ItemTooltipEvent` handler only for cross-cutting metadata or item-specific text that the machine builder does not own. For the verified vacuum-cover example, inspect only the `vacuum_cover` conditional and the shared provenance line in `GTOHJSItemTooltipHandler`. Do not copy its complete registry-ID allowlist or unrelated adjacent feature keys into a new implementation.

Keep technical claims synchronized with actual behavior. Translate placeholders identically across languages, preserve formatting arguments, and validate both JSON files with a structured parser. Do not hard-code visible user text in widgets when a translation key can be used.
