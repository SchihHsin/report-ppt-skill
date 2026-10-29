# 图表选型

## 图本身承载逻辑

解释复杂关系、内容结构或变化过程时，图本身要承载主要逻辑：**用节点、连线和区域组织复杂关系，让信息、内容和关系一眼可见。**

- **节点**承载具体对象、动作、状态或阶段；节点标题要能独立说明“这是什么 / 正在做什么”。
- **连线**表达有依据的关系、先后、依赖或流向；连线应连接明确的来源与目标，不为了视觉完整而补无意义的线。
- **区域**表达阶段、分组、层级或责任边界；用空间组织内容，避免再把相同分组重复写成一排小标题或说明卡。
- 先让读者能从图上读懂“有哪些内容、它们如何关联、过程如何变化”，再用短文字补充必要解释。不要把图做成装饰背景，再把关键关系散落在平铺文字里。
- 优先在一张主图内说明完整关系；只有当内容确实无法在同一视野中清楚阅读时，才拆成相互衔接的图，并保留阶段或路径线索。

## 原则：手画优先，但"能画 ≠ 好看"

1. **能纯 CSS/SVG 手画的优先手画**——自包含、最贴主题、最轻。
2. 但**只有精心调过的才好看**。从这套 deck 实战看，**唯一精调确认好看、可放心当模板复用的只有两个**：
   - **① 胶囊指标条**（数据洞察页）：渐变胶囊 + 超大百分比数字。模板已含。
   - **② 甘特浮动条**：白底玻璃大卡 + 浮动条 + 虚线网格底纹。组件骨架见 `components.md`。
3. **其它手画件只算"能显示"、未精调、不算好看**（星评★、进度/技能条 track+fill、SVG 情绪曲线、KPI 数字、conic 环形）——正式用前必须重新打磨，别当现成件直接抄。

## 何时上图表库（手画成本太高）

雷达 / 桑基 / 热力 / 关系树 / 大量散点 / 精细交互多系列 —— 用库：

- **ApexCharts**（~540KB，MIT）：基础图省心、默认就好看。
- **ECharts**（~1MB，Apache-2.0）：最全，复杂图首选；可调成 shadcn 风格（细网格虚线、roundRect 图例、圆角柱）。

两库"好看"基本打平，按**体积 / 图型**选。下载内联到 `lib/` 可离线。配色一律走渐变 token（用 `echarts.graphic.LinearGradient` / Apex `fill.gradient`）。`textStyle.fontFamily` 设成 `'HarmonyOS Sans SC, Inter, "Noto Sans SC", sans-serif'`。

ECharts 雷达最小示例（双系列对比）：
```js
echarts.init(el).setOption({
  textStyle:{fontFamily:'HarmonyOS Sans SC, Inter, "Noto Sans SC", sans-serif'},
  color:['#3B5BDB','#E8533B'],
  radar:{radius:'68%', indicator:[{name:'维度A'},{name:'维度B'},/*…*/],
    axisName:{color:'#5e646e',fontSize:10}, splitLine:{lineStyle:{color:'#d4d8df'}}},
  series:[{type:'radar',symbol:'none',data:[
    {value:[95,70,/*…*/],name:'组一',areaStyle:{color:'rgba(59,91,219,.1)'}},
    {value:[88,32,/*…*/],name:'组二',areaStyle:{color:'rgba(232,83,59,.1)'}}]}]
});
```

> 合并到单 deck 时，库 `<script src="lib/echarts.min.js">` 放在主脚本前；图表 init 包在 IIFE 里并 `if(typeof echarts==='undefined')return;` + `if(!el)return;` 保护，避免其它页报错。
