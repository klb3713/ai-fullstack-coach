# 前端适配：能力域为主 + 三处分叉轻量适配

前端**不按框架做完整切分**。工具链（TS + Vite + Vitest + ESLint）跨框架趋同，前端 AI 开发的瓶颈在设计层（域 2/3/6/7）而非工具链。因此：主体是 4 个框架无关的设计能力（域 2a/2b/3/6/7，下面「主体」），只在 3 个真正影响推荐的分叉点做轻量适配（下面「分叉」）。社区包名核实状态见 `skill-map.md`。

## 主体：4 个框架无关的设计能力

| 能力域 | 推荐方向 | 来源 |
|---|---|---|
| 2a 全局 UX 骨架 / 2b 局部 UX | 状态清单（empty/loading/error/success）、ASCII 线框、`grilling` 拷问取舍 | 宿主 catalog + 通用 |
| 3 UI 设计系统 | `frontend-design`（anti-slop + token 级设计计划）、性能预算 skill | 已核实 |
| 6 验证闭环 | DevLens / PageLens 视觉反馈 MCP、Lighthouse 预算 | 已核实 |
| 7 加固 | 组件测试（Testing Library 类）、E2E（Playwright） | 通用 |

这四条对所有前端栈一致，不要因框架不同而重复推荐。

## 分叉：读 `package.json` 依赖，三处变频

检测只读 `package.json` 的 dependencies / devDependencies，不联网。命中哪个就叠加哪条建议；可同时命中多个。

### 分叉 1 — 框架

| 检测信号 | 影响的推荐 |
|---|---|
| `react` | 组件约定 + 测试库 `@testing-library/react` |
| `vue` | 组件约定 + `@vue/test-utils` |
| `svelte` | 组件约定 + 对应测试库 |
| `@angular/core` | Angular 模块 / 依赖注入约定 + TestBed |

只影响「组件写法约定」和「测试库选择」，不改主体四条。

### 分叉 2 — 样式方案

| 检测信号 | 影响的推荐 |
|---|---|
| `tailwindcss` | token 落到 `tailwind.config.*` 的 theme 扩展，短 class 约定 |
| `daisyui` | 推荐 daisyUI skill 的短 class（如 `btn btn-primary`），语义 class 一致性 |
| `styled-components` | token 落到 ThemeProvider / CSS 变量 |
| 纯 CSS / CSS Modules | token 落到 `:root` CSS 变量 |

只影响「语义 token 的落地方式」，域 3 的 green 判据不变。

### 分叉 3 — SSR

| 检测信号 | 影响的推荐 |
|---|---|
| `next` | 数据获取（Server Component / `getServerSideProps`）与渲染约定 |
| `nuxt` | Nuxt 数据获取（`useFetch`）与渲染约定 |
| `@sveltejs/kit` | SvelteKit load 函数与渲染约定 |
| 无（纯 SPA） | 客户端数据获取与路由约定，跳过 SSR 项 |

只影响「数据获取与渲染约定」，不改变验证与设计能力。

## 判定纪律

- 先判 4 个主体能力域，再按命中的分叉补推荐。主体未覆盖时，不要因为识别出某个框架就跳过设计能力域的判定。
- 分叉不改变能力域的 green 判据，只改变"用什么具体约定达成 green"。
- 社区包名只有核实过的才写具体名字；未核实的作为可搜索方向提示。
