# AGENTS.md

This file provides guidance to Qoder (qoder.com) when working with code in this repository.

## 项目概述

Dev Tools — 面向开发者和IT从业者的在线工具集合。包含百余种实用工具（加密、格式转换、网络计算、文本处理等），支持中英文双语，可通过 Docker 自托管。

## 常用命令

```bash
pnpm dev                 # 启动开发服务器 (Vite)
pnpm build               # 类型检查 + 生产构建 (vue-tsc --noEmit + vite build)
pnpm preview             # 预览生产构建 (端口 5050)

pnpm test:unit           # 运行单元测试 (vitest + jsdom)
pnpm test:e2e            # 运行 e2e 测试 (Playwright)
pnpm test:e2e:dev        # 针对本地开发服务器运行 e2e 测试 (不启动 web server)
pnpm coverage            # 运行测试并生成覆盖率报告

pnpm lint                # ESLint 代码检查 (src 目录)
pnpm typecheck           # 仅类型检查 (不构建)

pnpm script:create:tool  # 在 src/tools/ 下快速创建新工具 (node scripts/create-tool.mjs)
pnpm script:create:ui    # 通过 hygen 生成 UI 组件
```

## 核心技术栈

- **Vue 3.3** + **Vite 4.4** + **TypeScript 5.2**
- **Naive UI 2.35** — UI 组件库
- **UnoCSS** — 原子化 CSS 引擎（支持 Less 做 scoped 样式）
- **Pinia** — 状态管理
- **vue-i18n 9.9** — 国际化（动态编译模式，JIT 编译）
- **pnpm 9.11** — 包管理器

## 架构与路由

### 工具系统（核心）

工具是自包含的模块，位于 `src/tools/<tool-name>/`。每个工具目录包含：

| 文件 | 作用 |
|---|---|
| `index.ts` | 使用 `defineTool()` 导出 `tool` 对象（名称、路径、图标、懒加载组件） |
| `<tool-name>.vue` | 工具的主 Vue 组件（懒加载） |
| `<tool-name>.service.ts` | (可选) 纯业务逻辑，可测试 |
| `<tool-name>.service.test.ts` | (可选) 业务逻辑单元测试 |
| `<tool-name>.e2e.spec.ts` | (可选) Playwright e2e 测试 |

**创建新工具的步骤：**
1. 运行 `pnpm script:create:tool <tool-name>` 在 `src/tools/` 下创建目录和文件框架
2. 在 `src/tools/index.ts` 中手动将新工具添加到对应的 `toolsByCategory` 分类中
3. (可选) 在 `locales/` 目录的各个 locale YAML 文件中添加翻译条目

### 路由

路由由工具列表自动生成（`src/router.ts`）。每个定义好的 tool 根据其 `path` 生成一个路由条目，页面使用 `ToolLayout`（在 `src/layouts/index.ts` 中注册）。首页和关于页面是手动定义的。

### 布局层级

- **BaseLayout** — 侧边栏（工具菜单）+ 顶部导航（搜索、语言切换、深色模式）+ `<slot>` 内容
- **ToolLayout** — 继承 BaseLayout，为工具页面添加标题/描述头部

### `defineTool()` 工厂函数

位于 `src/tools/tool.ts`，用于声明工具元信息。支持的字段：
- `name` / `path` / `description` / `keywords` — 元信息与 SEO
- `component: () => import(...)` — 懒加载的 Vue 组件
- `icon` — 来自 `@vicons/tabler` 的图标组件
- `redirectFrom` — (可选) 用于兼容旧 URL 的重定向
- `createdAt` — (可选) Date 对象，设置为最近两周内则自动标记 `isNew`

名称和描述支持 i18n，有两种方式：
- 直接传入字符串（无翻译）
- 使用 `translate('tools.<key>.title')` 从 locales 文件中读取翻译

### 国际化 (i18n)

- 翻译文件位于 `locales/<lang>.yml`，通过 `@intlify/unplugin-vue-i18n/vite` 在构建时动态编译
- 工具翻译的 key 模式：`tools.<toolname>.title` 和 `tools.<toolname>.description`
- 工具名来源于 `path` 去掉斜杠后的部分（例如 `/uuid-generator` → `uuid-generator`）
- 分类名称的 key：`tools.categories.<category-name-lowercase>`

### 自动导入

通过 `unplugin-auto-import` 自动导入，无需手动引入即可使用：
- **Vue API**：`ref`, `computed`, `watch`, `onMounted` 等
- **vue-router**：`useRoute`, `useRouter`
- **@vueuse/core**：`useStorage`, `syncRef` 等
- **vue-i18n**：`useI18n`
- **Naive UI 组合式函数**：`useDialog`, `useMessage`, `useNotification`, `useLoadingBar`

`unplugin-vue-components` 负责自动注册 Naive UI 组件和 `src/ui/` 目录下的自定义组件（命名模式：`<c-alert>`, `<c-button>`, `<c-card>` 等）。

### 状态管理

- `useToolStore` (Pinia) — 管理工具列表、收藏夹、分类。收藏使用 localStorage 持久化。
- `useStyleStore` (Pinia) — 管理深色/浅色主题和菜单折叠状态。

### 配置

`src/config.ts` 使用 `figue` 库进行类型安全的配置，支持通过环境变量覆盖。主要配置项：`app.version`, `app.baseUrl`, `app.env`, `plausible.*`。

### 关键依赖

- **Monaco Editor** (`@monaco-editor`) — 用于 JSON Diff 等工具的代码编辑器
- **TipTap** — 所见即所得 HTML 编辑器
- **`crypto-js`**, **`bcryptjs`**, **`node-forge`**, **`uuid`** — 加密相关
- **`sql-formatter`**, **`xml-formatter`** — 格式化工具
- **`netmask`** — IP 子网计算
- **`libphonenumber-js`** — 电话号码解析

## 代码约定

- 非单文件组件的目录使用 kebab-case 命名
- 工具级组件（`src/tools/<name>/`）的目录名称同时作为 URL 路径（`/<name>`）
- 工具内业务逻辑尽量抽到 `.service.ts` 文件中，保持 `.vue` 为纯 UI
- 所有工具使用 `ToolLayout` 包裹（通过路由 meta 自动设置）
