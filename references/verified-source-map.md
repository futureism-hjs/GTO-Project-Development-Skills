# Verified Source Map

This map names implementations known to match the Minecraft 1.20.1 / GTOCore 0.5.6-beta baseline. Locate them with `rg --files` and `rg -n`; do not assume a machine-specific absolute workspace path.

## Active GTOHJS Documentation

| Topic | Document |
| --- | --- |
| Single-block steam/electric registration | `docs/01_single_block_steam_electric.md` |
| Low-level steam multiblocks | `docs/02_low_level_steam_multiblock.md` |
| Large steam multiblocks | `docs/03_advanced_steam_multiblock.md` |
| Electric multiblocks | `docs/04_electric_multiblock_basic.md` |
| Advanced hatches and abilities | `docs/05_electric_multiblock_advanced_hatches.md` |
| Modules, tooltips, and preview | `docs/06_machine_modules_and_preview.md` |
| Recipe types | `docs/07_recipe_type_page_registration.md` |
| Recipes | `docs/08_recipe_registration.md` |
| GTOCore/GTOLib part audit | `docs/09_gtocore_gtolib_parts_audit.md` |
| Parallel and thread behavior | `docs/24_fix64_unbounded_custom_parallel_threads.md` |
| ME input assemblies and delayed validation | `docs/25_fix65_me_input_assemblies.md`, `docs/26_fix66_deferred_me_ability_validation.md` |
| ME Placement Tool port | `docs/34_me_placement_tool_gto_port.md` |
| Super pattern buffers | `docs/36_me_super_pattern_buffer_config.md`, `docs/37_me_super_wildcard_pattern_buffer.md` |

Use the active registration template at the project root as the first code-shaped reference.

## Active GTOHJS Implementations

| Capability | Implementation |
| --- | --- |
| Electric multiblock | `OneStopRareEarthProcessingPlantRegistration` |
| Large steam multiblock | `UniversalSteamFactoryRegistration` |
| Recipe type | `OneStopRareEarthRecipeTypeRegistration`, `FragmentWorldCollectionRecipeTypeRegistration` |
| Native recipe construction | `OneStopRareEarthRecipeRegistration`, `FragmentWorldCollectionRecipeRegistration`, `gtohjs_machine_registration.js` |
| Left-side filter tab | `AdvancedInfiniteIntakeHatchPartMachine` |
| Scrollable mode tabs | `PatternBufferModeSupport`, `ScrollablePatternBufferModeFancyConfigurator`, `UniversalSteamFactoryModeSupport` |
| Input part behavior | `AdvancedInfiniteIntakeHatchPartMachine`, `MEInputAssemblyPartMachine`, `MEStockingInputAssemblyPartMachine` |
| Item descriptions | `GTOHJSItemTooltipHandler`, `assets/gtohjs/lang/en_us.json`, `assets/gtohjs/lang/zh_cn.json` |
| Layered tiered rendering | `MEInputAssemblyRegistration`, `MESuperPatternBufferRegistration` |
| Vacuum | `VacuumCoverRegistration`, `VacuumCoverBehavior`, `VacuumCoverSupport`, `gtohjs_vacuum_cover.js` |
| AE registration | `MEInputAssemblyRegistration`, `MESuperPatternBufferRegistration`, `MESuperWildcardPatternBufferRegistration` |
| AE output and routing | `MESuperPatternBufferPartMachine`, `MESuperWildcardPatternBufferPartMachine`, `MEOutputCapablePatternBufferPartMachine` |

The stable intake definition is registered by the existing intake registration methods in `ThermalAndIntakeHatchRegistration`. Use only those intake methods as evidence; do not generalize unrelated registrations from the same file.

## Read-Only Native References

| Capability | Native reference |
| --- | --- |
| Steam single-block pair | GTCEu `GTMachines.STEAM_MACERATOR`; GTOCore `GTOMachines.STEAM_VACUUM_PUMP` |
| Electric single-block tiers | GTOCore `MachineRegisterUtils.registerSimpleMachines`; `GTOMachines.VACUUM_PUMP` |
| Low/large steam multiblocks | GTOCore `SteamMultiblockMachine`, `LargeSteamMultiblockMachine`, and matching definitions in `MultiBlockA` / `MultiBlockC` |
| Functional parts | GTOCore `GTOMachines` thread/overclock/acceleration definitions; `GCYMMachines.PARALLEL` |
| Ability collections | GTOCore `GTOPartAbility` |
| Maintenance patterns | GTOCore and GCYM multiblocks using exact `MAINTENANCE` predicates |
| Multiblock layer composition | Native `.workableCasingRenderer(casing, overlay)` definitions |

Never modify these dependency trees. If their current source conflicts with active documentation, update the fact base first, then adapt the active project.


