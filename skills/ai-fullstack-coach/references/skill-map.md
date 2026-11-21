# 能力域 → 推荐 skills

按判定结果推荐。**只推荐已核实的 skill 名**；标注「待联网核实」的社区包不要当确认项写入报告，而是作为"可搜索方向"提示用户自行核实（联网恢复后用 `npx skills add` 或对应渠道安装）。

## 本地可隐式调用的 skill（来自宿主 catalog）

这类 skill 在宿主环境中已存在、可被 agent 调用，可放心推荐：

| 能力域 | skill | 用途 |
|---|---|---|
| 0 | `writing-for-agents` | 写 / 改 `AGENTS.md`、`CLAUDE.md`、skill 时 |
| 0 | `skill-creator` | 需要为某技术栈生成项目本地 skill 时 |
| 1 / 2 | `grilling` | 拷问计划 / 需求 / 交互取舍，把模糊处逼出确定性 |
| 7 | `stage-change-workflow` | change 归档、Stage exit gate、Fast Track lane |

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

## 前端设计类（社区，核实状态见括号）

| 能力域 | skill / 工具 | 核实状态 |
|---|---|---|
| 3 | `frontend-design`（anthropics/skills 官方，anti-AI-slop，token 级设计计划） | 已核实：github.com/anthropics/skills，经 skills.sh 分发 |
| 3 | `@ctxr/skill-frontend-excellence`（框架无关性能预算 + 19 阶段审计） | 已核实：npm 包，`npx @ctxr/kit@latest install @ctxr/skill-frontend-excellence` |
| 3 | daisyUI frontend skill（短 class 约定，如 `btn btn-primary`） | 已核实：`npx -y skills add saadeghi/daisyui` |
| 2 / 3 | ui-ux-pro-max、design-system、theme-factory、react-best-practices | 待联网核实：见 github.com/ezra-y/awesome-claude-ui-armory 索引，具体安装名先核实 |
| 6 | DevLens MCP（持久 Chromium，写文件后 ~50ms 自动截图，dl_capture/dl_diff） | 已核实：npm 包 `devlens-mcp` |
| 6 | PageLens（零配置视觉/控制台 diff） | 已核实：`npx pagelens http://localhost:3000` |

前端三分叉（框架 / 样式 / SSR）的具体差异处理见 `frontend-stack-map.md`。

## 后端栈类

Python / Go / Rust / Node.js 的检测信号、确定性检查命令、本地 skill 生成策略见 `backend-stack-map.md`。社区可装 skill 的具体包名**待联网核实**，不要臆造；默认策略是用 `skill-creator` 为项目生成技术栈本地 skill。

## 易混淆项

- `ai-transform-review`：评估**人 / 团队**的 AI 使用情况并推送存档，不是评估项目工程状态。用户说"评估我的 AI 能力"用它；说"评估这个项目该怎么推进"用本教练。
- `grilling`：stress-test 一个计划 / 决策，是拷问器，不做成熟度评估。可在域 1 / 2 需要逼出确定性时配合本教练使用。
