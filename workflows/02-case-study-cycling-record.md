# 案例研究 01：专业骑行记录页 (Intent-Driven Handoff 进阶版)

**原始需求：** 做一个展示单次骑行记录的页面。包含详尽的专业数据（功率、心率、踏频、TSS等），并需要支持心率和功率的可视化图表。

**🚫 错误示范（传统大模型会这样写 Wireframe Prompt）：**
> “页面顶部放一个带返回键的导航栏，标题是‘骑行记录’，右上角放分享和更多图标。下面放四个 Tab：概览、详情、图表、计圈。
> 在‘详情’Tab里，向下排布列表，第一组是计时数据，左边写‘总时长’右边写‘04:54’，第二组是速度，第三组是心率... 在‘图表’Tab里，画一个横坐标是时间，纵坐标是心率的折线图，下面再画一个柱状图表示功率...”
> *(评价：彻底把大模型变成了“绘图指令复读机”。如果强制要求画折线图，设计引擎就无法探索“热力图”、“区间分布图”等更专业的可视化方案。)*

---

**✅ 正确示范（意图驱动的交付物 Intent Handoff Document）：**

我们将根据真实业务场景提取出**“深度的结构化专业数据”**，并赋予设计引擎（如 Stitch）针对图表可视化的**自由探索权**。

```markdown
# 意图交付书: 顽鹿专业骑行记录与分析页 (Onelap Pro Ride Analysis)

## 1. 核心目标 (User Goal)
面向**严肃训练者和竞技玩家**的单次骑行深度复盘。
- **浅层诉求 (Overview):** 快速确认整体运动量（总里程、路线轨迹）、分享至社交媒体。
- **深层诉求 (Deep Dive):** 审视训练质量与身体负荷，通过拆解高阶生理指标（功率、心率、TSS）以及数据可视化，评估本次训练是否达到预期刺激，或是否存在过度疲劳风险。

## 2. 数据与信息结构 (Data Schema)
页面需要具备多维度的空间（如 Tabs 或区块平滑过渡）来承载以下海量信息。请 UI 引擎自由决定信息架构的容器（如使用分组卡片、折叠面板或网格）。

### 2.1 全局元数据 (Global Meta)
- **用户身份:** 头像、自定义标题 (如：修仙骑均速20)
- **时空背景:** 骑行时间 (如：2025/11/02 06:27)、地理位置、运动类型 (如：户外骑行)
- **绝对核心指标 (Hero Metric):** 总里程 (如：76.45 千米)

### 2.2 轨迹与总览模块 (Overview Segment)
- **视觉核心:** GPS 轨迹地图，允许叠加速度/心率热力渐变色。
- **智能分析入口:** “Onelap AI” 一键深度分析入口 (带 Beta 标识)。
- **核心数据聚合 (Quick Stats):** 最大速度、骑行时长、消耗卡路里、平均速度、总时长、累计爬升。

### 2.3 专业数据列表模块 (Categorized Detailed Metrics)
必须按生理学与物理学逻辑严谨分组，允许附带帮助图标 (Tooltips) 与计算状态角标 (如“含零”、“不含零”)：
- **计时 (Time):** 总时长、骑行时长、骑行/平路/爬坡占比分布。
- **速度 (Speed):** 平均速度、最大速度、平路巡航速度。
- **心率 (Heart Rate):** 个人乳酸阈值心率(LTHR)参考值、平均心率、最大心率。
- **功率 (Power):** 平均功率(AP)、标准化功率(NP)、最大功率、最大平均功率(20分钟)、功体比(W/kg)。
- **训练负荷 (Training Stress):** 强度因子(IF)、训练压力(TSS)、效率系数(EF)、变化量指数(VI)、做功(千焦)。
- **其他:** 踏频 (Avg/Max/总圈数)、海拔 (累计爬升/下降/最小海拔)。

### 2.4 深度可视化模块 (Data Visualization Charts)
需要引擎探索最具专业感的数据可视化组件，呈现随时间或里程变化的趋势：
- **心率趋势分析 (Heart Rate Chart):** 展示心率曲线，并建议通过颜色区分不同的“心率区间 (Zones)”。
- **功率分布分析 (Power Chart):** 展示功率输出的稳定性，或功率在不同区间的分布比例。

## 3. 信息层级与优先级 (Information Hierarchy)
**P0 (情绪感知与成就基石):** 总里程数字（必须是全场最大字号）、GPS 轨迹图。
**P1 (训练质量评估):** 功率图表、心率图表、高阶训练指标（NP, TSS, IF）。这是严肃玩家的核心痛点，不能淹没在普通数据中。
**P2 (罗列型数据):** 详细的次级数据列表，需保持高密度的专业阅读体验，切忌为了排版而降低信息密度。

## 4. 情感与氛围基调 (Vibe & Tone)
- **极客、专业、数据驱动。**
- 让用户感觉自己像是在看一份**“赛车遥测数据报告 (Telemetry Data)”**。
- **自由度授权:** 鼓励引擎在图表部分使用极具科幻感的渐变填充（如心率区域的红橙黄渐变），在数据列表部分采用紧凑的等宽字体排版 (tabular-nums)。

## 5. 挂载设计系统 (Attach Design System)
> 强制应用已沉淀的设计资产：[`assets/design-systems/onelap-design.md`](../assets/design-systems/onelap-design.md)
必须严格遵循：
*   深色背景 (`#0A1011`)
*   无边框的紧凑数据网格或极小圆角 (`Radius-MD`) 的分组卡片
*   `color-primary` (`#C6FF00`) 作为关键状态高亮
```

## 6. 测试结论与工程踩坑记录
在实际向 Stitch 发起带有“完整意图 + Design.md”的长文本生成请求时，我们确认了以下两点：

1. **工程痛点：网关超时假象**
   调用 Stitch API 时多次返回 `MCP error -32001: Request timed out`。但这其实是一个**“网关超时假象”**。
   底层模型处理大量“排版+数据+Vibe推理”时耗时超过了 60 秒，导致客户端 HTTP 连接断开。**然而，Stitch 后台实际上一直在默默渲染，且最终成功落盘了**（通过 `stitch_list_screens` 接口查询，发现后台其实已经成功生成了 `Professional Cycling Analysis Report` 等多个屏幕资产）。
   *结论：如果在真实环境接入此类工具，必须采用“异步轮询 (Polling)”或者“Webhook 回调”的方式获取结果，绝对不能依赖同步等待。*

2. **效果预期：代码级高保真还原**
   基于纯意图交付，Stitch / 代码引擎能够直接越过传统线框图阶段，输出结构极佳的 UI。我们在本仓库的 `demos/CyclingRecord.tsx` 中用 React 手工验证了这套 Prompt 的效力：即使不干涉任何布局，只要控制好 `tabular-nums` 和 `Vibe Design`，页面自动就具备了专业竞技感。

3. **意图克制原则 (Intent Restraint)**
   我们进行了两组对比测试：
   - **A 组（详细版）：** 在意图中塞入了完整的 6 大类数据字段（计时、速度、心率、功率、训练负荷、踏频/海拔），以及精确的数值示例。结果 Stitch 生成的页面高度达到 5000+ px，排版非常混乱，所有信息堆砌在一起，缺乏视觉焦点。
   - **B 组（精简版）：** 只保留了一个 Hero Metric（总里程 76.45km）+ 一组次要指标网格 + 两张图表的意图描述。结果 Stitch 生成的页面更加聚焦、层次分明，设计感大幅提升。
   
   **结论：Intent Document 也需要"克制"。** 交给生成引擎的意图不是"越多越详细越好"，而是要像一份好的 PRD 一样，**只保留核心骨架和最高优先级的字段**。次要信息应该由引擎自主填充，而不是一次性全部塞给它，否则引擎的注意力会被分散，导致排版混乱。这进一步验证了我们在 `01-intent-driven-handoff.md` 中提出的核心观点：**把排版权还给设计工具。**

---

## 7. 附录：A/B 版提示词对比

### A 版提示词（详细版 - 导致排版混乱）
```
Generate a professional cycling record analysis page.

[DESIGN SYSTEM CONTEXT (Vibe & Rules)]
- Tone: Hardcore, competitive, telemetry data report. Carbon fiber, neon accents.
- Background: #0A1011 (Dark mode only).
- Text: Primary #FFFFFF, Secondary rgba(255,255,255,0.6).
- Primary Action/Highlight Color: #C6FF00 (Neon Green).
- Radius: Max 12px for cards. No pill shapes.
- Typography: Use tabular-nums for all metrics. Display size 80px for hero numbers, compact lines.

[INTENT & DATA STRUCTURE]
- Overview section: GPS track map with heat gradient, User avatar, Ride title (e.g., "Morning Ride"), Date/Time, and the Hero Metric: Total Distance (e.g., 76.45 km) in huge tabular font.
- Quick Stats below map: Max Speed, Duration, Calories, Avg Speed.
- Categorized Detailed Metrics (Use grids or tight lists to keep density high):
  - Time: Total Time, Ride Time.
  - Speed: Avg Speed, Max Speed.
  - Heart Rate: Avg HR (e.g., 126 bpm), Max HR.
  - Power: Avg Power (AP, 75W), Normalized Power (NP, 84W), Max Power (438W), W/kg (1.1).
  - Training Stress: IF (0.71), TSS (197.5), Work (1057 kJ).
- Visualizations: Include a Heart Rate Zone Chart and a Power Distribution Graph. Let the engine explore the best chart type (heatmaps, zone bars, etc.). Use #F14867 (Danger red) for high HR zones.
```

### B 版提示词（精简版 - 效果更好）
```
A single cycling ride summary page for a hardcore training app called Onelap.

The page should celebrate the rider's achievement with one hero number: total distance (76.45 km), displayed prominently at the top with a GPS route map below it.

Below that, show a compact grid of secondary stats: duration, avg speed, calories, elevation gain, avg heart rate, and avg power.

At the bottom, include two data visualizations: a heart rate zone chart and a power output chart. Use color to distinguish intensity zones.

Design rules: Dark mode only (#0A1011 background). Neon green accent (#C6FF00) for highlights and CTAs. Small border radius (max 12px). No rounded pill shapes. Professional, data-dense, telemetry-like aesthetic. All numbers should use monospace/tabular formatting.
```

### 关键差异分析
| 维度 | A 版（详细） | B 版（精简） |
|------|------------|------------|
| 字段数量 | 明确列出了 15+ 个具体字段，带数值示例 | 只列出 6 个次要指标名称，无具体数值 |
| 数据分类 | 分成 5 大类（Time/Speed/HR/Power/Stress） | 不分类，统称为 "secondary stats" |
| 图表描述 | 指定图表类型、颜色、热力图建议 | 仅说 "two data visualizations" |
| 字符数 | ~800 字符 | ~500 字符 |
| 生成结果 | 高度 5650px，信息堆砌混乱 | 高度 4366px，层次分明 |