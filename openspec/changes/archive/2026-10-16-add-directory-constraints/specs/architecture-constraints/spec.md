## MODIFIED Requirements

### Requirement: 仅在结构变化时启用架构约束
变更会新增文件、新增职责或新增公共出口时，skill MUST 先为本次新写的代码执行架构约束。skill MUST NOT 为了架构约束而修改、搬迁或拆分已有代码。已有文件只为接上新代码而改动时，Agent MUST 只加入接线所需的改动。变更只修改现有函数内部、且没有新增职责时，skill MUST 跳过架构约束。

#### Scenario: 新增文件
- **WHEN** Agent 新增源码文件或新的公共出口
- **THEN** Agent 在写实现之前先执行架构约束

#### Scenario: 局部修改
- **WHEN** Agent 只修改现有函数内部的逻辑，且没有新增职责
- **THEN** Agent 不因架构约束而移动代码、拆分文件或新建目录

#### Scenario: 旧文件接上新代码
- **WHEN** 已有页面需要渲染本次新增的组件
- **THEN** Agent 只在该页面加入导入和使用，不搬迁该页面，也不拆分其中已有的函数

### Requirement: 先复用已有实现
新增函数、组件、hook、请求函数或依赖之前，Agent MUST 先搜索项目中是否已有同类实现。现有实现经过小幅扩展就能满足时，Agent MUST 扩展它。扩展时 Agent MUST 只改满足本次需求所需的部分。

#### Scenario: 已有 helper 可以扩展
- **WHEN** 项目里已有 helper，小幅扩展后就能满足需求
- **THEN** Agent 扩展该 helper 中本次需要的部分，不另写一份实现，也不改写与本次需求无关的代码

### Requirement: 沿用仓库目录
Agent MUST 把新代码放进同一层已经承担同类职责的目录。Agent MUST NOT 在同一层再建一个职责重复的目录。同一层还没有同类目录时，Agent MUST 使用缺省目录。

#### Scenario: 已有请求层
- **WHEN** 仓库里已经有承载请求和协议适配的目录
- **THEN** 新的请求和协议适配代码放进该目录，不另建 `api/`

#### Scenario: 逻辑只服务一个功能
- **WHEN** 逻辑只服务一个页面或一个功能
- **THEN** Agent 把它放在该页面或功能旁边

#### Scenario: 逻辑被多处复用
- **WHEN** 逻辑被多个页面、功能或领域复用，且该层已有共享目录
- **THEN** Agent 把它放进该共享目录

#### Scenario: 根上的工具目录不叫 utils
- **WHEN** 源码根的工具函数已经放在其他目录，且新的工具函数被全项目复用
- **THEN** 新的工具函数放进该目录，不另建 `utils/`

### Requirement: 一个文件一个主职责
本次新写的代码 MUST 根据实际情况进行拆分，并保持原来的执行顺序、错误处理、兜底和副作用时机，不改变原有逻辑。函数只是行数长、职责仍然单一时，Agent MUST NOT 拆分。Agent MUST NOT 因为已有文件承担了多类职责或叠了多段流程而拆开该文件。Agent MUST NOT 为了拆而新建没有职责边界的目录。

#### Scenario: 长函数职责仍然单一
- **WHEN** 函数很长，但职责单一且结构清楚
- **THEN** Agent 保持该函数不动

#### Scenario: 已有页面已经混合多种职责
- **WHEN** 已有页面同时包含数据获取、列定义、表单状态、弹窗状态和展示标记，且本次只在其中加入一处新展示
- **THEN** Agent 不拆开该页面文件

### Requirement: 命名跟层走
某一层已经有稳定的文件命名风格时，该层的新文件 MUST 沿用该风格。这一层还没有时，Agent MUST 按下面的缺省命名。组件和页面组件的文件名与导出名 MUST 使用 PascalCase，并且两者相同。自定义 Hook 的文件名和导出函数名 MUST 使用 `useXxx`，并且两者相同。工具函数，以及只导出一个函数的接口文件，文件名和导出函数名 MUST 使用 camelCase，并且两者相同。同一资源的多个接口函数 MUST 放在一个 camelCase 文件中。类型文件和常量文件 MUST 使用 camelCase。类型导出 MUST 使用 PascalCase。常量导出 MUST 使用 `UPPER_SNAKE_CASE`。被源码 import 的图片、音频、字体、SVG，以及 `.sql` 文件，MUST 使用全小写，多个词 MUST 用 `-` 连接。

Next 的 `page.tsx`、`layout.tsx`、`loading.tsx`、`error.tsx`、`not-found.tsx`、`template.tsx`、`default.tsx`、`route.ts` MUST 保持框架文件名。

#### Scenario: 新增组件
- **WHEN** Agent 在还没有组件文件的 `components/` 中新增订单摘要组件
- **THEN** 文件名为 `OrderSummary.tsx`，导出名为 `OrderSummary`

#### Scenario: 同层文件已有另一种风格
- **WHEN** `components/` 里的文件已经稳定使用全小写加 `-`，且本次新增一个组件文件
- **THEN** 新文件沿用该风格

#### Scenario: 同一资源的多个接口
- **WHEN** Agent 为发票新增列表和详情两个接口函数，且 `api/` 还没有文件命名样本
- **THEN** 两个函数放在 `invoice.ts`

#### Scenario: 新增静态资源
- **WHEN** Agent 新增一张被源码 import 的空状态图
- **THEN** 文件名为 `invoice-empty.svg`

#### Scenario: Next 页面文件
- **WHEN** Agent 在 Next 的 `app/invoice-detail/` 中新增页面文件
- **THEN** 文件名为 `page.tsx`

### Requirement: 跨目录走公共入口
`components/`、`hooks/`、`utils/`、`types/`、`constants/`、`api/` 里的内容 MUST 从该目录的 `index.ts` 再导出。该文件 MUST 只做再导出，MUST NOT 存放实现。其他目录导入这些能力时 MUST 从该 `index.ts` 导入，MUST NOT 指向内部文件。同一目录的内容 MUST 统一导入。该目录已有其他入口文件时，新导出 MUST 加进已有入口，MUST NOT 另建 `index.ts`。

#### Scenario: 新增共享工具
- **WHEN** 在还没有入口的 `utils/` 中新增一个被其他目录使用的函数
- **THEN** Agent 从 `utils/index.ts` 再导出它，调用方从 `utils` 导入

#### Scenario: 已有入口不是 index.ts
- **WHEN** 共享目录已经用其他文件作为入口
- **THEN** 新导出加进该文件，不另建 `index.ts`

#### Scenario: 同一目录的多项导入
- **WHEN** Agent 从同一个目录新导入多项内容
- **THEN** 这些导入统一从该目录的入口导入

## REMOVED Requirements

### Requirement: 按职责分层
**Reason**: skill 正文没有组件、hook、util、`api/` 之间的调用分层
**Migration**: 不再要求组件避开请求、hook 不写 JSX，或 util 不依赖 React

### Requirement: 依赖向下
**Reason**: skill 正文没有依赖方向
**Migration**: 不再要求更宽的共享范围避开更窄的范围

### Requirement: 按阶段拆函数
**Reason**: skill 没有要求按阶段拆开新函数，只保留不拆的边界
**Migration**: 行数长但职责单一时不拆、已有多段流程不拆，并入「一个文件一个主职责」

## ADDED Requirements

### Requirement: 没有同类目录时使用缺省目录
在某一层还没有承担该职责的目录时，Agent MUST 按下列规则放置新代码。该层已有同类目录时，MUST 跟着那个目录。仓库还没有 `web/` 和 `mobile/` 时，Agent MUST NOT 新建这两端目录。

`components/`、`hooks/`、`utils/`、`types/`、`styles/`、`constants/` 按共享范围放置：全项目复用的放在源码根下的同名目录；只有一端复用、且该端目录已存在的，放在该端目录下的同名目录；只服务一个页面或功能的，放在该页面或功能旁边。

接口函数放在源码根下的 `api/`。`api/` MUST 只放接口函数。仓库里已经有请求目录时，MUST 放进那个目录，MUST NOT 另建 `api/`。被源码 import 的图片、音频、字体和 SVG 放在源码根下的 `assets/`。favicon 和 robots.txt 放在仓库根的 `public/`。新增的 `.sql` 文件放在仓库根的 `db/`。

#### Scenario: 全项目共用组件还没有目录
- **WHEN** 新组件被全项目复用，且源码根下还没有组件目录
- **THEN** Agent 把它放进源码根下的 `components/`

#### Scenario: 只有 web 使用的 hook
- **WHEN** 新 hook 只被 web 使用，且 `web/` 已存在、其下还没有 hook 目录
- **THEN** Agent 把它放进 `web/hooks/`

#### Scenario: 仓库还没有两端目录
- **WHEN** 新样式只被一端使用，且仓库里还没有 `web/` 和 `mobile/`
- **THEN** Agent 把它放进源码根下的 `styles/`，不新建 `web/` 或 `mobile/`

#### Scenario: 一端目录已有别的工具名
- **WHEN** 新工具函数只被 web 使用，源码根的工具函数放在其他目录，且 `web/` 下还没有工具目录
- **THEN** Agent 把它放进 `web/utils/`

#### Scenario: 新的接口还没有请求目录
- **WHEN** 本次新增接口函数，且仓库里还没有承载请求的目录
- **THEN** Agent 把它放进源码根下的 `api/`

#### Scenario: 新增 SQL 文件
- **WHEN** Agent 新增 `.sql` 文件，且仓库里还没有承载 SQL 的目录
- **THEN** Agent 把它放进仓库根的 `db/`

#### Scenario: 已有资源目录
- **WHEN** 仓库里已经有承载被源码 import 的图片的目录，且本次新增一张这样的图片
- **THEN** Agent 把图片放进那个目录，不另建 `assets/`

### Requirement: 页面目录区分 Next
非 Next 项目中，该层已有页面目录或路由目录时，新页面和路由表 MUST 放进那个目录。还没有时，新的挂路由页面 MUST 放进 `containers/`，新的路由表 MUST 放进 `routes/`。`package.json` 的依赖中有 `next`，且仓库已有 `app/`、`pages/`、`src/app/` 或 `src/pages/` 时，Agent MUST NOT 新建 `containers/` 或 `routes/`，新页面 MUST 放进已有的那个路由目录。

#### Scenario: 非 Next 的新页面
- **WHEN** 项目不是 Next，且本次新增一个挂路由的页面，仓库里还没有页面目录
- **THEN** Agent 把页面放进 `containers/`，路由表放进 `routes/`

#### Scenario: 已有页面目录
- **WHEN** 项目不是 Next，仓库已有页面目录，且本次新增一个挂路由的页面
- **THEN** Agent 把页面放进那个目录，不另建 `containers/`

#### Scenario: Next 项目
- **WHEN** `package.json` 的依赖中有 `next`，仓库已有 `app/`、`pages/`、`src/app/` 或 `src/pages/`，且本次新增一个页面
- **THEN** Agent 把页面放进已有的那个路由目录，不新建 `containers/` 或 `routes/`

### Requirement: 新建目录用全小写
某一层已经有稳定命名风格时，新增文件 MUST 沿用该风格。这一层还没有时，新建目录的名字 MUST 使用全小写，多个词 MUST 用 `-` 连接。这条 MUST NOT 约束代码文件名。

Next 路由中的路由组 `(group)`、动态段 `[id]`、`[...slug]`、`[[...slug]]`、平行路由 `@slot`、拦截路由 `(.)`、`(..)`、`(..)(..)`、`(...)`、私有目录 `_folder` 和 `%5F` MUST 保持框架写法。

#### Scenario: 新建多词目录
- **WHEN** Agent 为发票列表新建目录，且这一层还没有稳定命名风格
- **THEN** 目录名为 `invoice-list`，其中的 `useInvoiceList.ts` 保持原文件名

#### Scenario: Next 动态段
- **WHEN** Agent 在 Next 路由目录中新增动态段
- **THEN** 目录保持 `[id]` 这类框架写法
