# Left-Side Tab Writing

Use this document only for `/左侧标签栏编写`. Read [Shared workflow](shared-workflow.md) for lifecycle boundaries and [Evidence and validation map](verified-source-map.md) for the active UI examples.

## Established Fancy UI Path

For a part that already inherits standard tabs, extend the inherited contract:

```java
@Override
public void attachSideTabs(TabsWidget sideTabs) {
    super.attachSideTabs(sideTabs);
    sideTabs.attachSubTab(new FeatureConfigurator(this));
}
```

The verified filter-tab example is `AdvancedInfiniteIntakeHatchPartMachine`. Verified scrollable mode-tab examples are `PatternBufferModeSupport`, `ScrollablePatternBufferModeFancyConfigurator`, and `UniversalSteamFactoryModeSupport`.

## Contract

- Call `super.attachSideTabs` unless intentionally replacing the complete inherited contract.
- Implement the tab as `IFancyUIProvider` with a translated title, icon, tooltip, and stable page dimensions.
- Keep server authority in the machine setter; client actions send only the requested value.
- Synchronize initial state and changes through the established data-sync annotations or `FriendlyByteBuf` path.
- Constrain selection indexes on the server and keep client-only widgets behind the client distribution boundary.
- Use the inherited standard working-enable tab instead of duplicating a start/stop button inside an unrelated filter page.

For long mode lists, use a fixed-height viewport. The active row-snapping configurators show at most five 20-pixel rows, use the native vertical scrollbar, and move one row per wheel step.

Validate initial synchronization, server-side bounds, translated keys, stable dimensions, client rendering, and dedicated-server class loading.
