# Behavior Tree (`com.air.unity-behavior-tree`)

[简体中文](README.zh-CN.md)

Behavior trees for AI and game logic, built on **Node Graph Processor** — visual editor, JSON export, and runtime execution.

## Quick facts

| Item | Value |
|------|--------|
| **Layer** | Domain — standalone Unity module |
| **UPM id** | `com.air.unity-behavior-tree` |
| **Version** | 0.1.0 |
| **Asmdef** | `com.air.BehaviorTree.Runtime` / `com.air.BehaviorTree.Editor` → `Air.BehaviorTree` |
| **Unity** | 2020.3+ |
| **UPM dependencies** | `com.alelievr.node-graph-processor` 1.3.1+ only |
| **Must not depend on** | `com.air.unity-game-core`, `com.air.game-core`, `com.air.unity-ui` |

Indexed in meta repo [AirUnityPackage](https://github.com/Airuxul/AirUnityPackage) (`config/registry.json`, tag `domain-behavior-tree`, `installDefault: true`).

## Install

In your Unity project `Packages/manifest.json` (adjust path to your clone):

```json
{
  "dependencies": {
    "com.air.unity-behavior-tree": "file:../CustomPackages/packages/com.air.unity-behavior-tree"
  }
}
```

UPM resolves `com.alelievr.node-graph-processor` automatically. Git submodule: `packages/com.air.unity-behavior-tree` → [unity-behavior-tree](https://github.com/Airuxul/unity-behavior-tree.git).

## Quick start

1. **Create graph:** **Assets → Create → Behavior Tree → Graph** (or open an existing `BehaviorTreeGraph` asset).
2. **Author:** Add nodes in the Behavior Tree window; wire children on control nodes (Sequence, Selector, Parallel).
3. **Export:** Toolbar **Export** → save JSON (e.g. `TextAsset` under `Resources/` or your pipeline folder).
4. **Run:** Add `BehaviorTreeRunner` to a `GameObject`, assign the exported JSON `TextAsset`, choose **Once** / **Update** / **FixedUpdate**.

`BehaviorTreeNodeRegistration` registers built-in runtime node types before the first scene loads.

## Built-in nodes

| Category | Editor | Runtime |
|----------|--------|---------|
| Root | `RootNode` | `RuntimeRootBaseNode` |
| Control | `BTSequenceNode`, `BTSelectorNode`, `BTParallelNode` | `RuntimeBTSequenceNode`, `RuntimeBTSelectorNode`, `RuntimeBTParallelNode` |
| Action | `BTLogNode`, `BTWaitNode` | `RuntimeBTLogNode`, `RuntimeBTWaitNode` |
| Decorator | `BTInvertNode` | `RuntimeBTInvertNode` |

Custom nodes: add matching Editor + Runtime types under `Editor/Nodes/` and `Runtime/Nodes/`, then register in `BehaviorTreeNodeRegistration`.

## Runtime API

| Type | Role |
|------|------|
| `BehaviorTreeRunner` | Loads JSON via `RuntimeGraphBuilder.FromJson`, ticks `BehaviorTreeProcessor` |
| `BehaviorTreeProcessor` | Walks tree from root (`OnProcess` per node); returns `BehaviorTreeStatus` |
| `BehaviorTreeStatus` | `Success`, `Failure`, `Running` |
| `RuntimeGraph` | Exported graph instance (from Node Graph Processor) |

Manual tick (e.g. from game code):

```csharp
using Air.BehaviorTree;

var status = behaviorTreeRunner.Tick();
```

Headless / non-`MonoBehaviour` use: build `RuntimeGraph` from JSON, `new BehaviorTreeProcessor()`, `Init(graph)`, then `Tick()` each frame.

## Editor

- Double-click `BehaviorTreeGraph` assets to open **Behavior Tree** window.
- Toolbar: center view, exposed parameters, **Export**, ping asset in Project.
- Play mode: graph view can show runtime node status when a `BehaviorTreeRunner` is active (debug overlay).

## Layout

| Path | Purpose |
|------|---------|
| `Runtime/Nodes/` | `RuntimeBT*` execution nodes |
| `Runtime/NodeParamData/` | JSON param payloads for actions |
| `Editor/Nodes/` | `BT*` authoring nodes (mirror runtime folders) |
| `Editor/Views/` | Graph view, toolbar, debug styling |

See meta [C_SHARP_STANDARDS §4.4](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/C_SHARP_STANDARDS.md) for domain package conventions.

## Related

- [Node Graph Processor](../com.alelievr.node-graph-processor/README.md) — graph export and `RuntimeGraph` execution
- [AirUnityPackage PACKAGE_TAGS](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_TAGS.md) — `domain-behavior-tree`
- [AirUnityPackage CONSTRAINTS](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_CONSTRAINTS.md) — domain dependency rules
