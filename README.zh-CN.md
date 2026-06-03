# 行为树（`com.air.unity-behavior-tree`）

[English](README.md)

基于 **Node Graph Processor** 的行为树：可视化编辑、JSON 导出与运行时执行，用于 AI 与游戏逻辑。

## 概要

| 项 | 值 |
|----|-----|
| **分层** | 域包 — 独立 Unity 模块 |
| **UPM id** | `com.air.unity-behavior-tree` |
| **版本** | 0.1.0 |
| **程序集** | `com.air.BehaviorTree.Runtime` / `com.air.BehaviorTree.Editor` → `Air.BehaviorTree` |
| **Unity** | 2020.3+ |
| **UPM 依赖** | 仅 `com.alelievr.node-graph-processor` 1.3.1+ |
| **不得依赖** | `com.air.unity-game-core`、`com.air.game-core`、`com.air.unity-ui` |

在 meta 仓库 [AirUnityPackage](https://github.com/Airuxul/AirUnityPackage) 中索引（`config/registry.json`，标签 `domain-behavior-tree`，默认安装）。

## 安装

在 Unity 项目 `Packages/manifest.json` 中（路径按你的克隆调整）：

```json
{
  "dependencies": {
    "com.air.unity-behavior-tree": "file:../CustomPackages/packages/com.air.unity-behavior-tree"
  }
}
```

UPM 会自动解析 `com.alelievr.node-graph-processor`。Git 子模块：`packages/com.air.unity-behavior-tree` → [unity-behavior-tree](https://github.com/Airuxul/unity-behavior-tree.git)。

## 快速开始

1. **创建图：** **Assets → Create → Behavior Tree → Graph**（或打开已有 `BehaviorTreeGraph`）。
2. **编辑：** 在行为树窗口中添加节点；在控制节点（Sequence、Selector、Parallel）上连接子节点。
3. **导出：** 工具栏 **Export** → 保存 JSON（如 `Resources/` 下的 `TextAsset` 或项目约定目录）。
4. **运行：** 在 `GameObject` 上添加 `BehaviorTreeRunner`，指定导出的 JSON `TextAsset`，选择 **Once** / **Update** / **FixedUpdate**。

`BehaviorTreeNodeRegistration` 在首场景加载前注册内置运行时节点类型。

## 内置节点

| 类别 | Editor | Runtime |
|------|--------|---------|
| 根 | `RootNode` | `RuntimeRootBaseNode` |
| 控制 | `BTSequenceNode`、`BTSelectorNode`、`BTParallelNode` | `RuntimeBTSequenceNode`、`RuntimeBTSelectorNode`、`RuntimeBTParallelNode` |
| 动作 | `BTLogNode`、`BTWaitNode` | `RuntimeBTLogNode`、`RuntimeBTWaitNode` |
| 装饰 | `BTInvertNode` | `RuntimeBTInvertNode` |

自定义节点：在 `Editor/Nodes/` 与 `Runtime/Nodes/` 中成对添加，并在 `BehaviorTreeNodeRegistration` 中注册。

## 运行时 API

| 类型 | 作用 |
|------|------|
| `BehaviorTreeRunner` | 通过 `RuntimeGraphBuilder.FromJson` 加载 JSON，驱动 `BehaviorTreeProcessor` |
| `BehaviorTreeProcessor` | 从根节点遍历（每节点 `OnProcess`）；返回 `BehaviorTreeStatus` |
| `BehaviorTreeStatus` | `Success`、`Failure`、`Running` |
| `RuntimeGraph` | 导出后的图实例（来自 Node Graph Processor） |

手动 Tick（例如游戏逻辑中调用）：

```csharp
using Air.BehaviorTree;

var status = behaviorTreeRunner.Tick();
```

无 `MonoBehaviour` 时：从 JSON 构建 `RuntimeGraph`，`new BehaviorTreeProcessor()`，`Init(graph)`，每帧 `Tick()`。

## 编辑器

- 双击 `BehaviorTreeGraph` 打开 **Behavior Tree** 窗口。
- 工具栏：居中视图、暴露参数、**Export**、在 Project 中定位资源。
- 播放模式：存在活动的 `BehaviorTreeRunner` 时，图视图可显示运行时节点状态（调试叠加）。

## 目录结构

| 路径 | 用途 |
|------|------|
| `Runtime/Nodes/` | `RuntimeBT*` 执行节点 |
| `Runtime/NodeParamData/` | 动作节点的 JSON 参数数据 |
| `Editor/Nodes/` | `BT*` 编辑节点（与运行时目录对应） |
| `Editor/Views/` | 图视图、工具栏、调试样式 |

域包约定见 meta [C_SHARP_STANDARDS §4.4](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/C_SHARP_STANDARDS.md)。

## 相关

- [Node Graph Processor](../com.alelievr.node-graph-processor/README.md) — 图导出与 `RuntimeGraph` 执行
- [AirUnityPackage PACKAGE_TAGS](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_TAGS.md) — `domain-behavior-tree`
- [AirUnityPackage CONSTRAINTS](https://github.com/Airuxul/AirUnityPackage/blob/main/.cursor/rules/PACKAGE_CONSTRAINTS.md) — 域包依赖规则
