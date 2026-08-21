# Shared Workflow

## Establish the Fact Base

Use this order:

1. Read the nearest agent instructions and project index.
2. Read the active feature document and registration template.
3. Inspect the current implementation named in the verified source map.
4. Read targeted GTOCore, GTOLib, GTCEu, or AE2 source only when the active documentation is absent, inconsistent, or insufficient for the requested behavior.

Record every opened file in the project's English read index. A scoped `rg` discovery scan may be indexed by directory and glob; any source file used as implementation evidence must be listed individually.

## Select an Existing Contract

Identify the closest machine or part by all of these properties, not by appearance alone:

- registry owner and native registration window;
- controller or part class;
- recipe type and recipe modifier;
- pattern predicates and part abilities;
- UI and synchronization path;
- renderer and distribution boundary;
- tooltip and localization path;
- validation stage.

Copy that contract, then make the smallest required changes. A pattern accepting a hatch does not implement the hatch's effect. An ability declaration does not create handlers. A renderer path does not supply server-side behavior.

## Registration Windows

| Object | Verified native window |
| --- | --- |
| Ordinary GTO machine or multiblock | Before every `RETURN` of the owning machine data class `<clinit>()V`, commonly `GTOMachines` |
| GTO AE/ME part | Before every `RETURN` of `GTAEMachines.<clinit>()V` |
| GTO recipe type | Before every `RETURN` of `GTORecipeTypes.<clinit>()V` |
| GTO recipe builder/save | Immediately after the single `RecipeFilter.init()` call in `Data.commonInit()` |
| GTO cover | Before every `RETURN` of `GTOCovers.<clinit>()V` |

Fail when the expected anchor is absent or ambiguous. Do not silently fall back to ordinary Forge setup for a native GTO registry.

## Validation Stages

Validate at the stage where the relevant state is authoritative:

1. During registration, check non-null definitions and registry identity.
2. After Registrate binding, check ability membership and registered blocks.
3. At load-complete, build patterns, verify renderers, and check definition invariants.
4. After recipe finalization, verify resolved IDs, type, I/O, EUt, duration, and final-table identity.
5. Test client-only UI/render behavior in a client and shared loading behavior on a dedicated server when applicable.

Do not mistake a too-early ability-table read for a failed definition registration.

## Build and Documentation

- Use Java 21 and an online Gradle resolution path.
- Run a clean build unless the user explicitly changes that boundary.
- Follow the active project's deployment and command-line client verification rules after a successful build.
- Write development documents, task/completion records, agent instructions, and indexes in English.
- Preserve literal registry IDs, translation keys, class names, and user-defined Chinese command selectors.


