# draw.io 渲染（.drawio XML）

写 .drawio 前通读第 1、2、5 节。可对照 `examples/` 下按本规范生成的文件。

## 1. 最小模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mxfile host="app.diagrams.net">
  <diagram id="page-1" name="Page-1">
    <mxGraphModel dx="0" dy="0" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="850" pageHeight="1100" math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- 节点与连线写在这里 -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

关键规则：
- `id="0"`（根容器）与 `id="1"`（默认图层）必须存在
- 其余元素 `parent="1"` 或容器 id
- id 唯一，推荐语义化
- `vertex="1"` 与 `edge="1"` 互斥
- 坐标原点在左上，x 右增，y 下增
- style 以 `;` 结尾，键值区分大小写，布尔用 0/1
- 禁止 `compressed="true"`

## 2. 节点与连线

### 节点

```xml
<mxCell id="n1" value="文本" style="rounded=0;whiteSpace=wrap;html=1;fillColor=#DAE8FC;strokeColor=#6C8EBF;" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="160" height="60" as="geometry"/>
</mxCell>
```

### 连线

```xml
<mxCell id="e1" value="是" style="edgeStyle=orthogonalEdgeStyle;rounded=1;html=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;labelBackgroundColor=#FFFFFF;" edge="1" parent="1" source="n1" target="n2">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### 锚点速查（0~1 相对比例，exit 与 entry 同理）

| 位置 | exitX | exitY |
|---|---|---|
| 上 | 0.5 | 0 |
| 下 | 0.5 | 1 |
| 左 | 0 | 0.5 |
| 右 | 1 | 0.5 |
| 中心 | 0.5 | 0.5 |

### 连线路由样式

| 需求 | style |
|---|---|
| 正交折线（流程图推荐） | `edgeStyle=orthogonalEdgeStyle;rounded=1;` |
| 直线 | 不加 edgeStyle 或 `edgeStyle=none;` |
| ER 图连线 | `edgeStyle=entityRelationEdgeStyle;` |
| 虚线（回退/异常） | `dashed=1;` |
| 跳线弧（交叉美化） | `jumpStyle=arc;` |
| 双向箭头 | `startArrow=classic;endArrow=classic;` |
| 无箭头 | `endArrow=none;` |
| 加粗 | `strokeWidth=2;` |

### 连线中间点（waypoints）

```xml
<mxGeometry relative="1" as="geometry">
  <Array as="points">
    <mxPoint x="300" y="150"/>
    <mxPoint x="300" y="250"/>
  </Array>
</mxGeometry>
```

## 3. 形状名速查（区分大小写）

### 核心形状

| shape | 说明 | 必配 perimeter |
|---|---|---|
| `rectangle` | 矩形（默认） | — |
| `ellipse` | 椭圆 | 自动 |
| `rhombus` | 菱形 | `rhombusPerimeter` |
| `hexagon` | 六边形 | `hexagonPerimeter2` |
| `cylinder` / `cylinder3` | 圆柱（数据库） | — |
| `cloud` | 云 | — |
| `actor` / `umlActor` | 角色小人 | — |
| `swimlane` | 泳道/容器 | — |

### 扩展形状（流程图）

| shape | 说明 |
|---|---|
| `parallelogram` | 平行四边形（配 `parallelogramPerimeter`） |
| `document` | 文档（波浪底） |
| `process` | 既定处理（双竖线） |
| `manualInput` | 手动输入（倒梯形） |
| `internalStorage` | 内部存储 |
| `dataStorage` | 数据存储 |
| `loopLimit` | 循环界限 |
| `offPageConnector` | 页外连接符 |
| `delay` | 延迟（D 形） |
| `display` | 显示设备 |
| `partialRectangle` | 部分边框（并行方式：`top=1;bottom=1;left=0;right=0;`） |
| `or` / `xor` | 或门/异或门 |
| `note` | 便签 |
| `card` | 卡片（切角） |
| `datastore` | 数据存储（圆柱） |
| `callout` | 标注气泡 |

### UML 形状

`umlLifeline`、`startState`、`endState`、`umlFrame`、`component`、`module` 等。

### 模板库

格式 `shape=mxgraph.<库>.<形状名>`：

- `mxgraph.flowchart.*` — 流程图
- `mxgraph.bpmn.*` — BPMN
- `mxgraph.aws4.*` / `mxgraph.azure.*` / `mxgraph.gcp2.*` — 云架构
- `mxgraph.cisco19.*` — 网络设备
- `mxgraph.kubernetes.*` — K8s
- `mxgraph.er.*` — ER 图
- `mxgraph.uml.*` — UML

## 4. 样式属性速查

### 颜色

| 颜色 | fill | stroke | 用途 |
|---|---|---|---|
| 蓝 | `#DAE8FC` | `#6C8EBF` | 处理节点（默认） |
| 绿 | `#D5E8D4` | `#82B366` | 起止、成功路径 |
| 黄 | `#FFF2CC` | `#D6B656` | 判断节点 |
| 橙 | `#FFE6CC` | `#D79B00` | 警告、计数 |
| 红 | `#F8CECC` | `#B85450` | 错误、异常终止 |
| 紫 | `#E1D5E7` | `#9673A6` | 外部系统、准备 |
| 灰 | `#F5F5F5` | `#666666` | 背景、注释 |

### 文本

- 字号 `fontSize=14;`（默认 12）
- 粗、斜 `fontStyle=1;`（1 粗、2 斜、3 粗斜、4 下划线）
- 左对齐 `align=left;spacingLeft=8;`
- 顶对齐 `verticalAlign=top;spacingTop=4;`
- 多行 `<br>`（配合 html=1）
- 连线标签白底 `labelBackgroundColor=#FFFFFF;`

### 几何

- 圆角 `rounded=1;`；圆角大小 `arcSize=50;`（0–50）
- 固定比例 `aspect=fixed;`
- 旋转 `rotation=90;`
- 方向 `direction=north;`

### 箭头

| 需求 | 属性 |
|---|---|
| 实心三角 | `endArrow=classic;`（默认） |
| 空心三角 | `endArrow=open;` |
| 空心菱形（聚合） | `endArrow=diamond;endFill=0;` |
| 实心菱形（组合） | `endArrow=diamond;endFill=1;` |
| Crow's foot 一 | `endArrow=ERone;` |
| Crow's foot 多 | `endArrow=ERmany;` |
| Crow's foot 零或多 | `endArrow=ERzeroToMany;` |
| Crow's foot 零或一 | `endArrow=ERzeroToOne;` |
| 箭头大小 | `endSize=8;` |

## 5. 泳道

```xml
<!-- 横向泳道：标题在左（horizontal=0），标题栏占左侧宽度 30（startSize=30） -->
<mxCell id="lane_user" value="用户" style="swimlane;html=1;horizontal=0;startSize=30;fillColor=#DAE8FC;" vertex="1" parent="1">
  <mxGeometry x="40" y="80" width="280" height="600" as="geometry"/>
</mxCell>

<!-- 泳道内节点：parent=lane_user，坐标为相对坐标，x ≥ startSize(30)，避免压在标题栏上 -->
<mxCell id="n_login" value="登录" style="rounded=0;whiteSpace=wrap;html=1;" vertex="1" parent="lane_user">
  <mxGeometry x="70" y="60" width="160" height="60" as="geometry"/>
</mxCell>
```

- 横向泳道：`horizontal=0;`（标题在左，占左侧宽度 `startSize`，流程自左向右）→ 子元素 x ≥ startSize
- 纵向泳道（默认）：`horizontal=1;`（标题在上，占顶部高度 `startSize`，流程自上而下）→ 子元素 y ≥ startSize
- 隐藏容器边框：`style="group;"` 或加 `container=1;`

## 6. HTML 标签（html=1 时 value 可用）

支持 `<b> <i> <br> <font> <hr> <ul> <li> <img> <a>`，必须转义：`<`→`&lt;`、`>`→`&gt;`、`&`→`&amp;`、`"`→`&quot;`。

UML 类框模板：

```xml
value="<p style='margin:0;margin-top:4px;text-align:center;'><b>ClassName</b></p><hr size='1'/><p style='margin:0;margin-left:4px;'>+ field: Type</p><hr size='1'/><p style='margin:0;margin-left:4px;'>+ method(): void</p>"
```

配合 `verticalAlign=top;align=left;overflow=fill;html=1;whiteSpace=wrap;`。

## 7. 多页面

一个 `<mxfile>` 可含多个 `<diagram>`（每页一个），`id` 与 `name` 各自唯一。

## 8. 错误示例与正确写法

### value 未转义

错误：

```xml
<mxCell id="n1" value="<b>登录</b>" .../>
```

正确：

```xml
<mxCell id="n1" value="&lt;b&gt;登录&lt;/b&gt;" .../>
```

### 菱形缺 perimeter，连线从角上出

错误：

```xml
<mxCell id="check" style="rhombus;whiteSpace=wrap;html=1;" .../>
```

正确：

```xml
<mxCell id="check" style="rhombus;perimeter=rhombusPerimeter;whiteSpace=wrap;html=1;" .../>
```

### 泳道内节点压在标题栏上

错误（横向泳道 startSize=30，节点 x=10 压住左侧标题栏）：

```xml
<mxCell id="n1" parent="lane_user">
  <mxGeometry x="10" y="60" .../>
</mxCell>
```

正确：

```xml
<mxCell id="n1" parent="lane_user">
  <mxGeometry x="70" y="60" .../>  <!-- x ≥ startSize -->
</mxCell>
```

## 9. 常见问题排查

| 现象 | 原因 | 修复 |
|---|---|---|
| 文件打不开 | id=0/1 缺失、XML 未闭合、value 未转义 | 检查骨架与转义 |
| 连线乱飞 | 缺锚点或 waypoints | 加 exit/entry 或 points |
| 泳道内元素错位 | parent 不是泳道 id，或坐标没按 startSize 偏移 | parent 指向泳道，横向泳道校 x、纵向泳道校 y |
| 菱形连线从角上出 | 缺 `perimeter=rhombusPerimeter;` | 补 perimeter |
| 文字溢出 | 缺 `whiteSpace=wrap;html=1;` | 补样式或改注解符 |

## 10. 自检清单

- [ ] 以 `<?xml version="1.0" encoding="UTF-8"?>` 开头
- [ ] `id="0"` 与 `id="1"` 存在，`id="1"` 的 parent 为 0
- [ ] id 全局唯一；`vertex="1"` 与 `edge="1"` 不共存
- [ ] 所有 `source`/`target`/`parent` 指向已存在的 id
- [ ] value 中 `<`、`>`、`&`、`"` 已转义
- [ ] 标签闭合，无 `compressed="true"`
- [ ] 菱形有 `perimeter=rhombusPerimeter;`，平行四边形有 `parallelogramPerimeter`
- [ ] 泳道内子元素相对坐标未压标题栏（横向校 x、纵向校 y）
- [ ] 坐标对齐 10px 网格，节点矩形无重叠