# 设计系统驱动文件 (design.md) 编写指南

## 📌 背景：解决 AI 界面生成的“一致性危机”
在过去的 AI 界面生成工具（如 v0, Claude Artifacts）中，存在一个被行业广泛忽视的问题——**“一致性危机 (The Consistency Problem)”**。
你让 AI 生成第一个页面，很好看；但生成第二个页面时，按钮圆角变了，主色调偏差了，字体排版比例也不对。因为**早期的 Generative UI 是“无状态（Stateless）”的**。

近期，Google Stitch 等新一代引擎引入了 **`design.md` (Design Markdown)** 的概念，同时也提出了 **“Vibe Design（氛围设计）”** 的新范式。

`design.md` 是一份结构化的 Markdown 文档，本质上它是**“写给 AI 机器阅读的设计系统 (A Machine-Readable Design System)”**。只要我们将它放置在项目仓库中，或者在每次调用大模型生成 UI 时作为 Context（上下文）喂给它，AI 就能保持项目全局视觉的绝对一致性。

---

## 🛠️ design.md 的核心解构（基于业界实践沉淀）

根据我们对全网工具的调研和 Stitch 引擎的特性，一份优秀的、能让大模型完全理解的 `design.md`，应该包含以下 5 个核心维度：

### 1. 品牌气质与视觉隐喻 (Tone, Vibe & Metaphors)
*   **用途:** 设定全局的视觉基调（即 Stitch 强调的 Vibe Design）。告诉 AI 这是什么风格。
*   **范例:** "我们的产品是一款面向专业数据分析师的工具。视觉必须严谨、克制、冷淡、高对比度。禁止使用活泼可爱的阴影和大量圆角。参考终端命令行或极简主义建筑。"

### 2. 色彩语义系统 (Color Tokens & Usage)
*   **用途:** 不要只给色值，要给**语义（Semantic）**和使用场景，否则 AI 会乱用颜色。
*   **范例:**
    *   `Primary (品牌主色)`: #0F172A (Slate 900) - 用于最重要的行动按钮（CTA）和核心链接。
    *   `Surface (背景色)`: #F8FAFC (Slate 50) - 用于整个应用的大背景。
    *   `Danger (破坏性颜色)`: #EF4444 (Red 500) - 仅限于“删除”、“解散”等不可逆操作。

### 3. 排版比例与层级 (Typography Scale & Fonts)
*   **用途:** 控制文字的律动和可读性。AI 在处理排版时最容易放飞自我，必须严格框定比例。
*   **范例:**
    *   `Font Family`: 主标题使用 "Inter"，正文使用系统默认无衬线字体。
    *   `Heading 1 (页面大标题)`: 32px / Bold / 紧凑的行高。
    *   `Body (正文)`: 14px / Regular / 宽松的行高（如 1.6）以提升长文阅读体验。

### 4. 几何与空间规范 (Geometry & Spacing Rules)
*   **用途:** 控制边框、圆角、组件内外的留白节奏（比如 4px 还是 8px 网格）。
*   **范例:**
    *   `Radius (圆角)`: 卡片使用 12px，按钮使用 8px，输入框使用 6px。（禁止使用全圆角/胶囊按钮）。
    *   `Spacing (间距)`: 严格遵循 8pt (8px) 网格系统。模块之间间距 24px，组件内部间距 8px 或 16px。

### 5. 组件变体与交互状态 (Component Variants & States)
*   **用途:** 给 AI 补充悬停、按下、禁用等状态的规范，确保生成的代码具有完整的交互样式。
*   **范例:**
    *   `Hover State`: 所有卡片悬停时增加 Y 轴偏移的浅色投影（0 4px 6px -1px rgb(0 0 0 / 0.1)）。
    *   `Disabled State`: 降低 50% 透明度，光标变为 not-allowed。

---

## 🚀 工作流应用：如何使用 design.md？

1. **AI 前期推演：** 拿一张你喜欢的 Dribbble 设计图，或者告诉语言模型你想要的 Vibe（氛围），让 LLM 为你推演出一份初版的 `design.md`。
2. **作为系统资产入库：** 将这份 `.md` 文件提交到你的 Git 仓库，就像一份传统的 Design Spec。
3. **全局渲染引擎：** 在后续任何一次调用 Stitch 或 v0.dev 生成具体页面时，**强制附带这份 `design.md` 作为前置 Context**。这样，即使生成一千个页面，它们看起来也出自同一个高级设计师之手。