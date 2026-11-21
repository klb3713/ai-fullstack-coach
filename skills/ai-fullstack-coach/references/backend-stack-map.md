# 后端栈适配（Python / Go / Rust / Node.js）

三段式：**检测信号 → 确定性检查命令 → 推荐 skill / 工具**。检测只读 manifest 文件，不联网；检查命令给出证据；推荐分"本地生成 skill"和"社区可装 skill"两类。

## 通用推荐策略

对每种栈，优先用 `skill-creator` 在目标项目内生成一份**技术栈本地 skill**（把该栈的框架约定、分层、测试与 lint 命令固化下来），因为社区 skill 的版本与质量不稳定。社区可装 skill 的具体包名**待联网核实**：联网恢复后搜索确认，再写入报告；在此之前只作为"可搜索方向"提示，不写确定的名字。

生成本地 skill 时至少覆盖：项目结构约定、依赖管理方式、lint / typecheck / test 命令、错误处理约定、领域分层。

---

## Python

- **检测信号**：`pyproject.toml`、`requirements.txt`、`setup.cfg`、`Pipfile`；目录 `app/`、`src/`、测试 `tests/`。
- **确定性检查命令**：`ruff check .`（lint）、`mypy .`（类型）、`pytest`（测试）。依赖管理看是否有 `uv.lock` → 用 `uv`。
- **推荐**：本地 python-backend skill（FastAPI 分层：路由 → 服务 → 仓储；依赖注入约定；pydantic 模型）。社区 skill 包名待联网核实。

## Go

- **检测信号**：`go.mod`、`go.sum`；目录 `cmd/`、`internal/`、`pkg/`。
- **确定性检查命令**：`go vet ./...`、`go test ./...`、`golangci-lint run`。
- **推荐**：本地 go-backend skill（包可见性约定、`internal/` 边界、error wrapping、表驱动测试）。社区 skill 包名待联网核实。

## Rust

- **检测信号**：`Cargo.toml`、`Cargo.lock`；目录 `src/`、`tests/`。
- **确定性检查命令**：`cargo fmt --check`、`cargo clippy -- -D warnings`、`cargo test`。
- **推荐**：本地 rust-backend skill（所有权 / 生命周期约定、error 类型、`#[cfg(test)]` 约定）。社区 skill 包名待联网核实。

## Node.js（服务端）

- **检测信号**：`package.json` + `tsconfig.json`；目录 `src/`、`dist/`；运行时线索 `[express|fastify|nestjs|koa]` 依赖。
- **确定性检查命令**：`npx tsc --noEmit`（类型）、`eslint .`（lint）、`vitest run` 或 `npm test`（测试）。
- **推荐**：本地 node-backend skill（模块边界、异步错误处理、依赖注入）。社区 skill 包名待联网核实。

> Node.js 客户端 / 前端侧的框架、样式、SSR 分叉见 `frontend-stack-map.md`，两处不要重复判定。
