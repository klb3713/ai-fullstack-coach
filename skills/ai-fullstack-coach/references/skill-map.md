# 能力域 → 推荐 skills

按判定结果推荐。**只推荐已核实的 skill 名**（下列官方套装与各栈单项均已核实并附仓库 URL）；个别标「待核实」的索引站条目不要当确认项写入报告，作为"可搜索方向"提示用户装前用 `npx skills list` / 搜索确认。安装统一用 `npx skills add <owner/repo>` 或包自带命令。

## 本地可隐式调用的 skill（来自宿主 catalog）

这类 skill 在宿主环境中已存在、可被 agent 调用，可放心推荐：

| 能力域（层） | skill | 用途 |
|---|---|---|
| 0（底座） | `writing-for-agents` | 写 / 改 `AGENTS.md`、`CLAUDE.md`、skill 时 |
| 0（底座）/ 3（底座） | `skill-creator` | 需要为某技术栈或设计系统生成项目本地 skill 时 |
| 1 / 2a / 2b（循环 / 底座） | `grilling` | 拷问产品定义、需求、全局骨架与交互取舍，把模糊处逼出确定性 |
| 7（循环） | `stage-change-workflow` | change 归档、Stage exit gate、Fast Track lane |

## OpenSpec 治理层三件套（repo-local，来自用户自建 stage-openspec-skills）

这套 skill 是本地文件，安装后按路径调用；名字是英文 canonical，中文触发词写在各自 description 里。**它们替换通用 `openspec-*` 系列**：本教练不推荐 `openspec-explore` / `openspec-propose` / `openspec-update-change`，一律用下面三个。

| 场景 | 推荐 skill | 映射到能力域 |
|---|---|---|
| 新仓库 / 无治理层，从零建 Stage + OpenSpec 骨架 | `stage-openspec-init` | 0 |
| 治理层已存在但漂移、旧模板脚本引用、文档命令不一致 | `stage-project-bootstrap` | 0（维护） |
| 开工：判定 Stage / change / Fast Track，建 artifacts，走 review → 实现 → archive → Stage exit | `stage-change-workflow` | 1 + 实现 + 7 |

- 硬依赖：OpenSpec 2.1.0 + Node.js ≥22.20.0，版本不符先停（见 `assessment-guide.md`）。
- 安装：`stage-openspec-skills` 仓库的三个 skill 目录，复制到目标项目 `.claude/skills/`（或对应 skills 目录）。
- 关键区分：change archive ≠ Stage exit，两道 gate 分开，`stage-change-workflow` 守这两道门。

## 官方套装（一次装一批，优先）

两个官方仓库覆盖大部分前端设计能力，先装套装再按需补社区单项。安装后 `npx skills list` 确认实际 skill 名（不同版本前缀可能不同）。

- **Anthropic 官方**：`npx skills add anthropics/skills` → `frontend-design`（anti-slop）、`theme-factory`（成套主题）、`brand-guidelines`（品牌色与排版）、`web-artifacts-builder`、`canvas-design`、`algorithmic-art`。来源：[anthropics/skills](https://github.com/anthropics/skills)。
- **Vercel 官方**：`npx skills add vercel-labs/agent-skills` → `web-design-guidelines`（UI/无障碍审查，100+ 规则）、`react-best-practices`（React/Next.js 优化；部分来源称 skill 名为 `vercel-react-best-practices`，装后以 `npx skills list` 为准）、`vercel-composition-patterns`（React 组合模式，含 React 19 API）、`react-native-guidelines`、`vercel-deploy-claimable`。来源：[vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)、[composition-patterns 说明](https://skillhub.360.com/skills/vercel-labs/agent-skills/vercel-composition-patterns)。

## 前端设计类（社区单项，已核实包名与安装命令）

| 能力域 | skill / 工具 | 安装命令 / 来源 |
|---|---|---|
| 3 | `frontend-design`（anti-AI-slop，token 级设计计划） | `npx skills add anthropics/skills` · [repo](https://github.com/anthropics/skills) |
| 3 | `theme-factory`（成套主题，避免逐页定色） | 同上套装 · 官方 |
| 3 | `brand-guidelines`（品牌规范落到输出） | 同上套装 · 官方 |
| 3 | `web-design-guidelines`（UI/无障碍审查） | `npx skills add vercel-labs/agent-skills` · [repo](https://github.com/vercel-labs/agent-skills) |
| 5 / 3 | `react-best-practices`（React/Next 优化规则） | 同上套装 · 名称以 `npx skills list` 为准 |
| 3 | `vercel-composition-patterns`（避免布尔 prop 膨胀） | 同上套装 · [说明](https://baizhi.cloud/landing/skillhub/detail/vercel-labs/agent-skills/vercel-composition-patterns) |
| 2 / 3 | `ui-ux-pro-max`（多风格/多栈设计系统，状态与无障碍规则） | `npx -y skills add nextlevelbuilder/ui-ux-pro-max-skill --skill ui-ux-pro-max --agent claude-code` · [repo](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |
| 3 | `@ctxr/skill-frontend-excellence`（框架无关性能预算 + 19 阶段审计） | `npx @ctxr/kit@latest install @ctxr/skill-frontend-excellence` · [npm](https://www.npmjs.com/package/@ctxr/skill-frontend-excellence) |
| 3 | daisyUI frontend skill（短 class 约定，如 `btn btn-primary`） | `npx -y skills add saadeghi/daisyui` · [源](https://github.com/saadeghi/daisyui) |
| 6 | DevLens MCP（持久 Chromium，写文件后 ~50ms 自动截图，dl_capture/dl_diff） | npm 包 `devlens-mcp` · [npm](https://www.npmjs.com/package/devlens-mcp) |
| 6 | PageLens（零配置视觉/控制台 diff） | `npx pagelens http://localhost:3000` |

> 未核实项：索引站 `github.com/ezra-y/awesome-claude-ui-armory` 还列有 design-system、impeccable 等，具体安装名装前用 `npx skills list` / 搜索确认，不臆造。
> 前端三分叉（框架 / 样式 / SSR）的具体差异处理见 `frontend-stack-map.md`。

## 后端栈类（已核实包名）

检测信号、确定性检查命令见 `backend-stack-map.md`。社区可装 skill：

- **通用多栈后端包**：`jeffallan/claude-skills`（Fullstack Dev Skills Plugin，覆盖多语言），`npx skills add jeffallan/claude-skills` · [repo](https://github.com/jeffallan/claude-skills)。
- **Python / Go / Rust / Node.js 各自的已核实包名**见 `backend-stack-map.md`，逐个对应安装命令。

推荐纪律：官方/知名仓库（anthropics、vercel-labs 等）可直接装；其余按 `skills-lock.json` 锁版本、按需安装。本地生成 skill（`skill-creator`）仍是与社区包并行的默认策略，用于固化项目独有约定。

## 易混淆项

- `ai-transform-review`：评估**人 / 团队**的 AI 使用情况并推送存档，不是评估项目工程状态。用户说"评估我的 AI 能力"用它；说"评估这个项目该怎么推进"用本教练。
- `grilling`：stress-test 一个计划 / 决策，是拷问器，不做成熟度评估。可在域 1 / 2 需要逼出确定性时配合本教练使用。
