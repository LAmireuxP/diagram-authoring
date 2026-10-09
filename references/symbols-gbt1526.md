# GB/T 1526-1989 符号规范（流程图）

等同采用 ISO 5807:1985，适用于数据流程图、程序流程图、系统流程图、程序网络图、系统资源图。流程图必须按语义使用下列符号；全图用矩形是最常见的错误。

表中「draw.io 样式」列给出 draw.io 渲染写法，其他渲染器的映射见 `format-mermaid.md`。

## 一、符号对照表

| 符号 | 名称 | 何时用 | 何时不用 | draw.io 样式 |
|---|---|---|---|---|
| 圆角矩形 | 端点符（起止） | 流程唯一入口/出口 | 中间步骤 | `rounded=1;arcSize=50;whiteSpace=wrap;html=1;fillColor=#D5E8D4;strokeColor=#82B366;` |
| 矩形 | 处理 | 一个执行步骤（动词+名词） | 判断、输入输出 | `rounded=0;whiteSpace=wrap;html=1;fillColor=#DAE8FC;strokeColor=#6C8EBF;` |
| 菱形 | 判断 | 一进多出的分支点 | 并发 | `rhombus;perimeter=rhombusPerimeter;whiteSpace=wrap;html=1;fillColor=#FFF2CC;strokeColor=#D6B656;` |
| 平行四边形 | 数据（输入/输出） | 数据录入、展示 | 计算、判断 | `shape=parallelogram;perimeter=parallelogramPerimeter;fixedSize=1;whiteSpace=wrap;html=1;fillColor=#DAE8FC;strokeColor=#6C8EBF;` |
| 波浪底矩形 | 文档 | 产生单据、报表 | 普通处理 | `shape=document;whiteSpace=wrap;html=1;boundedLbl=1;fillColor=#FFF2CC;strokeColor=#D6B656;` |
| 双竖线矩形 | 既定处理 | 调用已定义的子流程 | 内联步骤 | `shape=process;whiteSpace=wrap;html=1;backgroundOutline=1;fillColor=#DAE8FC;strokeColor=#6C8EBF;` |
| 六边形 | 准备 | 初始化计数器、参数 | 普通处理 | `shape=hexagon;perimeter=hexagonPerimeter2;whiteSpace=wrap;html=1;fixedSize=1;fillColor=#E1D5E7;strokeColor=#9673A6;` |
| 小圆圈 | 连接符 | 流线断点、同页跳转 | 起止 | `ellipse;whiteSpace=wrap;html=1;fillColor=#F5F5F5;strokeColor=#666666;`（40×40） |
| 五边形 | 页外连接符 | 跨页跳转 | 同页 | `shape=offPageConnector;whiteSpace=wrap;html=1;boundedLbl=1;` |
| 上下两条横线 | 并行方式 | 并发开始/结束 | 判断 | `shape=partialRectangle;top=1;bottom=1;left=0;right=0;fillColor=none;html=1;whiteSpace=wrap;` |

## 二、流线

1. 流向默认自上而下、从左到右；非默认方向必须画箭头。
2. 从顶部或左边进入，从底部或右边离开，对准中心：
   - 上方进入 `entryX=0.5;entryY=0;`
   - 下方离开 `exitX=0.5;exitY=1;`
   - 左边进入 `entryX=0;entryY=0.5;`
   - 右边离开 `exitX=1;exitY=0.5;`
3. 判断节点一进多出，每条出线标条件（是/否、Y/N、具体值）。
4. 尽量不交叉；无法避免时直接交叉，或加 `jumpStyle=arc;` 画跳线弧。
5. 长线用连接符（小圆圈内标字母或数字）截断。

## 三、节点文本

- 处理节点写“动词+名词”（如“校验凭据”“写入日志”），不写光杆名词。
- 文本从左到右、自上而下书写，与流向无关。
- 文本过多时用注解符（`shape=note;dashed=1;`）引出。
- 各符号分配空间均匀，尺寸尽量统一。

## 四、推荐尺寸（对齐 10px 网格）

| 符号 | 宽×高 |
|---|---|
| 端点符 | 120×60 |
| 处理 | 160×60 |
| 判断 | 160×80 |
| 数据（输入/输出） | 160×60 |
| 既定处理 | 160×60 |
| 准备 | 140×60 |
| 连接符 | 40×40 |
| 泳道 | 280×600（横向） |

## 五、判断分支布局

```text
            [判断]
           /      \
      是  /          \  否
         v            v
    [主路径]      [分支节点]
         |              |
         v              v
    [主路径]      [分支判断]
```

- 主干居中向下（exitX=0.5, exitY=1）
- 分支从判断框左右两侧引出（exitX=0 / exitX=1），条件标签白底（`labelBackgroundColor=#FFFFFF;`）
- 分支节点与主干水平间距 ≥ 200px

## 六、常见错误

1. 起止、输入输出、判断一律画成矩形 → 改用端点符、平行四边形、菱形
2. 判断出线不带条件 → 每条出线标“是/否”
3. 用判断表示并发 → 改用“并行方式”符号
4. 大量流线交叉不做处理 → 用连接符断线或跳线弧
5. 文字溢出节点 → 加 `whiteSpace=wrap;html=1;` 或改用注解符
6. 处理节点只写名词 → 改为动词+名词