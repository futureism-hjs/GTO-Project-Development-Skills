---
name: gto-project-development-skills
description: Develop and maintain GregTech Odyssey machine registrations, recipes, recipe types, side tabs, multiblock parts, layered renderers, vacuum behavior, maintenance integration, item descriptions, and AE2/ME integrations by reusing established GTOHJS and native GTO patterns with documented validation boundaries. Use when a request selects one of this skill's Chinese command routes or asks for the same GTO development work.
---

# GTO Project Development Skills

Use this skill for the GTO 0.5.6-beta development baseline. Preserve the active project's existing architecture and registration windows.

## Command Router

The Chinese strings below are task selectors inside this skill. Codex does not register arbitrary native slash commands. Explicit invocation is `$gto-project-development-skills /<selector>`; a direct `/<selector>` request may also select this skill through its description.

| Selector | Read |
| --- | --- |
| `/蒸汽单方块机器注册` | [Steam single-block machines](references/steam-single-block-machines.md) |
| `/电力单方块机器注册` | [Electric single-block machines](references/electric-single-block-machines.md) |
| `/蒸汽类多方块机器注册` | [Steam multiblock machines](references/steam-multiblock-machines.md) |
| `/电力多方块机器注册` | [Electric multiblock machines](references/electric-multiblock-machines.md) |
| `/配方注册` | [Recipe registration](references/recipe-registration.md) |
| `/配方类型注册` | [Recipe type registration](references/recipe-type-registration.md) |
| `/左侧标签栏编写` | [Left-side tabs](references/left-side-tabs.md) |
| `/物品介绍编写` | [Item descriptions](references/item-descriptions.md) |
| `/功能性仓室注册` | [Functional hatches](references/functional-hatches.md) |
| `/输入输出类仓室注册` | [Input and output parts](references/input-output-parts.md) |
| `/维护功能相关` | [Maintenance integration](references/maintenance-integration.md) |
| `/组合类材质渲染` | [Layered rendering](references/layered-rendering.md) |
| `/真空等级相关` | [Vacuum level integration](references/vacuum-level-integration.md) |
| `/ae相关` | [AE integration](references/ae-integration.md) |

At the start of every development task, read the [Command reference index](references/INDEX.md), then read only the selected command document first. Consult [Shared workflow](references/shared-workflow.md) when the task crosses registration windows or lifecycle stages, and consult [Evidence and validation map](references/verified-source-map.md) when choosing an existing implementation or recording acceptance status. This keeps unrelated command guidance out of the active context while preserving the mandatory project-index check.

## Non-Negotiable Constraints

- Read the nearest `AGENT.md` or `AGENTS.md`, the project index, and the feature document before changing code.
- Reuse a verified implementation that already solves the request. Adapt it narrowly instead of creating a parallel framework.
- Treat dependency and upstream trees as read-only. Do not modify EMI; correctly registered GTO definitions and recipe tables feed EMI automatically.
- Keep client-only renderers and widgets behind the established distribution boundary.
- Use Java 21 for Gradle, allow dependency downloads, and run a clean build unless the user explicitly changes the build requirement.
- Update the English development document, task checklist, and read index in the same task as the behavior change.
- Do not promote experimental or incomplete code into a template.
