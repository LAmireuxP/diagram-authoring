---
name: diagram-authoring
description: 生成图表文件：流程图（GB/T 1526-1989）、泳道图、系统架构图、ER 图、网络拓扑、UML、思维导图、组织架构图。默认输出 draw.io XML（.drawio，可编辑），要求文本图表时输出 Mermaid。当用户要画图/绘图/流程图/架构图/拓扑图/时序图/ER图，或提到 drawio、diagrams.net、mxGraph、Mermaid 时使用。
license: MIT
---

# 图表绘制

根据用户描述生成图表文件。默认渲染器为 draw.io（输出 .drawio），用户要求 Mermaid 或可粘贴进 Markdown 的文本图表时改用 Mermaid。

## 触发条件

**使用**
- 要画流程图、架构图、泳道图、ER 图、拓扑图、时序图、类图、思维导图、组织架构图
- 提到 draw.io、diagrams.net、mxGraph、Mermaid
- 要生成 .drawio 文件，或把图表导出为 PNG/SVG/PDF

**不使用**
- 只要文字描述或 Markdown 说明
- 要的是照片、插画等非图表图片

## 工作流

1. **定类型**：查「类型速查表」，确定图表类型和规范文件。
2. **读规范**：读该类型的规范文件；流程图类必读 `references/symbols-gbt1526.md`。
3. **选渲染器**：默认 draw.io，读 `references/format-drawio.md`；用户要 Mermaid 或文本图表，读 `references/format-mermaid.md`。
4. **规划生成**：列节点（语义 id，如 `start`、`check_login`）→ 列连线（如 `e_start_check`）→ 排坐标（10px 网格，垂直间距 ≥60，水平 ≥80）→ 写完整文件落盘。

用户明确要求图片（“导出 PNG”“给我一张图”）时，先把源文件写盘，再执行文末「导出图片」节；未要求时不执行，也不做检测。

## 类型速查表

| 用户需求 | 类型 | 规范文件 | 遵循 |
|---|---|---|---|
| 流程图、业务流程、程序流程 | 流程图 | symbols-gbt1526.md、diagram-types.md §1 | GB/T 1526-1989 |
| 跨部门流程、角色分工 | 泳道图 | diagram-types.md §2 | GB/T 1526 符号 |
| 系统架构、部署图 | 架构图 | diagram-types.md §3 | 分层约定 |
| 实体关系、数据库设计 | ER 图 | diagram-types.md §4 | Crow's Foot |
| 网络拓扑、机房布线 | 网络图 | diagram-types.md §5 | 图标约定 |
| 类图、时序图、状态图 | UML | diagram-types.md §6–8 | UML 2.x |
| 头脑风暴、知识整理 | 思维导图 | diagram-types.md §9 | 辐射布局 |
| 汇报结构、部门结构 | 组织架构图 | diagram-types.md §10 | 树形布局 |

## 通用规则（与渲染器无关）

1. 符号按语义选型，不得一律用矩形：起止用端点符、判断用菱形、输入输出用平行四边形。
2. 流向默认自上而下、从左到右；非默认方向必须画箭头。
3. 判断节点一进多出，每条出线标条件（是/否或具体值）。
4. 坐标对齐 10px 网格；同类节点同尺寸；同层对齐；整图四周留 ≥40px。
5. 尽量不交叉；无法避免时用跳线弧或连接符断线。
6. 处理节点文本写“动词+名词”。
7. 泳道、分组容器内的子元素用相对坐标，且不能压在标题栏上：标题在左时 `x ≥ startSize`，标题在上时 `y ≥ startSize`。
8. 分层架构的层背景是视觉矩形，不是容器；层内节点用绝对坐标。

## 生成后自检（通用项）

- [ ] 图表类型与规范文件对应
- [ ] 节点齐全：起止、判断、输入输出
- [ ] 判断分支都有条件标签
- [ ] 坐标对齐、无重叠、无悬空连线
- [ ] 渲染器自检通过（format-drawio.md / format-mermaid.md 末尾清单）

## 交付

- 写完整文件并落盘，不给片段
- 编码 UTF-8
- 说明打开方式：.drawio 用 app.diagrams.net（无需安装）、draw.io 桌面版或 VS Code Draw.io Integration；Mermaid 用 mermaid.live 或支持 Mermaid 的 Markdown 渲染器
- 用户要求图片时转「导出图片」节

---

## 导出图片

仅当用户明确说“导出 PNG/SVG/PDF”“给我一张图”时执行。用户只说“画个图”则不执行，也不做任何检测。

1. 先把源文件写盘；导出失败不回滚、不阻断交付。
2. 按渲染器检测导出工具，只执行一次：
   - draw.io（macOS）：`which drawio || which draw.io || ls "/Applications/draw.io.app/Contents/MacOS/draw.io"`
     - 只命中 .app 内路径时（macOS 桌面版默认不进 PATH），导出命令用该完整路径代替 `drawio`
   - draw.io（Linux）：`which drawio || which draw.io || which drawio-desktop`
   - draw.io（Windows）：`where draw.io`
   - Mermaid：`which mmdc`
3. 检测命中时执行导出，只执行一次：
   - draw.io PNG：`<drawio> -x -f png -s 2 -b 10 --transparent -o 同名.png 文件.drawio`
   - draw.io SVG：`<drawio> -x -f svg -o 同名.svg 文件.drawio`
   - draw.io PDF：`<drawio> -x -f pdf -o 同名.pdf 文件.drawio`
   - draw.io 多页图：加 `-p 页码`，默认第 1 页
   - Mermaid：`mmdc -i 文件.mmd -o 同名.png -b transparent`（SVG/PDF 改 `-o` 的扩展名）
   - 参数说明：`-f` 输出格式；`-s` 分辨率倍数，PNG 取 2；`-b` 四周留白像素；`--transparent` 透明背景，仅 PNG 有效，版本不支持时去掉
4. 未检测到工具时给手动导出说明，不重试：
   - draw.io：在 app.diagrams.net 打开 .drawio → File → Export as → PNG/SVG/PDF
   - Mermaid：在 mermaid.live 粘贴内容 → Actions → PNG/SVG
5. 导出文件与源文件同目录、同名、仅扩展名不同；告知路径和格式。

不主动导出；是否需要图片只看用户原话。