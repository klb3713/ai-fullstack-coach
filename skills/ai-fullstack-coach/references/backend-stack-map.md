# 后端栈适配（Python / Go / Rust / Node.js）

三段式：**检测信号 → 确定性检查命令 → 推荐 skill / 工具**。检测只读 manifest 文件，不联网；检查命令给出 evidence；推荐分"本地生成 skill"和"社区可装 skill"两类。

## 通用推荐策略

两条并行，按项目情况选择或组合：

1. **本地生成 skill（默认）**：用 `skill-creator` 在目标项目内生成一份技术栈本地 skill，固化该栈的框架约定、分层、测试与 lint 命令、错误处理。社区包版本与质量不稳定，本地 skill 是稳定底座。
2. **社区可装 skill（补充）**：下面按栈列出**已核实包名与安装命令**。安装后 `npx skills list` 确认实际名字；只从知名仓库安装，用 `skills-lock.json` 锁版本。

生成本地 skill 至少覆盖：项目结构约定、依赖管理方式、lint / typecheck / test 命令、错误处理约定、领域分层。

---

## Python

- **检测信号**：`pyproject.toml`、`requirements.txt`、`setup.cfg`、`Pipfile`；目录 `app/`、`src/`、测试 `tests/`。
- **确定性检查命令**：`ruff check .`（lint）、`mypy .`（类型）、`pytest`（测试）。依赖管理看是否有 `uv.lock` → 用 `uv`。
- **推荐（本地）**：python-backend skill（FastAPI 分层：路由 → 服务 → 仓储；依赖注入；pydantic 模型）。
- **推荐（社区，已核实）**：
  - `fastapi`（Pydantic v2 + SQLAlchemy async + JWT，覆盖 422/CORS/异步阻塞等坑）：`npx skills add https://github.com/jezweb/claude-skills --skill fastapi` · [repo](https://github.com/jezweb/claude-skills)
  - `python-backend`（FastAPI+SQLAlchemy+Redis、pytest、ty、Pydantic 等合集）：`npx skills add jiatastic/open-python-skills --skill python-backend` · [repo](https://github.com/jiatastic/open-python-skills)
  - 跨栈包 `@buivietphi/skill-backend-mt` 也含 FastAPI/Flask/Django 模式。

## Go

- **检测信号**：`go.mod`、`go.sum`；目录 `cmd/`、`internal/`、`pkg/`。
- **确定性检查命令**：`go vet ./...`、`go test ./...`、`golangci-lint run`。
- **推荐（本地）**：go-backend skill（包可见性、`internal/` 边界、error wrapping、表驱动测试）。
- **推荐（社区，已核实）**：
  - `go-agent-skills`（内含 `go-coding-standards`/`go-naming`/`go-error-handling`/`go-context` 等）：`npx skills add eduardo-sl/go-agent-skills` · [repo](https://github.com/eduardo-sl/go-agent-skills)
  - `cc-skills-golang`（`golang-error-handling`/`golang-lint`/`golang-naming`/`golang-safety` 等）：[repo](https://github.com/samber/cc-skills-golang)
  - 跨栈包 `@buivietphi/skill-backend-mt` 也含 Go 模式。

## Rust

- **检测信号**：`Cargo.toml`、`Cargo.lock`；目录 `src/`、`tests/`。
- **确定性检查命令**：`cargo fmt --check`、`cargo clippy -- -D warnings`、`cargo test`。
- **推荐（本地）**：rust-backend skill（所有权 / 生命周期约定、error 类型、`#[cfg(test)]` 约定）。
- **推荐（社区，已核实）**：
  - `rust-skills`（179 条规则 / 14 类，含所有权借用、错误处理、并发）：`npx add-skill leonardomso/rust-skills` · [repo](https://github.com/leonardomso/rust-skills)
  - `rust-best-practices`（Apollo GraphQL 背书）：`npx skills add https://github.com/apollographql/skills --skill rust-best-practices` · [repo](https://github.com/apollographql/skills)

## Node.js（服务端）

- **检测信号**：`package.json` + `tsconfig.json`；目录 `src/`、`dist/`；运行时线索 `[express|fastify|nestjs|koa]` 依赖。
- **确定性检查命令**：`npx tsc --noEmit`（类型）、`eslint .`（lint）、`vitest run` 或 `npm test`（测试）。
- **推荐（本地）**：node-backend skill（模块边界、异步错误处理、依赖注入）。
- **推荐（社区，已核实）**：
  - `nodejs-backend-skill`（生成 Express/NestJS + TypeScript 骨架与文档）：`npx skills add https://github.com/jiushiwon/wg-skills --skill nodejs-backend-skill-jiushiwon` · [repo](https://github.com/jiushiwon/wg-skills)
  - `nodejs-backend`（Express/NestJS/Fastify、认证、ORM、微服务等）：[repo](https://github.com/thesaifalitai/claude-setup)
  - 跨栈包 `@buivietphi/skill-backend-mt`（NestJS/Express/Fastify/Next.js 等）：`npx @buivietphi/skill-backend-mt` · [npm](https://www.npmjs.com/package/@buivietphi/skill-backend-mt)

> Node.js 客户端 / 前端侧的框架、样式、SSR 分叉见 `frontend-stack-map.md`，两处不要重复判定。
