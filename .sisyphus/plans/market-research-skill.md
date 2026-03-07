# 市场调研 AI Skill 设计

## TL;DR

> **目标**：为顽鹿软件产品中心设计一个「市场调研」AI Skill，引导产品经理完成从需求分类到调研报告的全流程，核心亮点是 50-100 个 AI 模拟用户的 Kano 测算、深度访谈和功能投票能力。
>
> **交付物**：
> - Skill 完整定义文档（Prompt、交互流程、输入输出规范）
> - 模拟用户系统设计方案（画像数据结构、生成逻辑、存储方案）
> - 调研报告模板（宏观+微观两套完整模板）
> - 端到端示例（一次完整调研的演示）
>
> **预计工作量**：Medium
> **并行执行**：YES - 3 waves
> **关键路径**：Task 1 → Task 3 → Task 5 → Task 7 → Task 8

---

## Context

### 原始需求
为顽鹿软件产品中心的产品经理工作流中的「市场调研」阶段设计 AI Agent/Skill。基于已有的 `市场调研思路.md` 方法论，创建一个能交互式引导 PM 完成调研、辅助信息采集、并通过模拟用户系统进行虚拟验证的 Skill。

### 访谈摘要
**核心决策**：
- 产出形式：交互式引导 + 辅助信息采集 → 单个完整 Markdown 调研报告
- 使用环境：命令行 / AI 对话工具
- 联网能力：可选（默认不联网，按需开启搜索增强）
- 调研流程：先分类（全新产品→宏观 / 功能迭代→微观），再按维度逐步引导
- PM 可跳过已完成维度
- 两种调研模式（宏观+微观）都做

**模拟用户系统决策**：
- 用户画像库持久化存储，可跨调研复用
- 每次调研模拟 50-100 个角色
- 支持全套模拟：Kano 模型测算、场景化深度访谈、功能意向投票
- 画像由 AI 生成，数量要多

### Metis 审查
**已处理的差距**：
- 模拟用户存储方案：采用 JSON 文件持久化存储在仓库中
- 角色生成机制：混合模式（预生成基础画像库 + 调研时 AI 实时丰富细节）
- Kano 模型输入：基于模拟角色评分，PM 可人工调整
- Phase 边界：Phase 1 纯设计文档，不含代码实现
- 复杂度锁定：角色 ≤100，单一 Markdown 报告格式，纯文本交互

---

## 工作目标

### 核心目标
产出一套完整的「市场调研 AI Skill」设计文档，覆盖宏观和微观两种调研模式，包含模拟用户系统的完整设计方案，使后续开发团队可以直接据此实现。

### 具体交付物
- `市场调研Skill设计.md` — Skill 核心定义（Prompt、流程、规范）
- `模拟用户系统设计.md` — 用户画像库 + 模拟机制设计
- `调研报告模板.md` — 宏观+微观两套报告模板
- `市场调研Skill示例.md` — 端到端演示示例

### 完成标准
- [x] Skill Prompt/描述足够详细，可直接用于 AI 系统配置
- [x] 交互流程覆盖所有调研维度（宏观4维度 + 微观4维度）
- [x] 模拟用户数据结构定义完整（字段、类型、约束、JSON Schema）
- [x] 报告模板可直接填充使用
- [x] 包含至少一个端到端示例
- [x] 所有文档符合 AGENTS.md 中的内容规范（简体中文、Markdown 风格）

### 必须包含
- 需求分类逻辑（如何判断宏观 vs 微观）
- `市场调研思路.md` 中所有 8 个维度的引导流程
- 模拟用户画像库的数据结构和生成逻辑
- Kano 模型测算的输入/输出规范
- 场景化深度访谈的模拟机制
- 功能意向投票的统计和呈现方式
- 可选联网搜索的触发条件和基本流程

### 必须不包含（护栏）
- 不实现代码或自动化脚本（纯设计文档）
- 不设计外部 API 集成的技术细节
- 不涉及多租户/权限系统
- 不设计 Web UI 或可视化界面
- 不超出 `市场调研思路.md` 的方法论范围添加新框架（除非 PM 明确要求）
- 不假设 PM 熟悉所有方法论术语（必要时加入简要解释）

---

## 验证策略

> **零人工干预** — 所有验证由 Agent 执行

### 测试决策
- **基础设施**：无（纯文档仓库）
- **自动化测试**：无（设计文档无代码测试需求）
- **框架**：无

### QA 策略
每个任务完成后，Agent 需验证：
1. 文档内容覆盖了任务描述中列出的所有要点
2. 术语与 `市场调研思路.md` 和 `AGENTS.md` 保持一致
3. Markdown 格式符合仓库规范
4. 中文表达自然流畅，无机翻痕迹

---

## 执行策略

### 并行执行波次

```
Wave 1 (立即开始 — 基础设计，可并行):
├── Task 1: 需求分类逻辑与交互流程总览设计 [writing]
├── Task 2: 模拟用户画像库数据结构设计 [writing]
└── Task 3: 调研报告模板设计（宏观+微观） [writing]

Wave 2 (Wave 1 完成后 — 核心模块，可并行):
├── Task 4: 宏观调研 4 维度引导流程详细设计 (依赖: 1, 3) [writing]
├── Task 5: 微观调研 4 维度引导流程详细设计 (依赖: 1, 3) [writing]
└── Task 6: 模拟用户三大能力详细设计 (依赖: 2) [writing]

Wave 3 (Wave 2 完成后 — 整合与示例):
├── Task 7: Skill 核心定义文档整合 (依赖: 1, 4, 5, 6) [writing]
└── Task 8: 端到端示例编写 (依赖: 3, 6, 7) [writing]

Wave FINAL (所有任务完成后 — 审查):
├── Task F1: 方法论覆盖度审查 [deep]
├── Task F2: 文档质量与规范审查 [unspecified-high]
└── Task F3: 设计完整性与可实现性审查 [deep]

关键路径: Task 1 → Task 4/5 → Task 7 → Task 8 → F1-F3
并行加速: ~50% 快于串行
最大并发: 3 (Wave 1 & Wave 2)
```

### 依赖矩阵

| 任务 | 依赖 | 被依赖 | 波次 |
|------|------|--------|------|
| 1 | — | 4, 5, 7 | 1 |
| 2 | — | 6 | 1 |
| 3 | — | 4, 5, 8 | 1 |
| 4 | 1, 3 | 7 | 2 |
| 5 | 1, 3 | 7 | 2 |
| 6 | 2 | 7, 8 | 2 |
| 7 | 1, 4, 5, 6 | 8 | 3 |
| 8 | 3, 6, 7 | F1-F3 | 3 |

### Agent 分配摘要

- **Wave 1**: 3 任务 — T1 → `writing`, T2 → `writing`, T3 → `writing`
- **Wave 2**: 3 任务 — T4 → `writing`, T5 → `writing`, T6 → `writing`
- **Wave 3**: 2 任务 — T7 → `writing`, T8 → `writing`
- **FINAL**: 3 任务 — F1 → `deep`, F2 → `unspecified-high`, F3 → `deep`

---

## TODOs

### Wave 1 — 基础设计（可并行）

- [x] 1. 需求分类逻辑与交互流程总览设计

  **What to do**:
  - 设计需求分类决策树：PM 输入需求描述后，Skill 如何判断应走「宏观调研」还是「微观调研」
    - 定义分类关键词/特征（全新产品/新业务线 → 宏观；功能迭代/模块新增 → 微观）
    - 设计混合场景处理逻辑（如"新产品但已有类似业务"时如何判断）
    - PM 可手动覆盖自动分类结果
  - 设计完整交互流程图（文字流程图，非代码）：
    - 入口：PM 发起调研 → Skill 询问基本信息 → 需求分类 → 进入对应模式
    - 宏观模式：4 维度逐步引导（可跳过）→ 触发模拟用户系统 → 生成报告
    - 微观模式：4 维度逐步引导（可跳过）→ 触发模拟用户系统 → 生成报告
    - 每个维度的引导节点：Skill 提问 → PM 回答/提供资料 → Skill 总结确认 → 下一维度
  - 设计跳过机制：PM 如何告知已完成某维度、跳过后报告中如何标注
  - 设计可选联网搜索的触发条件（如 PM 说"帮我查一下"或 Skill 主动建议"是否需要联网查询 XX 数据"）
  - 产出文件：`市场调研Skill设计.md` 的「需求分类逻辑」和「交互流程总览」章节

  **Must NOT do**:
  - 不写代码或伪代码实现
  - 不设计 UI 界面
  - 不超出 `市场调研思路.md` 方法论范围

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 纯文档设计任务，需要结构化的中文写作能力
  - **Skills**: `[]`
    - 无需特殊技能，标准写作即可

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 2, 3)
  - **Blocks**: Tasks 4, 5, 7
  - **Blocked By**: None

  **References**:

  **Pattern References**:
  - `市场调研思路.md:4-8` — 宏观调研的核心目的和适用场景定义，用于构建分类决策树的「宏观」分支判断标准
  - `市场调研思路.md:42-46` — 微观调研的核心目的和适用场景定义，用于构建分类决策树的「微观」分支判断标准
  - `市场调研思路.md:76-79` — 宏观 vs 微观的总结对比（望远镜 vs 显微镜），作为分类逻辑的设计指导思想

  **API/Type References**:
  - `AGENTS.md:文档结构模式` — 文档应遵循的 Markdown 层级结构（`#` 文档标题、`###` 主要章节、`####` 子章节）

  **External References**:
  - 无需外部参考

  **Acceptance Criteria**:

  ```
  Scenario: 分类决策树完整性验证
    Tool: Bash (grep)
    Preconditions: 文件 市场调研Skill设计.md 已创建
    Steps:
      1. grep "宏观调研" 市场调研Skill设计.md — 确认包含宏观分类条件
      2. grep "微观调研" 市场调研Skill设计.md — 确认包含微观分类条件
      3. grep "跳过" 市场调研Skill设计.md — 确认包含跳过机制说明
      4. grep "联网" 市场调研Skill设计.md — 确认包含联网触发条件
    Expected Result: 所有 grep 均返回至少一行匹配结果
    Failure Indicators: 任何 grep 返回空结果
    Evidence: .sisyphus/evidence/task-1-classification-check.txt

  Scenario: 交互流程覆盖所有入口和分支
    Tool: Bash (read + 人工审查由 Agent 执行)
    Preconditions: 文件 市场调研Skill设计.md 已创建
    Steps:
      1. 读取「交互流程总览」章节
      2. 验证包含：入口节点、分类节点、宏观4维度节点、微观4维度节点、模拟用户触发节点、报告生成节点
      3. 验证包含：维度跳过的分支处理
    Expected Result: 流程图包含至少 12 个明确的流程节点（入口1 + 分类1 + 宏观4 + 微观4 + 模拟1 + 报告1）
    Failure Indicators: 缺少任何一个维度的引导节点
    Evidence: .sisyphus/evidence/task-1-flow-review.txt
  ```

  **Commit**: YES (groups with Wave 1)
  - Message: `添加市场调研Skill基础设计（流程、画像库、报告模板）`
  - Files: `市场调研Skill设计.md`

- [x] 2. 模拟用户画像库数据结构设计

  **What to do**:
  - 设计单个用户画像的完整字段定义（JSON Schema 格式描述）：
    - 基本信息：年龄、性别、城市（一线/二线/三线等）、职业、收入区间
    - 行为特征：技术接受度（高/中/低）、消费偏好、常用 APP 类型
    - 心理画像：核心痛点、消费决策因素、风险偏好
    - 场景标签：使用场景列表、行业标签
    - 元数据：创建时间、适用行业/产品类型、复用次数
  - 设计画像库的整体组织结构：
    - 文件存储方案（单个 JSON 文件 vs 按行业分文件）
    - 画像库索引/目录结构
    - 复用机制：如何按行业/场景筛选已有画像
  - 设计 AI 生成画像的策略：
    - 生成 Prompt 模板（输入行业+产品类型 → 输出 N 个多样化画像）
    - 多样性保证机制（年龄分布、地域分布、收入分布覆盖）
    - 每次调研 50-100 个角色的混合策略（复用已有 + 新生成补充）
  - 产出文件：`模拟用户系统设计.md` 的「画像数据结构」和「画像生成策略」章节

  **Must NOT do**:
  - 不写实际的 JSON Schema 代码文件（用 Markdown 中的代码块描述即可）
  - 不设计数据库表结构
  - 不实现画像生成脚本

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 数据结构设计文档，需要精确的结构化描述能力
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 1, 3)
  - **Blocks**: Task 6
  - **Blocked By**: None

  **References**:

  **Pattern References**:
  - `市场调研思路.md:30-32` — 用户画像（Persona）的定义和字段参考（年龄、地域、职业、收入等标签），用于设计画像库的基础字段
  - `市场调研思路.md:63-64` — Kano 模型的输入需求（需要基于具体角色做属性分类），画像字段需支持 Kano 测算
  - `.sisyphus/drafts/market-research-agent.md:30-41` — 草稿中已确认的画像库设计要点（持久化、50-100角色、三大模拟能力）

  **External References**:
  - 无需外部参考

  **Acceptance Criteria**:

  ```
  Scenario: 画像字段完整性验证
    Tool: Bash (grep)
    Preconditions: 文件 模拟用户系统设计.md 已创建
    Steps:
      1. grep "年龄" 模拟用户系统设计.md — 确认包含年龄字段
      2. grep "职业" 模拟用户系统设计.md — 确认包含职业字段
      3. grep "痛点" 模拟用户系统设计.md — 确认包含心理画像字段
      4. grep "JSON" 模拟用户系统设计.md — 确认使用 JSON 格式描述
      5. grep "多样性" 模拟用户系统设计.md — 确认有多样性保证机制
    Expected Result: 所有 grep 均返回至少一行匹配
    Failure Indicators: 任何关键字段缺失
    Evidence: .sisyphus/evidence/task-2-schema-check.txt

  Scenario: 画像示例验证
    Tool: Bash (read + Agent 审查)
    Preconditions: 文件 模拟用户系统设计.md 已创建
    Steps:
      1. 读取文档中的画像示例
      2. 验证示例包含所有定义的字段
      3. 验证示例数据合理性（年龄范围、城市真实性等）
    Expected Result: 至少包含 2 个完整的画像示例，所有字段均有合理取值
    Failure Indicators: 示例缺少字段或数据明显不合理
    Evidence: .sisyphus/evidence/task-2-example-review.txt
  ```

  **Commit**: YES (groups with Wave 1)
  - Message: `添加市场调研Skill基础设计（流程、画像库、报告模板）`
  - Files: `模拟用户系统设计.md`

- [x] 3. 调研报告模板设计（宏观+微观）

  **What to do**:
  - 设计宏观调研报告模板：
    - 报告封面/摘要区：调研主题、日期、PM 姓名、调研模式、使用的维度
    - 第一部分：行业与市场宏观环境（PEST 分析表格、市场规模数据）
    - 第二部分：市场竞争格局（竞品对比矩阵、竞争象限图文字描述）
    - 第三部分：目标用户大盘分析（用户画像汇总、核心痛点列表）
    - 第四部分：商业模式预判（盈利模式分析、成本结构估算）
    - 模拟用户验证区：Kano 测算结果、投票统计、关键访谈摘要
    - 结论与建议：Go/No-Go 推荐及依据
  - 设计微观调研报告模板：
    - 报告封面/摘要区：功能名称、所属产品、调研日期、PM 姓名
    - 第一部分：内部需求溯源与价值评估（数据漏斗分析、VOC 汇总、战略价值定位）
    - 第二部分：微观竞品功能级拆解（交互路径对比、跨界参考、舆情摘要）
    - 第三部分：现有用户圈层验证（Kano 结果表、访谈关键发现）
    - 第四部分：低成本试错与 MVP 测试方案（Fake Door 方案、可用性测试计划、A/B 方案）
    - 模拟用户验证区：Kano 测算结果、投票统计、关键访谈摘要
    - 结论与建议：功能优先级排序及实施建议
  - 设计"已跳过维度"的标注方式（如灰色区块 + "PM 已跳过此维度"提示）
  - 产出文件：`调研报告模板.md`

  **Must NOT do**:
  - 不填充实际数据（模板中使用 `[待填充]` 占位符）
  - 不设计报告导出/渲染功能
  - 不添加 `市场调研思路.md` 之外的分析框架

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 模板设计需要结构化排版和清晰的信息层级组织
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 1 (with Tasks 1, 2)
  - **Blocks**: Tasks 4, 5, 8
  - **Blocked By**: None

  **References**:

  **Pattern References**:
  - `市场调研思路.md:9-38` — 宏观调研的 4 个维度及其子项，逐一对应报告模板的 4 个部分
  - `市场调研思路.md:47-72` — 微观调研的 4 个维度及其子项，逐一对应报告模板的 4 个部分
  - `AGENTS.md:文档结构模式` — 文档 Markdown 层级风格（`#`/`###`/`####`）
  - `AGENTS.md:Markdown 风格` — 列表使用 `*`，分隔线使用 `---`

  **External References**:
  - 无需外部参考

  **Acceptance Criteria**:

  ```
  Scenario: 宏观模板维度覆盖验证
    Tool: Bash (grep)
    Preconditions: 文件 调研报告模板.md 已创建
    Steps:
      1. grep "行业与市场宏观环境" 调研报告模板.md
      2. grep "市场竞争格局" 调研报告模板.md
      3. grep "目标用户大盘" 调研报告模板.md
      4. grep "商业模式预判" 调研报告模板.md
      5. grep "PEST" 调研报告模板.md
    Expected Result: 宏观调研的 4 个维度均在模板中出现
    Failure Indicators: 任何维度缺失
    Evidence: .sisyphus/evidence/task-3-macro-check.txt

  Scenario: 微观模板维度覆盖验证
    Tool: Bash (grep)
    Preconditions: 文件 调研报告模板.md 已创建
    Steps:
      1. grep "内部需求溯源" 调研报告模板.md
      2. grep "微观竞品" 调研报告模板.md
      3. grep "用户圈层验证" 调研报告模板.md
      4. grep "MVP" 调研报告模板.md
      5. grep "Kano" 调研报告模板.md
    Expected Result: 微观调研的 4 个维度均在模板中出现
    Failure Indicators: 任何维度缺失
    Evidence: .sisyphus/evidence/task-3-micro-check.txt

  Scenario: 模板可用性验证
    Tool: Bash (read + Agent 审查)
    Preconditions: 文件 调研报告模板.md 已创建
    Steps:
      1. 读取完整模板文件
      2. 验证包含占位符标记（如 [待填充]）
      3. 验证包含"已跳过维度"标注方式
      4. 验证模拟用户验证区在两套模板中均存在
    Expected Result: 模板结构完整，占位符清晰，可直接用于填充
    Failure Indicators: 缺少占位符或模拟用户验证区
    Evidence: .sisyphus/evidence/task-3-usability-review.txt
  ```

  **Commit**: YES (groups with Wave 1)
  - Message: `添加市场调研Skill基础设计（流程、画像库、报告模板）`
  - Files: `调研报告模板.md`

### Wave 2 — 核心模块详细设计（可并行）

- [x] 4. 宏观调研 4 维度引导流程详细设计

  **What to do**:
  - 为宏观调研的每个维度设计详细的交互引导流程：
  - **维度1：行业与市场宏观环境**
    - Skill 提问清单：市场规模估算、PEST 四要素逐一引导
    - 每个 PEST 要素的引导话术示例（Policy/Economy/Society/Technology）
    - PM 可能的回答类型及 Skill 的跟进策略
    - 联网搜索建议触发点（如"需要查询最新行业报告吗？"）
  - **维度2：市场竞争格局**
    - 引导 PM 识别直接竞品、间接竞品、潜在替代品
    - TOP 3 竞品深度拆解的引导框架（产品架构、核心优势、致命弱点）
    - 数据获取引导（提示使用企查查/天眼查/七麦数据）
  - **维度3：目标用户大盘分析**
    - 引导 PM 描述典型用户画像
    - 痛点挖掘的追问策略（现有解决方案、不满之处）
    - 此处与模拟用户系统的衔接点（生成画像库的输入）
  - **维度4：商业模式预判**
    - 盈利模式选项引导（广告/会员/抽成/软硬件结合等）
    - 成本结构估算框架（研发/获客CAC/运营/服务器）
    - 同类产品财报参考引导
  - 每个维度包含：引导话术模板、PM 输入格式建议、Skill 总结确认话术、跳过处理
  - 产出：更新 `市场调研Skill设计.md` 的「宏观调研引导流程」章节

  **Must NOT do**:
  - 不添加 `市场调研思路.md` 中未提及的分析框架
  - 不设计技术实现细节
  - 不假设 PM 熟悉 PEST 等术语（需在引导中自然解释）

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 详细的引导话术和流程设计，需要高质量的中文写作
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 5, 6)
  - **Blocks**: Task 7
  - **Blocked By**: Tasks 1, 3

  **References**:

  **Pattern References**:
  - `市场调研思路.md:9-38` — 宏观调研 4 个维度的完整定义，包括每个维度的子项和调研方法。这是本任务的核心输入，需要逐项转化为引导流程
  - Task 1 产出的 `市场调研Skill设计.md` — 「交互流程总览」和「需求分类逻辑」章节，本任务需在其基础上展开宏观部分的详细设计
  - Task 3 产出的 `调研报告模板.md` — 宏观报告模板结构，引导流程需对齐报告各部分所需的输入

  **Acceptance Criteria**:

  ```
  Scenario: 4 维度全覆盖验证
    Tool: Bash (grep)
    Preconditions: 市场调研Skill设计.md 已更新
    Steps:
      1. grep "行业与市场宏观环境" 市场调研Skill设计.md — 维度1
      2. grep "市场竞争格局" 市场调研Skill设计.md — 维度2
      3. grep "目标用户大盘分析" 市场调研Skill设计.md — 维度3
      4. grep "商业模式预判" 市场调研Skill设计.md — 维度4
      5. grep "PEST" 市场调研Skill设计.md — PEST 分析引导
      6. grep "竞品" 市场调研Skill设计.md — 竞品分析引导
    Expected Result: 所有 grep 均有匹配
    Failure Indicators: 任何维度或关键方法论缺失
    Evidence: .sisyphus/evidence/task-4-dimension-check.txt

  Scenario: 引导话术质量验证
    Tool: Bash (read + Agent 审查)
    Preconditions: 市场调研Skill设计.md 已更新
    Steps:
      1. 读取「宏观调研引导流程」章节
      2. 验证每个维度包含：引导话术示例、PM 输入格式建议、Skill 总结确认话术
      3. 验证术语解释自然融入引导话术（PEST 等不假设 PM 已知）
      4. 验证跳过处理在每个维度中均有说明
    Expected Result: 4 个维度均有完整的引导话术 + 跳过处理
    Failure Indicators: 任何维度缺少话术示例或跳过处理
    Evidence: .sisyphus/evidence/task-4-guide-quality.txt
  ```

  **Commit**: YES (groups with Wave 2)
  - Message: `添加宏观/微观调研维度详细设计和模拟用户能力设计`
  - Files: `市场调研Skill设计.md`

- [x] 5. 微观调研 4 维度引导流程详细设计

  **What to do**:
  - 为微观调研的每个维度设计详细的交互引导流程：
  - **维度1：内部需求溯源与价值评估**
    - 引导 PM 描述数据异动（数据漏斗断裂点）
    - VOC（用户原声）收集引导：客服工单、差评、社群吐槽的整理方式
    - 战略价值定位引导：拉新/留存/变现三选一或组合
    - 内部数据获取建议（SQL 埋点、语义分析工具）
  - **维度2：微观竞品与功能级拆解**
    - 引导 PM 做交互路径拆解（层级、触发条件、操作步数）
    - 跨界参考引导（提示 PM 不限于同行竞品）
    - 功能级舆情收集引导（微博/小红书搜索策略）
    - 录屏分析、用户体验地图绘制建议
  - **维度3：现有用户圈层验证**
    - Kano 模型使用引导：如何将功能拆解为细项、如何设计正反问卷
    - 场景化深度访谈引导：不直接问需求，而是还原场景后追问
    - 此处与模拟用户系统的核心衔接点（触发 Kano 测算 + 模拟访谈）
    - 真实调研补充建议（APP内插屏问卷、电话回访）
  - **维度4：低成本试错与 MVP 测试**
    - Fake Door Test 方案设计引导（按钮文案、落地页内容）
    - 可用性测试方案引导（Figma 原型、观察要点）
    - A/B 测试方案引导（灰度比例、对比指标选择）
    - MVP 工具推荐（如 Maze）
  - 每个维度包含：引导话术模板、PM 输入格式建议、Skill 总结确认话术、跳过处理
  - 产出：更新 `市场调研Skill设计.md` 的「微观调研引导流程」章节

  **Must NOT do**:
  - 不添加 `市场调研思路.md` 中未提及的分析框架
  - 不设计技术实现细节
  - 不假设 PM 熟悉 Kano 模型等术语

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 详细的引导话术和流程设计
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 4, 6)
  - **Blocks**: Task 7
  - **Blocked By**: Tasks 1, 3

  **References**:

  **Pattern References**:
  - `市场调研思路.md:47-72` — 微观调研 4 个维度的完整定义，包括每个维度的子项和调研方法。核心输入，逐项转化为引导流程
  - `市场调研思路.md:63-64` — Kano 模型测算和场景化深度访谈的详细说明，本任务需将此转化为 Skill 引导 PM 的具体交互步骤
  - `市场调研思路.md:67-72` — MVP 测试的三种方法（Fake Door/可用性/A/B），需设计每种方法的引导话术
  - Task 1 产出的 `市场调研Skill设计.md` — 交互流程总览，本任务在其基础上展开微观部分
  - Task 3 产出的 `调研报告模板.md` — 微观报告模板结构，引导需对齐报告所需输入

  **Acceptance Criteria**:

  ```
  Scenario: 4 维度全覆盖验证
    Tool: Bash (grep)
    Preconditions: 市场调研Skill设计.md 已更新
    Steps:
      1. grep "内部需求溯源" 市场调研Skill设计.md — 维度1
      2. grep "微观竞品" 市场调研Skill设计.md — 维度2
      3. grep "用户圈层验证" 市场调研Skill设计.md — 维度3
      4. grep "低成本试错" 市场调研Skill设计.md — 维度4
      5. grep "Kano" 市场调研Skill设计.md — Kano 模型引导
      6. grep "Fake Door" 市场调研Skill设计.md — MVP 测试引导
    Expected Result: 所有 grep 均有匹配
    Failure Indicators: 任何维度或关键方法缺失
    Evidence: .sisyphus/evidence/task-5-dimension-check.txt

  Scenario: 模拟用户衔接点验证
    Tool: Bash (grep)
    Preconditions: 市场调研Skill设计.md 已更新
    Steps:
      1. grep -c "模拟用户" 市场调研Skill设计.md — 计算模拟用户相关引用次数
      2. 验证维度3中明确说明了触发 Kano 测算和模拟访谈的衔接逻辑
    Expected Result: "模拟用户"至少出现 3 次以上，维度3中有明确衔接说明
    Failure Indicators: 模拟用户系统在微观引导中缺少衔接描述
    Evidence: .sisyphus/evidence/task-5-simulation-link.txt
  ```

  **Commit**: YES (groups with Wave 2)
  - Message: `添加宏观/微观调研维度详细设计和模拟用户能力设计`
  - Files: `市场调研Skill设计.md`

- [x] 6. 模拟用户三大能力详细设计

  **What to do**:
  - **能力1：Kano 模型测算**
    - 输入规范：PM 提供的功能细项列表（名称 + 简要描述）
    - 模拟过程设计：每个画像角色对每个功能细项做正向+反向评价
    - 评分规则：必备属性/期望属性/魅力属性/无差异属性/反向属性 的判定逻辑
    - 输出规范：汇总统计表格（功能×属性分类矩阵）+ 优先级排序建议
    - PM 人工调整接口：哪些结果 PM 可以覆盖或微调
  - **能力2：场景化深度访谈**
    - 访谈场景设计：基于画像角色特征生成个性化场景描述
    - 问题生成策略：不直接问"你需要吗"，而是还原场景后追问（参考 `市场调研思路.md:64` 的方法）
    - 回答生成机制：基于角色画像（年龄/职业/痛点/消费偏好）生成符合角色特征的回答
    - 输出规范：访谈记录汇总（按角色分组 or 按主题分组）+ 关键发现提炼
    - 采样策略：50-100 个角色中选取多少做深度访谈（建议 8-15 个典型角色）
  - **能力3：功能意向投票**
    - 投票项设计：功能概念列表（名称 + 一句话描述）
    - 投票选项：想用 / 无所谓 / 不想用（三级）
    - 投票生成机制：每个角色基于自身画像特征和痛点做投票决策
    - 输出规范：每个功能的投票分布（百分比 + 绝对数）+ 热度排序 + 分群统计（如按年龄段/城市等级分组看偏好差异）
  - 设计三大能力的触发时机（在引导流程的哪个节点触发、PM 如何选择使用哪个能力）
  - 产出：更新 `模拟用户系统设计.md` 的「模拟能力设计」章节

  **Must NOT do**:
  - 不写实现代码
  - 不设计 API 接口
  - 不限制模拟结果必须完全"正确"（设计文档需说明这是 AI 模拟，仅供参考）

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 复杂的能力规范设计，需要精确的逻辑描述和结构化表达
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: YES
  - **Parallel Group**: Wave 2 (with Tasks 4, 5)
  - **Blocks**: Tasks 7, 8
  - **Blocked By**: Task 2

  **References**:

  **Pattern References**:
  - `市场调研思路.md:63` — Kano 模型测算的原始定义："把新功能拆解成几个具体的细项，通过问卷测试它们属于必备属性/期望属性/魅力属性"，这是能力1的设计基础
  - `市场调研思路.md:64` — 场景化深度访谈的核心方法论："不要直接问你需要这个功能吗，而是问上次你遇到XX情况时是怎么处理的"，这是能力2的访谈策略基础
  - Task 2 产出的 `模拟用户系统设计.md` — 画像数据结构定义，三大能力需基于此数据结构运作
  - `.sisyphus/drafts/market-research-agent.md:39-41` — 三大模拟能力的确认需求描述

  **Acceptance Criteria**:

  ```
  Scenario: 三大能力完整性验证
    Tool: Bash (grep)
    Preconditions: 模拟用户系统设计.md 已更新
    Steps:
      1. grep "Kano 模型测算" 模拟用户系统设计.md — 能力1
      2. grep "场景化深度访谈" 模拟用户系统设计.md — 能力2
      3. grep "功能意向投票" 模拟用户系统设计.md — 能力3
      4. grep "输入" 模拟用户系统设计.md — 确认有输入规范
      5. grep "输出" 模拟用户系统设计.md — 确认有输出规范
    Expected Result: 三大能力均有定义，且各有输入/输出规范
    Failure Indicators: 任何能力缺失或缺少输入/输出规范
    Evidence: .sisyphus/evidence/task-6-capabilities-check.txt

  Scenario: Kano 输出格式验证
    Tool: Bash (read + Agent 审查)
    Preconditions: 模拟用户系统设计.md 已更新
    Steps:
      1. 读取 Kano 模型测算部分
      2. 验证包含汇总统计表格的格式定义
      3. 验证包含 5 种属性分类（必备/期望/魅力/无差异/反向）
      4. 验证包含 PM 人工调整机制说明
    Expected Result: Kano 输出格式完整，包含统计表格、5种属性、调整机制
    Failure Indicators: 缺少属性分类或调整机制
    Evidence: .sisyphus/evidence/task-6-kano-format.txt

  Scenario: 触发时机与能力选择验证
    Tool: Bash (grep)
    Preconditions: 模拟用户系统设计.md 已更新
    Steps:
      1. grep "触发" 模拟用户系统设计.md — 确认有触发时机说明
      2. 验证说明了在引导流程的哪个节点触发各能力
      3. 验证 PM 可以选择使用哪个能力（非强制全用）
    Expected Result: 有明确的触发时机设计和能力选择机制
    Failure Indicators: 缺少触发时机或选择机制
    Evidence: .sisyphus/evidence/task-6-trigger-check.txt
  ```

  **Commit**: YES (groups with Wave 2)
  - Message: `添加宏观/微观调研维度详细设计和模拟用户能力设计`
  - Files: `模拟用户系统设计.md`

### Wave 3 — 整合与示例

- [x] 7. Skill 核心定义文档整合

  **What to do**:
  - 将前序任务产出的各模块整合为一个完整的 Skill 定义：
  - **Skill 基本信息**：
    - 名称：市场调研助手
    - 核心描述：一句话概括 Skill 的能力和定位
    - 适用场景列表
    - 前置条件（PM 需要准备什么）
  - **Skill Prompt 定义**：
    - 撰写完整的系统 Prompt（System Prompt），使 AI 扮演市场调研助手角色
    - Prompt 中需嵌入：需求分类逻辑、引导流程框架、模拟用户触发条件、报告生成指令
    - Prompt 须符合：不假设 PM 熟悉方法论术语、主动解释关键概念、引导而非灌输
  - **输入/输出规范汇总**：
    - 输入：PM 的需求描述、各维度的回答和资料、功能列表（用于模拟）
    - 输出：完整 Markdown 调研报告、模拟用户验证数据
  - **完整交互流程串联**：
    - 从 Task 1 的总览 + Task 4 的宏观详细 + Task 5 的微观详细，整合为一个连贯的交互流程文档
    - 确保宏观/微观两条路径的衔接点和模拟用户触发点清晰
  - **可选联网能力说明**：
    - 联网搜索的触发条件、搜索范围、结果融入方式
  - 产出：完善并定稿 `市场调研Skill设计.md`

  **Must NOT do**:
  - 不重复前序任务已有内容（引用并整合，不复制粘贴）
  - 不添加新的方法论维度
  - 不设计多 Skill 协作机制

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 整合性写作任务，需要全局视角和连贯的文档组织
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: NO
  - **Parallel Group**: Wave 3 (Task 8 部分依赖本任务)
  - **Blocks**: Task 8
  - **Blocked By**: Tasks 1, 4, 5, 6

  **References**:

  **Pattern References**:
  - Task 1 产出 `市场调研Skill设计.md` — 需求分类逻辑 + 交互流程总览（本任务整合的骨架）
  - Task 4 的宏观调研引导流程 — 4 维度详细引导话术（整合入完整流程）
  - Task 5 的微观调研引导流程 — 4 维度详细引导话术（整合入完整流程）
  - Task 6 产出 `模拟用户系统设计.md` — 三大能力的触发时机和使用方式（整合触发点）
  - `AGENTS.md:AI Agent 操作指南` — Skill 的 Prompt 设计需参考此处的 AI Agent 行为规范
  - `市场调研思路.md` 全文 — 作为 Skill Prompt 中嵌入的方法论知识来源

  **Acceptance Criteria**:

  ```
  Scenario: Skill Prompt 完整性验证
    Tool: Bash (grep)
    Preconditions: 市场调研Skill设计.md 已定稿
    Steps:
      1. grep "System Prompt" 市场调研Skill设计.md 或 grep "系统 Prompt" — 确认包含完整 Prompt
      2. grep "需求分类" 市场调研Skill设计.md — Prompt 中嵌入分类逻辑
      3. grep "宏观调研" 市场调研Skill设计.md
      4. grep "微观调研" 市场调研Skill设计.md
      5. grep "模拟用户" 市场调研Skill设计.md — Prompt 中嵌入触发条件
    Expected Result: Skill Prompt 涵盖分类逻辑、两种模式、模拟用户触发
    Failure Indicators: Prompt 缺少核心模块
    Evidence: .sisyphus/evidence/task-7-prompt-check.txt

  Scenario: 文档连贯性验证
    Tool: Bash (read + Agent 审查)
    Preconditions: 市场调研Skill设计.md 已定稿
    Steps:
      1. 通读完整文档
      2. 验证从"入口→分类→维度引导→模拟用户→报告生成"的流程连贯无断裂
      3. 验证宏观路径和微观路径的分叉点和汇合点清晰
      4. 验证联网搜索能力的触发条件在流程中有明确标注
    Expected Result: 文档作为一个整体可读、可理解、无逻辑跳跃
    Failure Indicators: 流程有断裂、模块间缺少衔接说明
    Evidence: .sisyphus/evidence/task-7-coherence-review.txt
  ```

  **Commit**: YES (groups with Wave 3)
  - Message: `整合Skill核心定义文档并添加端到端示例`
  - Files: `市场调研Skill设计.md`

- [x] 8. 端到端示例编写

  **What to do**:
  - 编写一个完整的端到端调研示例，展示 Skill 的实际运行效果：
  - **选择场景**：选一个具体的产品场景（建议：微观模式 — 电商APP新增直播功能，与 `市场调研思路.md:45` 的案例呼应）
  - **示例内容**：
    - 开场：PM 输入需求描述 → Skill 自动分类为微观调研
    - 维度引导示例：至少完整展示 2 个维度的完整对话（引导提问 → PM 回答 → Skill 总结）
    - 跳过维度示例：展示 PM 跳过 1 个维度时的交互
    - 模拟用户能力示例：
      - Kano 模型：展示 3-5 个功能细项的测算过程和结果表格
      - 深度访谈：展示 2-3 个典型角色的访谈问答摘要
      - 功能投票：展示投票分布统计表格
    - 报告生成：展示最终生成的调研报告片段（基于 Task 3 的模板格式）
  - **示例格式**：
    - 使用 `> Skill:` 和 `PM:` 前缀标注对话方
    - 模拟用户结果用表格和列表呈现
    - 报告片段使用 `调研报告模板.md` 的格式
  - 产出文件：`市场调研Skill示例.md`

  **Must NOT do**:
  - 不编造不合理的数据（模拟数据需标注为 AI 模拟结果）
  - 不在示例中展示全部 4 个维度的完整对话（选 2 个完整 + 1 个跳过即可，避免过长）
  - 不脱离 `调研报告模板.md` 的格式

  **Recommended Agent Profile**:
  - **Category**: `writing`
    - Reason: 需要创造性的示例编写和自然的对话风格
  - **Skills**: `[]`

  **Parallelization**:
  - **Can Run In Parallel**: NO
  - **Parallel Group**: Wave 3 (Sequential after Task 7)
  - **Blocks**: F1, F2, F3
  - **Blocked By**: Tasks 3, 6, 7

  **References**:

  **Pattern References**:
  - `市场调研思路.md:45` — "电商APP加直播"的案例提及，可作为示例场景的灵感来源
  - `市场调研思路.md:63-64` — Kano 模型和场景化访谈的方法论描述，示例需体现这些方法
  - Task 3 产出 `调研报告模板.md` — 报告片段必须使用此模板格式
  - Task 6 产出 `模拟用户系统设计.md` — 三大能力的输入/输出规范，示例需按此格式展示结果
  - Task 7 产出 `市场调研Skill设计.md` — Skill 的完整交互流程，示例需按流程演示
  - Task 2 产出 `模拟用户系统设计.md` — 画像数据结构，示例中的角色需符合此结构

  **Acceptance Criteria**:

  ```
  Scenario: 示例完整性验证
    Tool: Bash (grep)
    Preconditions: 市场调研Skill示例.md 已创建
    Steps:
      1. grep "Skill:" 市场调研Skill示例.md 或 grep "> Skill" — 确认有 Skill 对话
      2. grep "PM:" 市场调研Skill示例.md — 确认有 PM 对话
      3. grep "Kano" 市场调研Skill示例.md — 确认有 Kano 测算示例
      4. grep "投票" 市场调研Skill示例.md — 确认有投票示例
      5. grep "访谈" 市场调研Skill示例.md — 确认有访谈示例
      6. grep "跳过" 市场调研Skill示例.md — 确认有跳过维度示例
    Expected Result: 所有关键要素均在示例中出现
    Failure Indicators: 缺少任何关键能力的示例展示
    Evidence: .sisyphus/evidence/task-8-completeness-check.txt

  Scenario: 报告片段格式一致性验证
    Tool: Bash (read + Agent 审查)
    Preconditions: 市场调研Skill示例.md 和 调研报告模板.md 均已创建
    Steps:
      1. 读取示例中的报告片段
      2. 对比 调研报告模板.md 的格式
      3. 验证报告片段的标题层级、章节结构与模板一致
    Expected Result: 报告片段完全遵循模板格式
    Failure Indicators: 格式偏离模板
    Evidence: .sisyphus/evidence/task-8-format-consistency.txt

  Scenario: 模拟数据合理性验证
    Tool: Bash (read + Agent 审查)
    Preconditions: 市场调研Skill示例.md 已创建
    Steps:
      1. 读取 Kano 测算结果表格
      2. 验证百分比之和为 100%
      3. 验证投票分布数据合理（不是所有人都选同一项）
      4. 验证访谈角色的回答与其画像特征一致
    Expected Result: 数据逻辑自洽，角色回答符合画像特征
    Failure Indicators: 百分比不合理或角色回答与画像矛盾
    Evidence: .sisyphus/evidence/task-8-data-sanity.txt
  ```

  **Commit**: YES (groups with Wave 3)
  - Message: `整合Skill核心定义文档并添加端到端示例`
  - Files: `市场调研Skill示例.md`

---

## 最终验证波次

> 3 个审查 Agent 并行运行。全部必须通过。拒绝 → 修复 → 重跑。

- [x] F1. **方法论覆盖度审查** — `deep`
逐一对照 `市场调研思路.md` 的所有维度和调研方法，验证 Skill 设计文档是否完整覆盖。检查宏观 4 维度 + 微观 4 维度的每个子项是否都有对应的引导流程。检查所有 `**调研方法：**` 行中提到的工具和步骤是否在 Skill 中有对应的操作指引。
   
   **检查结果：**
   - 宏观维度 [4/4] ✅ 行业环境、竞争格局、用户分析、商业模式完整覆盖
   - 微观维度 [4/4] ✅ 需求溯源、竞品分析、用户验证、MVP测试完整覆盖
   - 调研方法覆盖 [20/21] ⚠️ 20/21个调研方法完整覆盖，遗漏百度指数/微信指数趋势分析
   - 结论: 通过（整体覆盖度95%，可直接用于实现）
   
   输出: `宏观维度 [4/4] | 微观维度 [4/4] | 调研方法覆盖 [20/21] | 结论: 通过`

- [x] F2. **文档质量与规范审查** — `unspecified-high`
  检查所有产出文档是否符合 `AGENTS.md` 的内容规范：简体中文、Markdown 风格（`#`/`###`/`####`层级、`*`列表、`---`分隔）、术语一致性、无机翻痕迹。检查文件命名是否符合规范（描述性中文名称）。验证报告模板的完整性和可用性。
  输出: `语言规范 [通过/问题N个] | 格式规范 [通过/问题N个] | 术语一致性 [通过/问题N个] | 结论: 通过/拒绝`

- [x] F3. **设计完整性与可实现性审查** — `deep`
  审查 Skill 设计是否足够详细可直接用于实现：Prompt 是否清晰无歧义、交互流程是否有遗漏分支、模拟用户数据结构是否完整可用（JSON Schema 是否有效）、Kano 测算逻辑是否可执行。检查端到端示例是否覆盖了主要场景。标记任何需要 PM 做额外决策的模糊点。
  输出: `Prompt 清晰度 [高/中/低] | 流程完整度 [N/N 分支] | 数据结构 [完整/缺字段] | 示例覆盖 [N/N 场景] | 结论: 通过/拒绝`

---

## 提交策略

| 波次 | 提交信息 | 文件 |
|------|----------|------|
| Wave 1 | `添加市场调研Skill基础设计（流程、画像库、报告模板）` | 新增的设计文档 |
| Wave 2 | `添加宏观/微观调研维度详细设计和模拟用户能力设计` | 更新的设计文档 |
| Wave 3 | `整合Skill核心定义文档并添加端到端示例` | 整合文档 + 示例 |

---

## 成功标准

### 最终检查清单
- [x] 所有「必须包含」项均已体现在设计文档中
- [x] 所有「必须不包含」项均未出现
- [x] `市场调研思路.md` 的 8 个维度 100% 覆盖
- [x] 模拟用户系统的三大能力（Kano/访谈/投票）均有完整设计
- [x] 报告模板可直接使用
- [x] 端到端示例完整且自洽
- [x] 所有文档符合 AGENTS.md 内容规范
