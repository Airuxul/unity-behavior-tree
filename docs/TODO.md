# TODO — `com.air.unity-behavior-tree`

**Last Updated:** 2026-06-03 · **Owner:** package maintainers · **Scope:** behavior-tree domain follow-ups (English)

> **User doc (Chinese):** [../TODO.zh-CN.md](../TODO.zh-CN.md)

> **Boundary:** BT graph authoring, export consumption, tree `Tick()` processor, BT runtime/editor nodes.  
> **Depends on:** `com.alelievr.node-graph-processor` for graph export/runtime graph — **do not duplicate** NGP core.  
> **Meta rollup:** [AirUnityPackage `docs/TODO_ROADMAP.md`](https://github.com/Airuxul/AirUnityPackage/blob/main/docs/TODO_ROADMAP.md)

## Capability baseline

- `BehaviorTreeGraph` + editor window/view (child order via `ISortChildrenByPosition`)
- JSON export → `BehaviorTreeRunner` + `BehaviorTreeProcessor` (tree traversal, not full-graph compute)
- Nodes: Root, Sequence, Selector, Parallel, Log, Wait, Invert
- Play-mode debug overlay on selected runner

## TODO

| ID | Pri | Title | Description |
|----|-----|-------|-------------|
| BT-01 | P0 | Post-build validation | Null nodes, missing root, orphan nodes, decorator child count after `RuntimeGraphBuilder`. |
| BT-02 | P0 | Export / tick smoke tests | Sequence resume, Selector fail-through, Parallel threshold, Invert. |
| BT-03 | P1 | Null-child hardening | `RuntimeBTInvertNode` / `GetChild()` when graph incomplete. |
| BT-04 | P1 | Play-mode debug binding | Pin runner, multi-runner, or match `sourceGraphPath` vs `Selection` only. |
| BT-05 | P1 | Node registration | Reflection or codegen registry per `BehaviorTreeNodeRegistration` comment. |
| BT-06 | P2 | Wait semantics docs | Tick-based wait vs wall-clock; exposed-parameter wiring. |
| BT-07 | P2 | BT export UX wrapper | Defaults for export path/name; optional one-click `TextAsset`. |
| BT-08 | P3 | Naming alignment | `RuntimeBTRootBaseNode.cs` vs `RuntimeRootBaseNode` class name. |
| BT-09 | P3 | Custom-node checklist | README section for `RuntimeNodeType`, export JSON, editor mirror. |

## Do not assign here

| Topic | Owner package |
|-------|----------------|
| `BaseGraph`, export schema, `RuntimeGraph` caches | `com.alelievr.node-graph-processor` |
| `ProcessGraphProcessor` compute-order runs | `com.alelievr.node-graph-processor` |
| `GameRuntime` / blackboard wiring | Game project or `com.air.unity-game-core` consumer |
| Generic non-BT graph nodes | NGP or game code |
