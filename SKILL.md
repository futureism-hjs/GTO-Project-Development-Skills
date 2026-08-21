---
name: gto-project-development-skills
description: Develop and maintain GregTech Odyssey machine registrations, recipes, recipe types, side tabs, multiblock parts, layered renderers, vacuum behavior, maintenance integration, item descriptions, and AE2/ME integrations by reusing verified GTOHJS and native GTO patterns. Use when a request selects one of this skill's Chinese command routes or asks for the same GTO development work.
---

# GTO Project Development Skills

Use this skill for the GTO 0.5.6-beta development baseline. Preserve the active project's existing architecture and registration windows.

## Command Router

The Chinese strings below are task selectors inside this skill. Codex does not register arbitrary native slash commands. Explicit invocation is `$gto-project-development-skills /命令`; a direct `/命令` request may also select this skill through its description.

| Selector | Read |
| --- | --- |
| `/蒸汽单方块机器注册` | [Machine registration](references/machine-registration.md#steam-single-block-machines) |
| `/电力单方块机器注册` | [Machine registration](references/machine-registration.md#electric-single-block-machines) |
| `/蒸汽类多方块机器注册` | [Machine registration](references/machine-registration.md#steam-multiblock-machines) |
| `/电力多方块机器注册` | [Machine registration](references/machine-registration.md#electric-multiblock-machines) |
| `/配方注册` | [Recipes and recipe types](references/recipes-and-recipe-types.md#recipe-registration) |
| `/配方类型注册` | [Recipes and recipe types](references/recipes-and-recipe-types.md#recipe-type-registration) |
| `/左侧标签栏编写` | [UI and item descriptions](references/ui-and-item-descriptions.md#left-side-tabs) |
| `/物品介绍编写` | [UI and item descriptions](references/ui-and-item-descriptions.md#item-descriptions) |
| `/功能性仓室注册` | [Hatches and maintenance](references/hatches-and-maintenance.md#functional-hatches) |
| `/输入输出类仓室注册` | [Hatches and maintenance](references/hatches-and-maintenance.md#input-and-output-parts) |
| `/维护功能相关` | [Hatches and maintenance](references/hatches-and-maintenance.md#maintenance-integration) |
| `/组合类材质渲染` | [Rendering and vacuum](references/rendering-and-vacuum.md#layered-rendering) |
| `/真空等级相关` | [Rendering and vacuum](references/rendering-and-vacuum.md#vacuum-level-integration) |
| `/ae相关` | [AE integration](references/ae-integration.md) |

Always read [Shared workflow](references/shared-workflow.md). Use [Verified source map](references/verified-source-map.md) to locate the closest working implementation before editing.

## Non-Negotiable Constraints

- Read the nearest `AGENT.md` or `AGENTS.md`, the project index, and the feature document before changing code.
- Reuse a verified implementation that already solves the request. Adapt it narrowly instead of creating a parallel framework.
- Treat dependency and upstream trees as read-only. Do not modify EMI; correctly registered GTO definitions and recipe tables feed EMI automatically.
- Keep client-only renderers and widgets behind the established distribution boundary.
- Use Java 21 for Gradle, allow dependency downloads, and run a clean build unless the user explicitly changes the build requirement.
- Update the English development document, task checklist, and read index in the same task as the behavior change.
- Do not promote experimental or incomplete code into a template.


