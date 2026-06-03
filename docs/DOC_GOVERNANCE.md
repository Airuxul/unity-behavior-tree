# Documentation Governance — `com.air.unity-behavior-tree`

**Last Updated:** 2026-06-02 · **Owner:** package maintainers · **Scope:** documentation workflow (English)

## Purpose

Define how user and agent documentation is structured and updated in **this** Git submodule, while linking enforceable standards in the [AirUnityPackage](https://github.com/Airuxul/AirUnityPackage) meta repository.

## Document layout

| Track | Paths | Language |
|-------|--------|----------|
| User | `README.md`, `README.zh-CN.md` | English + Chinese (must stay in sync) |
| Agent | `docs/AGENTS.md`, `docs/DOC_GOVERNANCE.md`, `docs/CHANGELOG_AGENT.md` | English |

There is **no** `config/` or `tools/validate-docs.ps1` in this package repository. Validation hooks run in the meta repo when this submodule is part of an AirUnityPackage working tree.

## Cursor skills — meta repository only

Cursor Agent skills **`doc-read-index`** and **`doc-generate-update`** exist **only** under the meta repo:

`https://github.com/Airuxul/AirUnityPackage/tree/main/.cursor/skills/`

**Do not** create `.cursor/skills/` or any `SKILL.md` under `com.air.unity-behavior-tree`. Submodule doc updates are performed from the meta repo (or by following the same rules manually).

## Metadata (recommended)

Each managed markdown file should include near the top:

- **Last Updated** (date)
- **Owner** (`package maintainers` or team name)
- **Scope** (user vs agent)

## Update workflow

1. Read [AGENTS.md](AGENTS.md) and this file.
2. From the meta repo, run skill `doc-read-index` when unsure what is out of date.
3. Classify impact: user README / agent `docs/` / code-only.
4. Apply changes; keep `README.md` and `README.zh-CN.md` aligned for user-visible edits.
5. Append a dated summary to [CHANGELOG_AGENT.md](CHANGELOG_AGENT.md) for non-trivial agent doc work.
6. If committing from **AirUnityPackage**, run `.\tools\validate-docs.ps1` or use `.\tools\install-git-hooks.ps1` (meta repo only).

## Cross-repo standards

| Topic | Authority |
|-------|-----------|
| Domain layering | [ARCHITECTURE.md](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_ARCHITECTURE.md), [CONSTRAINTS.md](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_CONSTRAINTS.md) |
| C# layout (§4.4 behavior-tree) | [C_SHARP_STANDARDS.md](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/C_SHARP_STANDARDS.md) |
| Feature tag `domain-behavior-tree` | [PACKAGE_TAGS.md](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_TAGS.md) |
| Meta doc governance | [DOC_GOVERNANCE.md](https://github.com/Airuxul/AirUnityPackage/blob/main/docs/DOC_GOVERNANCE.md) |

## What not to add

- `.cursor/skills/` or copied SKILL.md files in this package
- Redundant `QUICKSTART.md` when README covers install and authoring flow
- Duplicate registry tables (source of truth: meta `config/registry.json`)
- Documentation implying a dependency on `com.air.unity-game-core` (unless `package.json` changes)

## Domain dependency contract

Document and preserve:

- **Only** UPM dependency: `com.alelievr.node-graph-processor`
- **No** `com.air.unity-game-core`, `com.air.game-core`, or `com.air.unity-ui` in `package.json` / runtime asmdef unless explicitly approved and updated everywhere (README, AGENTS, CONSTRAINTS in meta).
