# 更新文档

## 1.0.0（2026-10-27）

- :boom: (style) 导入按 PascalCase、camelCase、`UPPER_SNAKE_CASE` 分组，值导入位于 `import type` 之前，单行导入按整行长度从长到短；集中导出先区分单行与多行，再按命名风格分组，同组单行导出按整行长度从长到短
- :sparkles: (code) 新增源码按类型、常量、文件内函数、导出函数排列，函数体按读取、解析、派生、校验、构造和副作用分段，段间保留一个空行；组件不依赖页面实现，hook 不依赖页面，util 不依赖 React 组件和请求层，纯展示组件不绕过 hook 或 api 直接请求
- :bug: (architecture) 已有 Next 路由目录时，新的 HTTP 接口放进该目录下的 `api/`，App Router 使用 `route.ts`，Pages Router 使用路由段文件名；源码根的 `api/` 只放可 import 的接口函数，路由目录下的 `api/` 不作为请求目录，也不建入口再导出
- :recycle: (mxcat-code-standards) 规范按任务拆到独立规则文件，入口只标明读取时机；无特殊要求时只约束本次新增代码，项目里已有的风格继续沿用
- :recycle: (directory) 新代码按职责放进同一层已有的同类目录；多处复用的放进既有共享层，只服务一个页面或功能的就近放置，页面入口留在既有页面层或路由层
- :recycle: (naming) 项目没有稳定命名风格时，组件和类型用 PascalCase，函数和变量用 camelCase，模块常量用 `UPPER_SNAKE_CASE`，自定义 Hook 用 `useXxx`，新建目录用全小写连字符

## 0.1.0（2026-10-19）

- :tada: (mxcat-code-standards) 发布可安装技能，约束新写 TypeScript、JavaScript、JSX、TSX 的目录落点、命名、导入导出和函数 JSDoc，可用 `npx skills` 安装到 Cursor、Claude Code
- :books: (openspec) 写入架构约束与代码风格规格，并归档目录约束和技能瘦身两条变更
- :wrench: (cursor) 加入 OpenSpec 工作流命令与技能，用于在本仓库推进变更
