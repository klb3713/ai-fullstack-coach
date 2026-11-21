# ai-fullstack-coach

一套**教练 / 导航型** skill：评估 AI 驱动全栈项目的当前状态，按最佳实践引导开发者逐步推进，并在每个阶段推荐合适的 skill。

它不写业务代码，不替开发者做架构决策。执行交给它推荐的专业 skill。

## 它解决什么

AI 全栈开发的瓶颈从"写代码"转移到了两件事：**说清楚要什么** 和 **验证做对了没**。这套 skill 把最佳实践拆成 8 个可评估的能力域，让项目现状、缺口和下一步一目了然，并且跨会话留存。

双层模型——共用的基础设施前置建一次，feature 特有的随循环反复：

```
底座层（项目级，建一次）：
  0 上下文与约束 → 2a 全局 UX 骨架 → 3 UI 设计系统 → 4a 契约规范
         ↓（底座就绪后，每个 feature 反复走）
循环层（feature 级）：
  1 需求 → 2b 局部 UX → 4b 契约实例(tracer bullet) → 5 并行实现 → 6 验证闭环 → 7 加固交付
```

UI 设计系统和全局 UX 骨架属于底座、先于具体 feature：后置会导致每个 feature 各写一套、语义 token 失效。需求本身也分层——全局产品定义先定（喂底座），局部需求逐 feature 明细。

- **Standard Flow**：先补底座层 red，再走循环层；域 4 契约不 green 时禁止向实现冒进。
- **Fast Track**（bug 修复 / 样式 / ≤1h）：跳过循环层的域 1 / 2b / 4b，底座层仍需满足；evidence 记录与存档更新不跳过。

## 目录

```
ai-fullstack-coach/
├── README.md
└── skills/ai-fullstack-coach/
    ├── SKILL.md                       # 主流程：探测→判定→报告→存档→指针→下一步
    ├── references/
    │   ├── capability-areas.md        # 8 能力域定义与 green 判据
    │   ├── assessment-guide.md        # 探测清单、提问策略、证据标准、四态规则
    │   ├── skill-map.md               # 能力域→推荐 skills（含 OpenSpec 三件套替换）
    │   ├── backend-stack-map.md       # Python/Go/Rust/Node.js 检测→检查→推荐
    │   └── frontend-stack-map.md      # 能力域为主 + 框架/样式/SSR 三分叉
    └── assets/
        └── report-template.md         # docs/ai-assessment.md 存档骨架
```

## 安装

把 `skills/ai-fullstack-coach/` 整个目录复制到目标项目的 skills 目录：

- Claude Code：`.claude/skills/ai-fullstack-coach/`
- Codex：`.agents/skills/ai-fullstack-coach/`

宿主需支持 Agent Skills 标准（skill = 带 frontmatter 的 `SKILL.md` + 同级 references/assets）。

## 使用

在宿主中自然语言触发，例如：

- 「评估一下这个项目的 AI 开发状态」
- 「我该按什么顺序推进这个项目」
- 「下一步该用什么 skill」

触发后它会：

1. 自动探测项目文件 + 最多问 2–3 个关键问题；
2. 逐能力域判定（🟢/🟡/🔴/⚪，每条带 evidence）；
3. 输出对话报告；
4. 写入目标项目的 `docs/ai-assessment.md`；
5. 询问后，在 `AGENTS.md` 加一行指向存档的指针；
6. 给出按优先级排序的下一步与推荐 skills。

## 依赖与内置关系

- **OpenSpec 治理三件套**：`stage-openspec-init`、`stage-project-bootstrap`、`stage-change-workflow`（用户自建 `stage-openspec-skills`，repo-local）。本教练**用它们替换**通用 `openspec-*` 系列。硬依赖：OpenSpec 2.1.0 + Node.js ≥22.20.0。
- **宿主 catalog skill**：`writing-for-agents`、`skill-creator`、`grilling` 可直接调用。
- **前端设计 skill**：`frontend-design`、`theme-factory`、`brand-guidelines`、`web-design-guidelines`、`react-best-practices`、`vercel-composition-patterns`、`ui-ux-pro-max`、`@ctxr/skill-frontend-excellence`、daisyUI skill、DevLens / PageLens MCP（包名与安装命令见 `skill-map.md`）。
- **社区包名**：已联网核实，前端官方套装（anthropics/skills、vercel-labs/agent-skills）与 Python/Go/Rust/Node.js 各一个以上是真实包名，附仓库 URL 与安装命令，见 `skill-map.md` / `backend-stack-map.md`。仅少数索引站条目（design-system、impeccable 等）仍标"待核实"，推荐时装前确认。

## 术语

- **能力域（0–7）**：本教练的成熟度维度。
- **Stage**：`stage-openspec-skills` 的治理层交付容器。两者同名不同义，不混用。
- **ai-transform-review**：评估人 / 团队的 AI 使用情况，不是评估项目——不要与本项目教练混淆。
