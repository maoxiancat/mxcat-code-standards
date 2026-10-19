## Context

见 `proposal.md` 的 Why。发布物是 `skills/mxcat-code-standards/SKILL.md`。本设计记录这份正文里已经生效的决定。

## Goals / Non-Goals

**Goals:**

- 启用段写成：只约束本次新写的代码；旧文件只接受接线。
- 架构段写出缺省目录、共享范围、`api/`、资源与 SQL、Next 与双端。
- 已经有稳定命名风格时，新增文件沿用。没有时，新建目录用全小写，多个词用 `-` 连接。代码文件名和 Next 框架路由段、框架文件名除外。
- 同层没有既有文件命名风格时，文件名跟导出的标识符走，且文件名与导出名相同。静态资源和 `.sql` 用全小写加 `-`。
- 拆分只写正文里的边界：按实际情况拆，行数长但职责单一时不拆，已有文件不拆。
- 代码段保留导入分组和 JSDoc，作用范围收成新写的代码。导入顺序与正文一致。

**Non-Goals:**

- 不把规范拆进 `references/`。
- 不规定 `services/`、`layouts/`、`store/`。
- 不规定组件、hook、util、`api/` 的调用方向。
- 不规定样式写法，包括 Tailwind、`.scss` 和 `@apply`。
- 不规定空行怎么分段，也不规定 Promise 的 `async` 和 `return await`。
- 不要求目标仓库搬迁已有文件。

## Decisions

### 缺省名只补该层的空缺

同一层已有同类目录时，新代码进去，不用缺省名再开一个。该层没有时才用缺省名。

另一条路是无论仓库现状都新建缺省目录。那会和「不在同层再建一个职责重复的目录」冲突，所以不采用。

### 共享范围只有三档

全项目用源码根，一端用已存在的 `web/` 或 `mobile/`，单个页面或功能放在旁边。`components/`、`hooks/`、`utils/`、`types/`、`styles/`、`constants/` 都按这三档。仓库还没有两端目录时，不为此创建 `web/`、`mobile/`。

根上的旧名字不带到下一层。源码根的工具在 `helpers/`、而 `web/` 下还没有工具目录时，仅 web 使用的新工具放进 `web/utils/`。

另一条路是把根上的叫法继承下去，变成 `web/helpers/`。按「只认这一层已经存在的目录」定的，所以不继承。

### 接口集中在一个 api

接口函数放在源码根的 `api/`。`api/` 只放接口函数。仓库里已经有请求目录时，放进那个目录，不另建 `api/`。`api/` 不按端再拆。

### 新建目录用全小写

已经有稳定命名风格时，新增文件沿用。没有时，新建目录用全小写，多个词用 `-` 连接，例如 `invoice-list/`。这条不约束代码文件名。

这一层还没有文件命名样本时，文件名跟主导出走，且文件名去掉扩展名后与导出名相同：组件和页面组件是 `OrderSummary.tsx` 导出 `OrderSummary`，Hook 是 `useInvoiceList.ts` 导出 `useInvoiceList`，工具函数和只导出一个函数的接口是 `formatAmount.ts`、`getInvoiceList.ts`。同一资源的多个接口函数放在 `invoice.ts`。类型和常量文件用 camelCase，类型导出用 PascalCase，常量导出用 `UPPER_SNAKE_CASE`。

图片、音频、字体、SVG 和 `.sql` 不是代码标识符，用全小写加 `-`，例如 `invoice-empty.svg`、`create-invoice.sql`。

另一条路是代码文件也全小写加 `-`。那会把 `useInvoiceList.ts` 写成 `use-invoice-list.ts`，所以不采用。

Next 路由里的路由组 `(group)`、动态段 `[id]`、`[...slug]`、`[[...slug]]`、平行路由 `@slot`、拦截路由 `(.)`、`(..)`、`(..)(..)`、`(...)`、私有目录 `_folder` 和 `%5F` 保持框架写法。`page.tsx`、`layout.tsx`、`loading.tsx`、`error.tsx`、`not-found.tsx`、`template.tsx`、`default.tsx`、`route.ts` 保持框架文件名。

### Next 要两个条件一起成立

`package.json` 的依赖里有 `next`，并且仓库已经有 `app/`、`pages/`、`src/app/` 或 `src/pages/`。`create-next-app` 勾选 `src/` 时路由在 `src/app` 或 `src/pages`。这时新页面进已有的那个路由目录，不建 `containers/`、`routes/`。只有依赖、还没有这些路由目录时，仍走非 Next 的缺省名。非 Next 时，该层已有页面目录或路由目录就跟着那个目录。

### 入口只覆盖点名的目录

`components/`、`hooks/`、`utils/`、`types/`、`constants/`、`api/` 从 `index.ts` 再导出。`index.ts` 只做再导出。其他目录从该入口导入，不指向内部文件。同一目录的内容统一导入。已有别的入口文件时，新导出加进那个文件。

### 风格规则保留导入和注释

导入分组和 JSDoc 保留，作用范围收成新写的代码。导入顺序是 `types`、`containers`、`components`、`utils`、`api`、`hooks`、`assets`、`constants`、`styles`、`routes`。`@param` 写这个参数在这里做什么，`@return` 写返回的内容，不要求写类型。

空行和 Promise 在代码段开头被点名，正文没有对应条文，所以规格不要求它们。

### 仍然只改一个技能文件

变更落在 `skills/mxcat-code-standards/SKILL.md` 的启用、架构、代码三段。不新增引用文件。

## Risks / Trade-offs

- [接线被做成整文件整理] → 启用段放在正文最前，并写一条只加导入和使用的反例。
- [同一层出现两套目录名] → 架构段先写「该层已有目录就进去」，缺省名放在后面。
- [把埋点写进 api] → 明文写清 `api/` 只放接口函数。
- [把代码文件名也改成小写加 `-`] → 明文写清目录规则不约束代码文件，并给出按导出命名的缺省。
- [把 Next 动态段或 `page.tsx` 改成普通名字] → 点名路由组和框架文件名保持框架写法。
- [把文件名和导出名写成同一风格但不是同一个名字] → 组件、页面组件、Hook、工具函数和单函数接口写明两者相同。

## Migration Plan

改 `SKILL.md` 的启用、架构和代码三段。回退时恢复该文件。目标仓库的已有代码不动。

## Open Questions

无。
