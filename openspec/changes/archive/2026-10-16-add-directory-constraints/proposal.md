## Why

仓库里还没有同类目录时，现有规范不规定默认目录名，新代码的落点不稳定。同时规范允许在碰到旧文件时整理风格、拆分职责，Agent 会顺手改写用户没有点名的已有代码。

## What Changes

- **BREAKING** 本 skill 只约束本次新写的代码。为接上新代码而必须改动的旧文件，只加接线所需的那几行。不搬迁旧文件，不拆旧函数，不重排已有导入，不给旧函数补注释。
- 某一层还没有承担该职责的目录时，使用默认目录名。这一层已经有别的名字时，新代码跟着旧目录，不另开一个职责重复的目录。
- `components/`、`hooks/`、`utils/`、`types/`、`styles/`、`constants/` 按共享范围创建：全项目、一端（`web/` 或 `mobile/`）、或单个页面或功能。
- 新的接口放进源码根的 `api/`。`api/` 只放接口函数。仓库已有请求目录时不另建 `api/`。`services/`、`layouts/`、`store/` 不纳入约束。
- 代码里 import 的图片、音频、字体、SVG 放进源码根的 `assets/`。favicon、robots.txt 放进仓库根的 `public/`。裸 `.sql` 放进仓库根的 `db/`。该层已有同类目录时跟着那个目录。
- 非 Next 项目的新页面放进 `containers/`，路由表放进 `routes/`。该层已有页面目录或路由目录时跟着那个目录。Next 项目（`package.json` 有 `next`，且已有 `app/`、`pages/`、`src/app/` 或 `src/pages/`）不新建这两个目录，页面跟着已有路由目录。
- 已经有稳定命名风格时，新增文件沿用；没有时，新建目录用全小写，多个词用 `-` 连接。代码文件名不套这条。Next 路由里的路由组 `(group)`、动态段 `[id]`、`[...slug]`、`[[...slug]]`、平行路由 `@slot`、拦截路由 `(.)`、`(..)`、`(..)(..)`、`(...)`、私有目录 `_folder` 和 `%5F` 保持框架写法。`page.tsx`、`layout.tsx`、`loading.tsx`、`error.tsx`、`not-found.tsx`、`template.tsx`、`default.tsx`、`route.ts` 保持框架文件名。
- 同一层已经有稳定的文件命名风格时，新增文件沿用。这一层还没有时：组件和页面组件的文件名、导出名用 PascalCase，且两者相同；自定义 Hook 的文件名和导出名用 `useXxx`，且两者相同；工具函数和只导出一个函数的接口文件用 camelCase，且两者相同；同一资源的多个接口函数放在一个 camelCase 文件里；类型和常量文件用 camelCase，类型导出用 PascalCase，常量导出用 `UPPER_SNAKE_CASE`。图片、音频、字体、SVG 和 `.sql` 用全小写，多个词用 `-` 连接。
- 新代码按实际情况拆分。行数长但职责单一时不拆。不因已有文件多职责或多段流程而拆开它。不规定组件、hook、util、`api/` 的调用方向，也不规定样式写法。

## Capabilities

### New Capabilities

- 无。

### Modified Capabilities

- `architecture-constraints`: 启用范围收成只约束新代码；补上缺省目录名、共享范围、`api/`、资源与 SQL 的落点，以及 Next 与双端的分支。拆分只保留 skill 里的边界。没有稳定命名风格时，新建目录用全小写，多个词用 `-` 连接。代码文件在同层没有既有风格时按导出的标识符命名，且文件名与导出名相同。静态资源和 `.sql` 用全小写加 `-`。去掉分层、依赖方向和样式写法。
- `code-style`: 导入、导出和注释只应用于本次新写的代码。导入顺序与 skill 一致。`@param` 和 `@return` 不要求写类型。去掉空行和 Promise 的条文。

## Impact

- 发布物仍是 `skills/mxcat-code-standards/SKILL.md`。
- 不改目标仓库的已有文件，不引入新的格式化依赖。
