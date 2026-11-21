# 8 个能力域

**术语**：0–7 是「能力域」（capability areas）——AI 全栈开发的成熟度维度。"Stage" 专指 `stage-openspec-skills` 的治理层交付容器，二者同名不同义，本文件只用「能力域」。

四态：**🟢 green** = 已建立且可验证；**🟡 yellow** = 已建立但有明确 gap；**🔴 red** = 缺失或不可验证；**⚪ 未知** = fog of war，尚未看见，需提问。

每个能力域给出：目的、green 判据（可复核）、探测信号、yellow/red 表现。判定时把「green 判据」当 checklist，逐条找 evidence。

---

## 域 0 — 上下文与约束

**目的**：让 agent 开工前就知道项目规则、工作流与质量底线，而不是每次重新推断。

**green 判据**（逐条找 evidence）：
- 存在 agent 规则文件（`AGENTS.md` / `CLAUDE.md`），内容是**可执行的约定**（命令、工作流、护栏），不是泛泛而谈。
- 有可运行的质量命令（lint / typecheck / test），且在规则文件或 CI 中被引用。
- 治理层一致：规则、RUNBOOK、OpenSpec 配置互不矛盾。

**探测信号**：根目录 `AGENTS.md`/`CLAUDE.md`、`doc/workflow/RUNBOOK.md`、`openspec/config.yaml`、`.claude/skills/`、`package.json` scripts、`Makefile`。
**yellow/red**：有规则文件但陈旧或自相矛盾 → yellow；完全没有 → red。治理漂移交给 `stage-project-bootstrap`，从零建立交给 `stage-openspec-init`。

---

## 域 1 — 需求澄清与规格

**目的**：把"想要什么"变成可验收的规格，让 AI 有明确的 done 定义。

**green 判据**：
- 当前工作有可验收的标准（Goals / acceptance criteria / OpenSpec delta specs 之一）。
- 需求描述覆盖：视觉、结构、行为、数据、约束五维中的相关维度。
- 存在"待实现"的变更提案（`doc/openspec/changes/` 活跃 change 或等价 backlog）。

**探测信号**：`doc/openspec/changes/`、`docs/specs/`、`PLANS.md`、issue 跟踪、需求文档。
**yellow/red**：有提案但验收标准不可测 → yellow；直接凭口头需求开工 → red。
**注意**：Fast Track 跳过本域。

---

## 域 2 — UX：信息架构与交互状态

**目的**：先定结构、流程与状态，再谈视觉，避免 AI 一上来就画像素。

**green 判据**：
- 有信息架构 / 页面结构 / 交互流程（线框、ASCII 草图、流程图任一）。
- 关键页面覆盖四种状态：empty / loading / error / success。
- 交互决策经过推敲（有记录的关键取舍），不是 AI 即兴发挥。

**探测信号**：设计稿、`*.md` 里的线框 / 流程图、路由结构、`docs/ux/`、状态枚举。
**yellow/red**：只有静态 happy path，缺空/错/载态 → yellow；无结构直接编码 → red。

---

## 域 3 — UI：设计系统与语义 token

**目的**：把设计约束固化成 agent 可复用的规则，防止每次生成风格漂移（AI slop）。

**green 判据**：
- 有设计系统或组件库约定，agent 可读到（skill、文档或 token 文件）。
- 颜色/间距/字号用**语义 token**（如 `--color-primary`），而非散落的硬编码值。
- 有反 AI-slop 约束（一致性、代码体量、状态覆盖）写入 agent 规则。

**探测信号**：`*.css` 变量、`tailwind.config.*`、`theme.ts`、`design-system/`、frontend-design 类 skill。
**yellow/red**：有组件库但硬编码横飞 → yellow；无约定、每次自由发挥 → red。

---

## 域 4 — 契约（tracer bullet）

**目的**：冻结前后端共享契约，这是并行开发的枢纽。

**green 判据**：
- 数据模型 / API 契约 / 前端类型三者一致（OpenAPI + 生成的 TS 类型，或等价契约优先方案）。
- 契约在版本控制中，可作为单一事实来源。
- 前后端都从同一契约派生，而非各自手写。

**探测信号**：`openapi.yaml`/`swagger.json`、`*.d.ts`、`schemas/`、`api/` contract、生成的 client。
**yellow/red**：有契约但手工同步、偶有漂移 → yellow；前后端各写各的 → red。
**注意**：Fast Track 跳过本域；Standard Flow 下本域不 green 时禁止跳到域 5。

---

## 域 5 — 并行实现

**目的**：契约冻结后，前后端各自的高效实现循环。

**green 判据**：
- 后端：领域逻辑 → API → 单元测试的顺序可辨认，有测试。
- 前端：脚手架 → 逻辑 → UI → 状态 → 组件测试的顺序可辨认，测试用 mock 隔离后端。
- 两侧都能独立跑通（后端 `test`，前端 `test`/`build`）。

**探测信号**：测试目录、后端 manifest 与 `test` 命令、前端 `package.json` 测试脚本、mock/fixture。
**yellow/red**：有代码无测试，或测试跑不通 → yellow；无结构的大杂烩 → red。

---

## 域 6 — 集成与持续验证

**目的**：把"验证"从交付后补做，变成过程中常驻的回路。

**green 判据**：
- 有视觉反馈回路（浏览器截图 / 视觉 diff MCP，如 DevLens、PageLens）。
- 有 E2E 或集成测试覆盖关键路径。
- 前端有性能预算（Lighthouse / bundle size 目标）并被检查。

**探测信号**：E2E 配置（Playwright/Cypress）、MCP 配置、`lighthouserc.*`、CI 里的验证步骤。
**yellow/red**：只有手工肉眼检查 → yellow；写完从不验证 → red。

---

## 域 7 — 加固与交付

**目的**：上线前的审查、自动化护栏与收尾。

**green 判据**：
- 有代码审查机制（reviewer subagent / PR 检查 / `/code-review` 之一）。
- 有 hooks 或 CI 强制零例外规则（format、lint、typecheck、test 卡口）。
- 变更有归档与追溯（OpenSpec change archive / Stage exit gate）。

**探测信号**：`.claude/settings.json` hooks、CI 配置、PR 模板、`openspec/changes/archive/`、Stage 文档。
**yellow/red**：靠人记得跑检查 → yellow；无任何卡口 → red。
**注意**：change archive ≠ Stage exit，两道 gate 分开（见 `stage-change-workflow`）。
