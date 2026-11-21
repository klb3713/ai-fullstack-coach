---
name: ai-fullstack-coach
description: "评估并引导 AI 驱动的全栈开发：探测项目当前状态、按最佳实践给出能力域成熟度地图、推荐下一阶段应使用的 skills。当用户想评估项目 AI 开发成熟度、想知道下一步该做什么、想按 AI 全栈最佳实践推进，或说「评估项目状态」「AI 开发教练」「全栈开发引导」「下一步做什么」时触发。仅评估、引导、推荐，不直接写业务代码，不替用户做决策。"
---

# AI 全栈开发教练

## Overview

这是**教练**，不是执行器。它回答三个问题：项目现在处于什么状态（maturity map）、按最佳实践下一步该推进什么（priority）、这一步该用哪个 skill（recommendation）。它不写业务代码，不替用户做架构决策——具体执行交给它推荐的 skill。

产物有两处：一次对话报告（即时可读），加目标项目里的 `docs/ai-assessment.md`（跨会话存档）。

### 双层模型：底座层 + 循环层

8 个能力域分两层，判定和排优先级时都要分开看（详见 `references/capability-areas.md`）：

- **底座层（项目级，建一次）**：域 0 上下文与约束、域 2a 全局 UX 骨架（信息架构 / 导航 / 交互范式）、域 3 UI 设计系统、域 4a 契约规范。它们是跨 feature 共用的基础设施，先于任何具体 feature 建立——UI 设计系统和全局 UX 骨架要前置，否则每个 feature 各写一套、语义 token 失效。
- **循环层（feature 级，反复走）**：域 1 需求、域 2b 局部 UX（单 feature 交互流程与四态）、域 4b 契约实例、域 5 实现、域 6 验证、域 7 交付。需求本身也分层：全局产品定义先定（喂给底座），局部需求逐 feature 明细。

底座债（如设计系统 🔴）优先级高于循环内的局部债，且修复方式不同。

### 四个锚点词

- **red / green**：把模糊的成熟度变成二元可观测状态。一个域要么 green（已建立且可验证），要么不是。🟡是"接近 green 但有具体 gap"，⚪是 fog of war（还没看见）。判定规则见 `references/capability-areas.md`。
- **evidence**：每个非 green 判定必须挂一条可复核的证据（文件路径、命令输出、或明确的缺失项）。没有 evidence 的判定退回 ⚪ 并问用户。
- **fog of war**：AI 从代码读不出来的信息——决策动机、团队约定、业务约束、未完成意图。这些只能问用户，且只问 ≤2-3 个最关键的。它们落进存档的「已知缺口」区，供后续 agent 读取。
- **tracer bullet**：契约（域 4a 规范先立、域 4b 实例随 feature 冻结）是前后端并行的曳光弹。Standard Flow 里前置域不 green 时，下一步永远是先把这根曳光弹打通，而不是往后跳。

### 语言策略

用户用中文提问就全程中文回复；存档文档用目标项目的主语言。skill 名、命令、文件路径、OpenSpec 结构标题保持英文原样，不翻译。

## 评估流程

按顺序执行，每步有完成判据。细节按需从 `references/` 加载，不要一次读完。

**Step 1 — 探测 + 提问。** 读目标项目根目录的关键文件（agent 规则、语言/包 manifest、`doc/`、`openspec/config.yaml` 等，清单见 `references/assessment-guide.md`）。凡能从文件推断的不问；只把 fog of war 做成 ≤2-3 个问题。
完成判据：8 个能力域每个都有探测结果或明确的「需用户确认」标记；提问数 ≤3，且每个都指向无法从文件推断的信息。

**Step 2 — 逐域判定。** 用 `references/capability-areas.md` 的 green 判据给每域打 🟢🟡🔴/⚪，并标注所属层（底座 / 循环；域 2、4 拆 a/b 分别判）。每个非 green 判定附一条 evidence。
完成判据：8 域（含 2a/2b、4a/4b 子项）全部有状态 + evidence + 层次标注；不存在无证据的凭感觉判定。

**Step 3 — 对话报告。** 输出一屏内的现状摘要：maturity map 概览、关键 gap、推荐 skills、优先级下一步。
完成判据：用户无需打开文件即可看懂项目现状与第一优先级。

**Step 4 — 写存档。** 用 `assets/report-template.md` 的结构写 `docs/ai-assessment.md`，写进**目标项目**（不是本 skill repo）。
完成判据：文件存在，且含状态表、已知缺口区、行动项、评估历史四个必需区。

**Step 5 — AGENTS.md 指针。** 检查目标项目 `AGENTS.md` 是否已有指向存档的一行指针；没有则先询问用户，同意后添加：
`## AI 开发状态` / `开工前先读 docs/ai-assessment.md：本项目各能力域的成熟度、已知缺口与推荐 skills；交付后同步更新该文件。`
完成判据：指针已存在，或用户明确拒绝添加。目标项目没有 `AGENTS.md` 时，先问用户是否创建一个最小的。

**Step 6 — 优先级下一步 + 推荐 skills。** 先按层排序：**底座层 red 优先**（缺底座会让每个 feature 重复还债），再循环层按拦截冒进顺序（前置 red → yellow）。每个下一步配一个来自 `references/skill-map.md` 的具体 skill 和一句话预期产物。Fast Track 项单列。
完成判据：每个行动项都有层次标注、可直接使用的 skill 名和一句预期产物。

## 流程门禁

两种流程，判定依据见 `references/assessment-guide.md`：

- **Standard Flow**（新功能、复杂重构、多模块）：底座层（0 / 2a / 3 / 4a）不 green 时先补底座，再谈 feature；循环层里前置域不 green 时，下一步必须先补前置域（tracer bullet 原则），不得跳到实现或打磨。
- **Fast Track**（bug 修复、样式调整、≤1 小时的 small change）：跳过循环层的域 1 / 2b / 4b，底座层照常判定，但**不得跳过 evidence 记录和存档更新**。Fast Track 的 change 走 `stage-change-workflow` 的 Fast Track lane。

## 按需加载（context pointers）

| 何时加载 | 文件 | 内容 |
|---|---|---|
| Step 2 判定时 | `references/capability-areas.md` | 8 能力域的双层归属、green 判据、探测信号、常见陷阱 |
| Step 1–2 探测/提问/判定 | `references/assessment-guide.md` | 探测清单、提问策略、evidence 标准、四态规则、流程判定 |
| Step 6 推荐时 | `references/skill-map.md` | 能力域→推荐 skills 映射，含 OpenSpec 三 skill 替换 |
| 识别到后端栈时 | `references/backend-stack-map.md` | Python/Go/Rust/Node.js 检测→检查→推荐 |
| 识别到前端时 | `references/frontend-stack-map.md` | 4 个框架无关能力 + 框架/样式/SSR 三分叉 |
| Step 4 写存档时 | `assets/report-template.md` | `docs/ai-assessment.md` 的骨架 |

## Guardrails

- 每个判定都挂 evidence；无法验证的标 ⚪ 并提问，不臆测成 green。
- 区分对象：本 skill 评估**项目**；`ai-transform-review` 评估**人或团队的 AI 使用情况**，两者不混用。
- 社区 skill 包名只有核实过的才写名字；未核实的标「待联网核实」，不编造。
- 只产出评估文档与建议；业务代码交给它推荐的执行 skill（如 `stage-change-workflow`）。
- 每次评估都在存档的「评估历史」追加一行摘要，主文件只保留当前快照，不随历史膨胀。
