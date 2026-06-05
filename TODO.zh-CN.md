# 待办 — `com.air.unity-behavior-tree`

**最后更新：** 2026-06-03 · **范围：** 行为树领域现有功能的后续优化（中文）

> **职责边界：** BT 图编辑、导出消费、树 `Tick()` 处理器、BT 运行时/编辑器节点。  
> **依赖：** `com.alelievr.node-graph-processor` 导出与运行时图 — **勿重复实现** NGP 核心。  
> Agent 英文条目：[`docs/TODO.md`](docs/TODO.md)

## 现有能力概要

- `BehaviorTreeGraph` 与编辑器视图（子节点顺序 `ISortChildrenByPosition`）
- JSON 导出 → `BehaviorTreeRunner` + `BehaviorTreeProcessor`（树遍历，非全图计算序）
- 节点：Root、Sequence、Selector、Parallel、Log、Wait、Invert
- 运行模式调试叠加层

## 待办列表

| ID | 优先级 | 标题 | 说明 |
|----|--------|------|------|
| BT-01 | P0 | 构建后校验 | 空节点、缺根、孤儿、装饰器子节点数等。 |
| BT-02 | P0 | 导出与 Tick 冒烟测试 | Sequence/Selector/Parallel/Invert 等语义回归。 |
| BT-03 | P1 | 空子节点防护 | 图不完整时 `Invert`/`GetChild` 不 NRE。 |
| BT-04 | P1 | 调试绑定增强 | 固定 Runner、多 Runner、按 `sourceGraphPath` 匹配。 |
| BT-05 | P1 | 节点注册优化 | 反射或代码生成注册表。 |
| BT-06 | P2 | Wait 语义文档 | 按 Tick  vs 真实时间；暴露参数接线。 |
| BT-07 | P2 | 导出 UX 封装 | 默认路径/一键 `TextAsset`。 |
| BT-08 | P3 | 命名对齐 | 文件名与 `RuntimeRootBaseNode` 类名一致。 |
| BT-09 | P3 | 自定义节点清单 | README 补充注册与 Editor/Runtime 镜像步骤。 |

## 请勿在本包实现

| 主题 | 归属包 |
|------|--------|
| `BaseGraph`、导出格式、`RuntimeGraph` 缓存 | `com.alelievr.node-graph-processor` |
| 全图计算序执行 | `com.alelievr.node-graph-processor` |
| `GameRuntime`/黑板接线 | 游戏工程或 `com.air.unity-game-core` 消费方 |
| 通用非 BT 图节点 | NGP 或游戏代码 |
