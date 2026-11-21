# 8 个能力域

**术语**：0–7 是「能力域」（capability areas）——AI 全栈开发的成熟度维度。"Stage" 专指 `stage-openspec-skills` 的治理层交付容器，二者同名不同义，本文件只用「能力域」。

## 双层总览（先分清层，再判状态）

8 个能力域分两层。共用的基础设施前置、建一次；feature 特有的东西随循环反复。混在一条线上评估会误判优先级。

| 层 | 能力域 | 性质 |
|---|---|---|
| **底座层**（项目级，建一次） | 0 上下文与约束、2a 全局 UX 骨架、3 UI 设计系统、4a 契约规范 | 跨 feature 复用，先于任何具体 feature；缺了会让每个 feature 重复还债 |
| **循环层**（feature 级，反复走） | 1 需求、2b 局部 UX、4b 契约实例、5 实现、6 验证、7 交付 | 每个 feature 走一遍，需求也逐 feature 明细 |

**为什么 2、4 要拆 a/b**：UX 的"信息架构骨架 / 导航 / 交互范式"是全项目共用的（前置），而"某 feature 的交互流程与四态"依赖该 feature 需求（后置）；契约的"规范与生成机制"是底座（先立），"某 feature 的具体数据模型与接口"是循环实例（随 feature 冻结）。**UI 设计系统（域 3）整体属于底座**——它像 `AGENTS.md` 一样是全局约束，后置会导致风格漂移、语义 token 失效。

四态：**🟢 green** = 已建立且可验证；**🟡 yellow** = 已建立但有明确 gap；**🔴 red** = 缺失或不可验证；**⚪ 未知** = fog of war，尚未看见，需提问。

每个能力域给出：目的、green 判据（可复核）、探测信号、yellow/red 表现。判定时把「green 判据」当 checklist，逐条找 evidence。判定结果要带上层次标注（底座 / 循环），存档里据此排序。

---

## 域 0 — 上下文与约束 `【底座层】`

**目的**：让 agent 开工前就知道项目规则、工作流与质量底线，而不是每次重新推断。

**green 判据**（逐条找 evidence）：
- 存在 agent 规则文件（`AGENTS.md` / `CLAUDE.md`），内容是**可执行的约定**（命令、工作流、护栏），不是泛泛而谈。
- 有可运行的质量命令（lint / typecheck / test），且在规则文件或 CI 中被引用。
- 治理层一致：规则、RUNBOOK、OpenSpec 配置互不矛盾。

**探测信号**：根目录 `AGENTS.md`/`CLAUDE.md`、`doc/workflow/RUNBOOK.md`、`openspec/config.yaml`、`.claude/skills/`、`package.json` scripts、`Makefile`。
**yellow/red**：有规则文件但陈旧或自相矛盾 → yellow；完全没有 → red。治理漂移交给 `stage-project-bootstrap`，从零建立交给 `stage-openspec-init`。

---

## 域 1 — 需求澄清与规格 `【循环层】`

**目的**：把"想要什么"变成可验收的规格，让 AI 有明确的 done 定义。分两层：**全局产品定义**（范围、目标用户、核心流程）先定，喂给底座层；**局部需求**（当前 feature 的五维明细）逐 feature 展开。

**green 判据**：
- 当前工作有可验收的标准（Goals / acceptance criteria / OpenSpec delta specs 之一）。
- 需求描述覆盖：视觉、结构、行为、数据、约束五维中的相关维度。
- 存在"待实现"的变更提案（`doc/openspec/changes/` 活跃 change 或等价 backlog）。
- 全局产品定义可辨认（支撑域 2a 的骨架决策）。

**探测信号**：`doc/openspec/changes/`、`docs/specs/`、`PLANS.md`、issue 跟踪、需求文档、产品愿景 / README。
**yellow/red**：有提案但验收标准不可测 → yellow；直接凭口头需求开工 → red。
**注意**：Fast Track 跳过本域。

---

## 域 2a — UX 全局骨架 `【底座层】`

**目的**：先定全项目共用的信息架构、导航与交互范式，前置到任何 feature 之前，让后续页面有统一的骨架。

**green 判据**：
- 有全局信息架构 / 页面结构 / 导航结构（线框、ASCII 草图、站点地图任一）。
- 有跨 feature 一致的交互范式（如统一的错误呈现、导航模式、反馈方式）。
- 关键取舍有记录，不是每个 feature 即兴发挥。

**探测信号**：路由结构、`docs/ux/`、站点地图、全局线框、设计稿里的页面层级。
**yellow/red**：只有零散单页草图、无全局结构 → yellow；无骨架直接编码 → red。

---

## 域 2b — UX 局部交互 `【循环层】`

**目的**：每个 feature 自己的交互流程与状态覆盖，依赖该 feature 需求明确后再做。

**green 判据**：
- 当前 feature 有交互流程（线框 / 流程图任一）。
- 关键页面覆盖四种状态：empty / loading / error / success。
- 交互决策经过推敲（有记录的关键取舍），不是 AI 即兴发挥。

**探测信号**：feature 级设计稿、`*.md` 里的线框 / 流程图、状态枚举。
**yellow/red**：只有静态 happy path，缺空/错/载态 → yellow；无结构直接编码 → red。

---

## 域 3 — UI：设计系统与语义 token `【底座层】`

**目的**：把设计约束固化成 agent 可复用的全局规则，防止每次生成风格漂移（AI slop）。它是底座，必须先于任何具体 feature 建立——后置会导致每个 feature 各写一套、语义 token 失效。

**green 判据**：
- 有设计系统或组件库约定，agent 可读到（skill、文档或 token 文件）。
- 颜色/间距/字号用**语义 token**（如 `--color-primary`），而非散落的硬编码值。
- 有反 AI-slop 约束（一致性、代码体量、状态覆盖）写入 agent 规则。

**探测信号**：`*.css` 变量、`tailwind.config.*`、`theme.ts`、`design-system/`、frontend-design 类 skill。
**yellow/red**：有组件库但硬编码横飞 → yellow；无约定、每次自由发挥 → red。

---

## 域 4a — 契约规范与生成机制 `【底座层】`

**目的**：先立"契约怎么写、怎么生成、以谁为单一事实来源"的规范，一次建立、全项目复用。这是 tracer bullet 的底座部分。

**green 判据**：
- 有契约规范（API 风格、版本策略、命名约定）写入 agent 可读的规则文件。
- 有契约优先的生成机制（如 OpenAPI → 生成 TS client 的管线），而非纯手工同步。
- 契约产物在版本控制中，被确立为单一事实来源。

**探测信号**：`openapi.yaml`/`swagger.json` 的生成配置、`*.d.ts` 生成脚本、`schemas/`、codegen 配置、规则文件里的 API 约定。
**yellow/red**：有契约文件但靠手工同步 → yellow；无规范、无生成机制 → red。

---

## 域 4b — 契约实例 `【循环层】`

**目的**：每个 feature 的具体数据模型 / API / 前端类型，随该 feature 冻结，是并行开发的枢纽。

**green 判据**：
- 当前 feature 的数据模型 / API 契约 / 前端类型三者一致（走域 4a 的生成机制，或等价契约优先方案）。
- 前后端都从同一契约派生，而非各自手写。

**探测信号**：feature 的 `openapi.yaml`/`swagger.json`、`*.d.ts`、`schemas/`、生成的 client。
**yellow/red**：有契约但偶有漂移 → yellow；前后端各写各的 → red。
**注意**：Fast Track 跳过本域；Standard Flow 下 4a / 4b 不 green 时禁止跳到域 5。

---

## 域 5 — 并行实现 `【循环层】`

**目的**：契约冻结后，前后端各自的高效实现循环。

**green 判据**：
- 后端：领域逻辑 → API → 单元测试的顺序可辨认，有测试。
- 前端：脚手架 → 逻辑 → UI → 状态 → 组件测试的顺序可辨认，测试用 mock 隔离后端。
- 两侧都能独立跑通（后端 `test`，前端 `test`/`build`）。

**探测信号**：测试目录、后端 manifest 与 `test` 命令、前端 `package.json` 测试脚本、mock/fixture。
**yellow/red**：有代码无测试，或测试跑不通 → yellow；无结构的大杂烩 → red。

---

## 域 6 — 集成与持续验证 `【循环层】`

**目的**：把"验证"从交付后补做，变成过程中常驻的回路。

**green 判据**：
- 有视觉反馈回路（浏览器截图 / 视觉 diff MCP，如 DevLens、PageLens）。
- 有 E2E 或集成测试覆盖关键路径。
- 前端有性能预算（Lighthouse / bundle size 目标）并被检查。

**探测信号**：E2E 配置（Playwright/Cypress）、MCP 配置、`lighthouserc.*`、CI 里的验证步骤。
**yellow/red**：只有手工肉眼检查 → yellow；写完从不验证 → red。

---

## 域 7 — 加固与交付 `【循环层】`

**目的**：上线前的审查、自动化护栏与收尾。

**green 判据**：
- 有代码审查机制（reviewer subagent / PR 检查 / `/code-review` 之一）。
- 有 hooks 或 CI 强制零例外规则（format、lint、typecheck、test 卡口）。
- 变更有归档与追溯（OpenSpec change archive / Stage exit gate）。

**探测信号**：`.claude/settings.json` hooks、CI 配置、PR 模板、`openspec/changes/archive/`、Stage 文档。
**yellow/red**：靠人记得跑检查 → yellow；无任何卡口 → red。
**注意**：change archive ≠ Stage exit，两道 gate 分开（见 `stage-change-workflow`）。
