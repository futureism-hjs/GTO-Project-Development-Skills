# Repository Maintenance Rules

This repository contains the `gto-project-development-skills` Codex skill.

1. Write all instructions, references, indexes, progress records, and maintenance notes in English. Keep the user-defined Chinese command selectors unchanged.
2. Base guidance on verified active-project implementations first, then on the matching GTOCore, GTOLib, or GTCEu source for the target runtime. Never invent a second implementation when a verified path already exists.
3. Treat GTOCore, GTOLib, GTCEu, AE2, GTL, community source, modpack resources, and EMI as read-only references. Never instruct an agent to patch EMI.
4. Do not add incomplete or experimental features to this skill. A feature must have a stable implementation and an evidence-backed validation contract before it becomes a template.
5. Keep `SKILL.md` concise. Put command-specific mechanics in `references/` and link every reference from `SKILL.md`.
6. Preserve the runtime baseline unless current project evidence proves it changed: Minecraft 1.20.1, Forge 47.4.20, GTOCore 0.5.6-beta, GTOLib 26.7.4, GTCEu 26.7.3, and Java 21.
7. Validate every change with the bundled skill validator. For GTO code changes, use Java 21, an online Gradle resolution path, and a clean build unless the user explicitly changes the build boundary.


