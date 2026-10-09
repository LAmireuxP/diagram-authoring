# diagram-authoring

图表绘制技能包。按 GB/T 1526-1989、UML 2.x、Crow's Foot 等规范生成图表文件，默认输出 draw.io XML（.drawio，可编辑），要求文本图表时输出 Mermaid。

## 获取

```bash
git clone https://github.com/LAmireuxP/diagram-authoring.git
```

或在仓库页 Code → Download ZIP 下载。

## 目录结构

```text
diagram-authoring/
├── SKILL.md
├── references/
│   ├── symbols-gbt1526.md   GB/T 1526-1989 符号、流线、判断分支布局
│   ├── diagram-types.md     各类型图表的结构与布局约定
│   ├── format-drawio.md     draw.io XML：语法、样式、泳道、自检
│   └── format-mermaid.md    Mermaid：适用场景、语法要点、符号近似对照
└── examples/
    ├── flowchart-gbt.drawio
    ├── swimlane-cross-functional.drawio
    ├── architecture-layered.drawio
    ├── flowchart-mermaid.mmd
    └── architecture-mermaid.mmd
```

## 安装

### Claude Code（项目级或用户级）

```bash
# 项目级
mkdir -p .claude/skills/diagram-authoring

# 用户级
mkdir -p ~/.claude/skills/diagram-authoring
```

把 `SKILL.md`、`references/`、`examples/` 放入对应目录，重启 Claude Code 或运行 `/doctor` 生效。

### Claude.ai 网页版 / Claude Desktop

1. 设置 → Capabilities 开启 Code execution and file creation
2. 把 `diagram-authoring/` 压缩为 zip
3. 设置 → Customize → Skills → “+” → Upload skill，上传 zip

### 兼容性

遵循 Agent Skills 标准（SKILL.md 格式），Claude Code、Cursor、Codex CLI 等可加载。

## 依赖

- 生成 .drawio / .mmd：无依赖
- 导出图片（可选）：draw.io 桌面版导出 .drawio；mermaid-cli（mmdc）导出 .mmd。未安装时手动导出：app.diagrams.net、mermaid.live
- 打开 / 编辑：app.diagrams.net（.drawio）；mermaid.live 或支持 Mermaid 的 Markdown 渲染器（.mmd）

## 示例

| 文件 | 内容 |
|---|---|
| flowchart-gbt.drawio | GB/T 1526 用户登录流程，含回退分支与跳线弧 |
| swimlane-cross-functional.drawio | 请假审批跨职能泳道图 |
| architecture-layered.drawio | 三层系统架构图 |
| flowchart-mermaid.mmd | 同一登录流程的 Mermaid 写法 |
| architecture-mermaid.mmd | 同一分层架构的 Mermaid 写法（subgraph 分层） |