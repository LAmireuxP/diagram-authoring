# Mermaid 渲染

## 何时用 Mermaid

- 用户明确要 Mermaid、文本图表，或要粘贴进 Markdown 的内容
- 其余情况用 draw.io：.drawio 可编辑，符号齐全

## 与标准符号的差距

Mermaid flowchart 的形状集合有限，无法完整表达 GB/T 1526 符号。近似写法：

| GB/T 符号 | Mermaid 写法 | 说明 |
|---|---|---|
| 端点符（起止） | `id([文本])` | 胶囊形 |
| 处理 | `id[文本]` | 矩形 |
| 判断 | `id{文本}` | 菱形 |
| 数据（输入/输出） | `id[/文本/]` | 平行四边形 |
| 文档、既定处理、连接符、并行方式 | 无对应 | 流程图严格合规时改用 draw.io |

## 语法要点

- 流程图：`flowchart TD`（纵向）/ `flowchart LR`（横向）；连线 `-->`、虚线 `-.->`、带标签 `-->|是|`
- 时序图：`sequenceDiagram`；`A->>B: 消息`，返回 `B-->>A: 结果`
- 类图：`classDiagram`；继承 `A <|-- B`，组合 `A *-- B`，聚合 `A o-- B`，依赖 `A ..> B`
- ER 图：`erDiagram`；`CUSTOMER ||--o{ ORDER : places`
- 状态图：`stateDiagram-v2`；`[*] --> 状态1`
- 思维导图：`mindmap` 类型（渲染器需较新版本）
- 分组：`subgraph 名称` … `end`，对应泳道或分层
- 数据库节点：`id[(库名)]`
- 样式：`classDef proc fill:#DAE8FC,stroke:#6C8EBF;` 后接 `class n1,n2 proc;`
- 文本含特殊字符时用引号：`A["文本 (x)"]`

## 布局约束

- 判断节点每条出线必须带标签：`A -->|是| B`
- 泳道用 `subgraph 角色名`，节点声明在对应 subgraph 内
- 分层架构用多个 `subgraph` 顺序堆叠
- 不手工调坐标，版式交给渲染器；节点 id 仍要语义化
- 节点 id 避开保留字（`end`、`graph`、`subgraph` 等）

## 示例

`examples/flowchart-mermaid.mmd`、`examples/architecture-mermaid.mmd`。

## 自检清单

- [ ] 无语法错误（能在 mermaid.live 渲染）
- [ ] 节点 id 唯一、语义化，未用保留字
- [ ] 判断出线全部带条件标签
- [ ] 含特殊字符的文本已用引号包裹
- [ ] subgraph 名称与节点 id 不冲突