# AGENTS — `com.air.unity-behavior-tree`

**Last Updated:** 2026-06-02 · **Owner:** package maintainers · **Scope:** canonical agent entry (this repository, English)

## Canonical rule

This file is the agent entrypoint for **this** Git repository (`com.air.unity-behavior-tree`). If another file in this repo conflicts with it, this file wins.

## Package scope

| Property | Value |
|----------|--------|
| **UPM id** | `com.air.unity-behavior-tree` |
| **Layer** | Domain — Unity behavior trees on Node Graph Processor |
| **Repository** | [unity-behavior-tree](https://github.com/Airuxul/unity-behavior-tree.git) |
| **Meta index** | [AirUnityPackage `config/registry.json`](https://github.com/Airuxul/AirUnityPackage/blob/main/config/registry.json) |
| **Feature tag** | `domain-behavior-tree` |

**In scope:** Visual `BehaviorTreeGraph` authoring, JSON export, runtime traversal (`BehaviorTreeProcessor`), `BehaviorTreeRunner`, built-in control/action/decorator nodes, editor debug overlay.

**Out of scope:** `com.air.unity-game-core`, `GameRuntime`, UI, CLI/HTTP, gameplay-tag, timeline. Do not add a dependency on Air L0/L1 packages unless `package.json` and product owners explicitly require it.

## Dependencies

| Package | Role |
|---------|------|
| `com.alelievr.node-graph-processor` | `BaseGraph`, export, `RuntimeGraph` / `RuntimeGraphBuilder` |

Runtime asmdef references only `com.alelievr.NodeGraphProcessor.Runtime`. Editor asmdef references BehaviorTree runtime + Node Graph Processor Editor/Runtime.

## User documentation

| File | Language |
|------|----------|
| [README.md](../README.md) | English |
| [README.zh-CN.md](../README.zh-CN.md) | Chinese — must stay in sync with English |
| [TODO.zh-CN.md](../TODO.zh-CN.md) | Chinese backlog — IDs sync with [TODO.md](TODO.md) |

## Agent documentation (this repo)

| File | Purpose |
|------|---------|
| [AGENTS.md](AGENTS.md) | This file |
| [DOC_GOVERNANCE.md](DOC_GOVERNANCE.md) | Doc workflow; skills live in meta repo only |
| [CHANGELOG_AGENT.md](CHANGELOG_AGENT.md) | Agent-side changelog |
| [TODO.md](TODO.md) | English optimization backlog |
| [TODO.md](TODO.md) | Optimization backlog (existing features; meta [TODO_ROADMAP](https://github.com/Airuxul/AirUnityPackage/blob/main/docs/TODO_ROADMAP.md)) |

Do **not** add `QUICKSTART.md` when install and workflow fit in README. Do **not** create `.cursor/skills/` in this package.

## Code layout

| Path | Responsibility |
|------|----------------|
| `Runtime/BehaviorTreeRunner.cs` | `MonoBehaviour` — load JSON `TextAsset`, tick processor |
| `Runtime/BehaviorTreeProcessor.cs` | Tree traversal from root; not compute-order execution |
| `Runtime/BehaviorTreeNodeRegistration.cs` | `RuntimeInitializeOnLoadMethod` node creator registration |
| `Runtime/Nodes/` | `RuntimeBT*` nodes (`Control/`, `Action/`, `Decorator/`, `Base/`) |
| `Runtime/NodeParamData/` | Serializable param blobs for export JSON |
| `Editor/BehaviorTreeGraph.cs`, `BehaviorTreeWindow.cs` | Graph asset + editor window |
| `Editor/Nodes/` | `BT*` editor nodes (mirror `Runtime/Nodes/`) |
| `Editor/Views/` | Graph view, toolbar (export), runtime debug status |

**Namespaces:** `Air.BehaviorTree` (runtime), `Air.BehaviorTree.Editor` (editor). **Naming:** Editor `BT*` / `*Node`; Runtime `RuntimeBT*`.

When adding a node type, register a creator in `BehaviorTreeNodeRegistration` and pair Editor + Runtime implementations.

## Meta repository standards

When editing C#, layering, or cross-package boundaries, follow [AirUnityPackage](https://github.com/Airuxul/AirUnityPackage) docs:

| Document | Use when |
|----------|----------|
| [C_SHARP_STANDARDS](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/C_SHARP_STANDARDS.md) | §4.4 domain folder layout, asmdef, `BT*` / `RuntimeBT*` naming |
| [ARCHITECTURE](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_ARCHITECTURE.md) | Domain vs L0/L1/L2 ownership |
| [CONSTRAINTS](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_CONSTRAINTS.md) | `unity-behavior-tree` → `node-graph-processor` only |
| [PACKAGE_TAGS](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_TAGS.md) | `domain-behavior-tree` |

## Cursor skills (meta repo only)

| Skill | Location | Use |
|-------|----------|-----|
| `doc-read-index` | `AirUnityPackage/.cursor/skills/doc-read-index/SKILL.md` | Read-only doc inventory, gaps, language parity |
| `doc-generate-update` | `AirUnityPackage/.cursor/skills/doc-generate-update/SKILL.md` | Apply doc updates with dual-track rules |

**Do not** copy skills into this submodule.

## Required reads before doc updates

1. `docs/AGENTS.md` (this file)
2. `docs/DOC_GOVERNANCE.md`
3. `README.md` and `README.zh-CN.md`
4. Meta [DOC_GOVERNANCE](https://github.com/Airuxul/AirUnityPackage/blob/main/docs/DOC_GOVERNANCE.md) when validation or hooks apply to a meta-repo commit

## Language policy

- Agent markdown in `docs/` is English.
- User-facing changes require both `README.md` and `README.zh-CN.md`.

## Dual-track update rule

| Change type | Action |
|-------------|--------|
| User-visible (install, nodes, export, runner) | Update both READMEs **and** relevant `docs/*.md` |
| Agent-only `docs/*.md` | Update README only if user-visible behavior changed |
| Code-only, no user impact | README optional; append `CHANGELOG_AGENT.md` for non-trivial agent edits |

## Code change reminders

- One main type per file; namespace matches asmdef `rootNamespace` and folder.
- No `UnityEditor` in `Runtime/`.
- New nodes: Editor class + Runtime class + `BehaviorTreeNodeRegistration` entry.
- Do not reference `com.air.unity-game-core` asmdefs from this package without an explicit architecture change.
