# directory-rules Specification

## Purpose

规定新增 TypeScript、JavaScript、JSX、TSX 源码时，各类目录承载什么、新代码放进哪一层，以及 Next 框架目录和文件名如何保持。

## Requirements

### Requirement: 目录按职责表承载
Agent MUST 按下列职责使用这些目录。每种目录只承载表中列出的内容，MUST NOT 承载表中排除的内容。

| 类型 | 承载 | 不承载 |
| --- | --- | --- |
| `components` | UI 渲染、局部展示交互、视图组合 | 裸请求、协议适配、跨页面业务编排 |
| `utils` | 纯转换、格式化、无 UI 辅助逻辑 | React 状态、页面上下文、组件渲染 |
| `hooks` | 状态与数据获取编排、复用交互逻辑 | JSX 布局、协议与客户端细节 |
| `api` | 请求、客户端封装、协议适配、模型转换 | 页面状态、UI 组合、展示分支 |
| `assets` | 被源码 import 的图片、音频、字体、SVG | 按 URL 直接提供的站点根文件、SQL、业务源码 |
| `constants` | 路由 path、环境标记、storage key、选项和静态映射 | 类型契约、请求实现、展示组件 |
| `containers` | 非 Next 的挂路由页面、页面组合和高层编排 | 路由表、请求客户端、长期混杂的具体实现 |
| `types` | 类型、接口等编译期契约 | 运行时值、请求、UI |
| `styles` | 样式和样式变量 | 业务逻辑、请求、组件实现 |
| `public` | favicon、robots.txt 等按 URL 直接提供的站点根文件 | 被源码 import 的图片、音频、字体、SVG、业务源码 |
| `db` | SQL | 业务源码、静态资源、站点根文件 |
| `routes` | 路由表，以及路径与页面的装配 | 页面实现、请求客户端、业务转换 |

#### Scenario: 被源码 import 的图片
- **WHEN** Agent 新增一张被源码 import 的图片
- **THEN** 该图片由 `assets` 承载，不放进 `public`

#### Scenario: 按 URL 提供的站点根文件
- **WHEN** Agent 新增 favicon 或 robots.txt
- **THEN** 该文件由 `public` 承载，不放进 `assets`

#### Scenario: 新增 SQL
- **WHEN** Agent 新增 SQL
- **THEN** 该文件由 `db` 承载，不放进业务源码、静态资源或站点根文件目录

### Requirement: 复用同层同类目录
Agent MUST 按照共享范围以及所需类型创建对应类型目录。项目中已经存在承担同类职责的目录时，Agent MUST 复用该目录，MUST NOT 在同层新造语义重复的新目录。

#### Scenario: 同层已有工具目录
- **WHEN** 同一层已经有承载纯转换和格式化的目录，且本次新增一个同类工具函数
- **THEN** Agent 把函数放进该目录，不另建一个职责相同的目录

### Requirement: 按共享范围放置
逻辑只服务单个页面、路由或 feature 时，Agent MUST 优先与该页面或 feature 就近放置。逻辑会被多个页面、多个业务域或多个 feature 复用时，Agent MUST 放入项目既有的共享层目录。

#### Scenario: 只服务一个页面
- **WHEN** 一段逻辑只服务一个页面
- **THEN** Agent 把它放在该页面旁边

#### Scenario: 多处复用
- **WHEN** 一段逻辑被多个页面复用，且项目已有共享层目录
- **THEN** Agent 把它放进该共享层目录

### Requirement: 跨越职责边界时归位
文件跨越这些职责边界时，Agent MUST 将相应实现归位，页面保留组合关系。Agent MUST NOT 把筛选、请求、转换、弹窗与展示全部长期堆在一个入口，也 MUST NOT 为了拆分随机制造目录。新增目录只是因为当前文件太大时，Agent MUST 先判断职责边界，再按职责拆目录，MUST NOT 把文件随意分散到多个目录。

#### Scenario: 入口长期堆了多种职责
- **WHEN** 一个入口同时长期堆着筛选、请求、转换、弹窗和展示
- **THEN** Agent 按职责把实现归位，页面保留组合关系，不为拆分随机制造目录

#### Scenario: 只因为文件太大
- **WHEN** 准备新建目录的原因只是当前文件太大
- **THEN** Agent 先判断职责边界，再按职责拆目录

### Requirement: 页面入口留在既有页面层
新增页面级或路由级入口时，Agent MUST 放到仓库既有的页面层或路由层目录，由该入口承接路由装配与页面编排，MUST NOT 把页面主体散落到共享组件层或临时目录。页面入口代码 MUST 放在项目既有的页面层，MUST NOT 把页面编排逻辑塞进共享组件目录。本技能 MUST NOT 强制页面层名叫 `pages`、`containers` 或 `app`，Agent MUST 沿用仓库既有主流结构。

#### Scenario: 新增页面入口
- **WHEN** Agent 新增一个页面级入口，且仓库已有自己的页面层目录
- **THEN** Agent 把入口放进该页面层，不把页面主体放到共享组件目录，也不改页面层的既有名称

### Requirement: 共享展示、工具和请求各归其层
共享展示组件 MUST 放在项目既有的共享组件层，MUST NOT 散落在多个页面目录里重复维护。纯工具逻辑 MUST 放在项目既有的工具层，MUST NOT 藏在页面文件或组件文件尾部。请求和协议适配 MUST 放在项目既有的 service 或 api 层，MUST NOT 直接散落到组件、页面或通用 util 中。

#### Scenario: 新增共享展示组件
- **WHEN** 本次新增一个被多处使用的展示组件，且项目已有共享组件层
- **THEN** Agent 把它放进该共享组件层，不在多个页面目录各放一份

#### Scenario: 新增请求和协议适配
- **WHEN** 项目已有 service 或 api 层，且本次新增请求和协议适配
- **THEN** Agent 把这段实现放进该层，不写进组件、页面或通用 util

### Requirement: 类型目录统一导出
`components`、`utils`、`hooks`、`api`、`constants`、`types`、`styles` 类型目录 MUST 进行统一导出，并沿用项目已有导出风格，例如 `index.ts`。

#### Scenario: 已有导出风格
- **WHEN** `utils` 已经用某种文件做统一导出，且本次在其中新增一个对外函数
- **THEN** Agent 按该目录已有的导出风格把它导出

### Requirement: Next 特殊写法和文件名保持框架形式
框架中的特殊写法 MUST 沿用框架的要求。Next 路由里的路由组 `(group)`、动态段 `[id]`、`[...slug]`、`[[...slug]]`、平行路由 `@slot`、拦截路由 `(.)`、`(..)`、`(..)(..)`、`(...)`、私有目录 `_folder` 和 `%5F` MUST 保持框架写法。`page.tsx`、`layout.tsx`、`loading.tsx`、`error.tsx`、`not-found.tsx`、`template.tsx`、`default.tsx`、`route.ts` MUST 保持框架文件名。

#### Scenario: 新增动态段
- **WHEN** Agent 在 Next 路由目录中新增动态段
- **THEN** 目录名保持 `[id]` 这类框架写法

#### Scenario: 新增页面文件
- **WHEN** Agent 在 Next 路由目录中新增页面文件
- **THEN** 文件名为 `page.tsx`
