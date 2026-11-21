# 评估操作指南

供 Step 1–2 使用：怎么探测、问什么、证据怎么算、四态怎么判、流程怎么分。

## 探测清单（按优先级，能读就读）

| 目标 | 文件 / 命令 | 供给哪个域 |
|---|---|---|
| agent 规则 | `AGENTS.md`、`CLAUDE.md`、`CLAUDE.local.md`、`.claude/rules/` | 0 |
| 治理骨架 | `doc/workflow/RUNBOOK.md`、`openspec/config.yaml`、`doc/stages/`、`README.md` | 0 / 7 |
| skill 清单 | `.claude/skills/`、`.agents/skills/` | 0 / 6 |
| 质量命令 | `package.json` scripts、`Makefile`、`pyproject.toml`、`go.mod`、`Cargo.toml` | 0 / 5 |
| 规格 / change | `doc/openspec/changes/`、`docs/specs/`、`PLANS.md` | 1 |
| UX 材料 | 设计稿、`*.md` 线框/流程图、`docs/ux/`、路由定义 | 2 |
| 设计 token | `*.css` 变量、`tailwind.config.*`、`theme.ts`、`design-system/` | 3 |
| 契约 | `openapi.yaml`/`swagger.json`、`*.d.ts`、`schemas/`、生成的 client | 4 |
| 测试 / 验证 | 测试目录、E2E 配置、MCP 配置、`lighthouserc.*`、CI 配置 | 5 / 6 / 7 |
| 工具链版本 | `openspec --version`、`node --version` | 0（治理层硬依赖） |

只读存在的路径，用 `glob`/`read`，不要因为文件缺失而报错中断。目录不存在本身就是 red 的 evidence。

## 提问策略（≤2–3 个）

先探测，后提问。只问 AI 从文件读不出来的 fog of war，按对判定的影响排序：

1. **项目性质**：这是新功能、重构、bug 修复，还是纯样式调整？（决定 Standard / Fast Track，直接影响 8 域怎么判）
2. **缺件的意图**：某个域找不到材料时——是"还没做"，还是"做了但不在仓库里（如外部设计稿）"？（决定 red 还是 ⚪）
3. **团队约定**：有没有未写入文件、但团队实际遵守的关键规则？（补全域 0 的 evidence）

一次最多 3 个。用户答"跳过/不知道"就按可见 evidence 判定，不追问。

## 证据（evidence）标准

一条合格 evidence 是以下之一：
- **存在性**：具体文件路径（"未发现 `AGENTS.md`" 可判域 0 red）。
- **内容性**：文件里的具体行（"`package.json` 无 `test` script"）。
- **命令性**：一次确定性检查的输出（"`npx tsc --noEmit` 通过" 支撑域 5 green）。
- **陈述性**：用户明确回答（标注"（用户陈述）"）。

不合格："看起来像有测试"、"应该有设计系统"——退回 ⚪ 并提问。

## 四态判定规则

- 所有 green 判据都有 evidence → 🟢。
- 主体在，但有明确列出的 gap → 🟡，gap 写进 evidence。
- 关键项缺失或无法验证 → 🔴，写清缺什么。
- 材料可能在仓库外 / 用户未答 / 无法判断 → ⚪，进「已知缺口」。

顺序：宁可标 ⚪ 也不要无证据标 green。Fast Track 下域 1–4 直接标 `⏭ 跳过（Fast Track）`，不计入成熟度。

## 流程判定（Standard vs Fast Track）

- **Fast Track**：bug 修复、样式调整、配置更新、行为不变且无 API/权限/数据语义变更、预计 ≤1 小时。跳过域 1–4。
- **Standard Flow**：新功能、复杂重构、多模块、涉及契约或数据语义变更。走全 8 域；域 4 契约不 green 时，下一步锁定为"先补域 4"，禁止向域 5/6 冒进。
- 拿不准时按 **Standard Flow** 处理，并把"是否 Fast Track"作为第 1 个问题问用户。

## 治理层硬检查（仅域 0）

`stage-openspec-skills` 的三个 skill 要求 OpenSpec **2.1.0** + Node.js **≥22.20.0**。探测到版本不符时，在报告里标注为域 0 的 gap，并推荐 `stage-openspec-init`（会先检测、必要时才安装）。不要在版本不符时推荐直接进入 change 流程。
