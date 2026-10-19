## 1. 启用范围

- [x] 1.1 改写 `skills/mxcat-code-standards/SKILL.md` 的启用段：只约束本次新写的代码；旧文件只加接线所需的导入和使用；不搬迁旧文件、不拆旧函数、不重排已有导入、不给旧函数补注释
- [x] 1.2 更新 frontmatter `description`，写明本 skill 约束的是新写的 TypeScript / JavaScript / JSX / TSX，并且只执行本文件列出的约束

## 2. 架构落点

- [x] 2.1 复用改为搜索已有函数、组件、hook 和请求函数；小幅扩展时只改本次需要的部分
- [x] 2.2 写入目录规则：同一层已有同类目录就放进去，不另开缺省名；没有时 `components/`、`hooks/`、`utils/`、`types/`、`styles/`、`constants/` 按全项目、已存在的 `web/` 或 `mobile/`、页面旁边三档放置；根上的旧名字不传到下一层
- [x] 2.3 写入固定落点：接口进源码根 `api/`，且 `api/` 只放接口函数；已有请求目录不另建 `api/`；import 的图片、音频、字体、SVG 进 `assets/`，favicon 与 robots.txt 进仓库根 `public/`，`.sql` 进仓库根 `db/`；该层已有同类目录时跟着那个目录。不规定 `services/`、`layouts/`、`store/`
- [x] 2.4 写入页面规则：非 Next 用 `containers/` 和 `routes/`；该层已有页面目录或路由目录时跟着那个目录；`package.json` 有 `next` 且已有 `app/`、`pages/`、`src/app/` 或 `src/pages/` 时，新页面进已有路由目录；仓库还没有 `web/`、`mobile/` 时不新建
- [x] 2.5 拆分写成：新代码按实际情况拆分，并保持执行顺序、错误处理、兜底和副作用时机；行数长但职责单一时不拆；不因已有文件多职责或多段流程而拆开它；不为了拆新建没有职责边界的目录。不写分层和依赖方向
- [x] 2.6 写入目录命名：已经有稳定命名风格时新增文件沿用；没有时新建目录用全小写，多个词用 `-` 连接；文件名不套这条；Next 路由里的路由组 `(group)`、动态段 `[id]`、`[...slug]`、`[[...slug]]`、平行路由 `@slot`、拦截路由 `(.)`、`(..)`、`(..)(..)`、`(...)`、私有目录 `_folder` 和 `%5F` 保持框架写法

## 3. 代码风格作用面

- [x] 3.1 在代码段写明导入、导出和注释只覆盖新写的代码；旧文件里新增的导入按分组插入，原有导入保持原样
- [x] 3.2 通读 `SKILL.md`：导入分组和 JSDoc 示例在；空行和 Promise 只有作用范围、没有条文；正文没有把 `services/`、`layouts/`、`store/` 写成必选目录

## 4. 文件命名与样式

- [x] 4.1 写入文件命名：同层已有风格则沿用；没有时组件和页面组件用 PascalCase，Hook 用 `useXxx`，工具函数和单函数接口用 camelCase，且这几类的文件名与导出名相同；同一资源的多个接口放在一个 camelCase 文件，类型和常量文件用 camelCase、类型导出用 PascalCase、常量导出用 `UPPER_SNAKE_CASE`；图片、音频、字体、SVG 和 `.sql` 用全小写加 `-`；Next 的 `page.tsx`、`layout.tsx`、`loading.tsx`、`error.tsx`、`not-found.tsx`、`template.tsx`、`default.tsx`、`route.ts` 保持框架文件名
- [x] 4.2 样式写法不写入 skill。`styles/` 只作为共享范围里的目录名，出现在落点和导入顺序中
