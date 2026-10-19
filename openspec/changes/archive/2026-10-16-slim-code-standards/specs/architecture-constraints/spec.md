## Purpose

约束 Agent 在新增或扩大 TypeScript / JavaScript 职责时，先复用现有实现，再按仓库已有边界放置和拆分代码。

## ADDED Requirements

### Requirement: 仅在结构变化时启用架构约束
变更会新增文件、新增职责或新增公共出口时，skill MUST 先执行架构约束。变更只修改现有函数内部、且没有新增职责时，skill MUST 跳过架构约束。

#### Scenario: 新增文件
- **WHEN** Agent 新增源码文件或新的公共出口
- **THEN** Agent 在写实现之前先执行架构约束

#### Scenario: 局部修改
- **WHEN** Agent 只修改现有函数内部的逻辑，且没有新增职责
- **THEN** Agent 不因架构约束而移动代码、拆分文件或新建目录

### Requirement: 先复用已有实现
新增函数、组件、hook、service 或依赖之前，Agent MUST 先搜索项目中是否已有同类实现。现有实现经过小幅扩展就能满足时，Agent MUST 扩展它。

#### Scenario: 已有 helper 可以扩展
- **WHEN** 项目里已有 helper，小幅扩展后就能满足需求
- **THEN** Agent 扩展该 helper，不另写一份实现

### Requirement: 沿用仓库目录
Agent MUST 把新代码放进已经承担同类职责的目录。Agent MUST NOT 在同层再建一个职责重复的目录。skill MUST NOT 规定目录必须叫 `pages` 或 `api`。

#### Scenario: 已有请求层
- **WHEN** 仓库里已经有承载请求和协议适配的目录
- **THEN** 新的请求和协议适配代码放进该目录

#### Scenario: 逻辑只服务一个功能
- **WHEN** 逻辑只服务一个页面或一个功能
- **THEN** Agent 把它放在该页面或功能旁边

#### Scenario: 逻辑被多处复用
- **WHEN** 逻辑被多个页面、功能或领域复用
- **THEN** Agent 把它放进仓库已有的共享层

### Requirement: 按职责分层
Agent MUST 把展示、状态编排、纯函数和请求适配分开放。组件 MUST NOT 包含裸请求。hook MUST NOT 渲染 JSX。util MUST NOT 依赖 React 或页面上下文。service MUST NOT 包含界面文案或组件组合。

#### Scenario: 组件需要远程数据
- **WHEN** 组件需要远程数据
- **THEN** 请求和协议适配放在 service 层，组件只接收整理后的数据

### Requirement: 依赖向下
页面可以依赖组件、hook、service 和 util。组件或 hook MUST NOT 依赖具体页面。util MUST NOT 依赖组件、页面或请求层。service MUST NOT 依赖组件、hook 或页面。共享底层 MUST NOT 依赖页面。

#### Scenario: 引用共享 util
- **WHEN** Agent 从共享 util 新增导入
- **THEN** 该 util 不导入页面、组件或 service

### Requirement: 跨目录走公共入口
Agent MUST 通过仓库已有的 `index.*` 暴露目录。跨目录导入 MUST 使用对方的公共入口。入口文件 MUST 只做再导出，MUST NOT 存放长期演进的实现。

#### Scenario: 引用另一个目录的能力
- **WHEN** Agent 从另一个目录导入能力
- **THEN** 导入使用该目录的公共入口，不指向内部私有实现文件

### Requirement: 一个文件一个主职责
同一文件承担展示、状态、请求、转换中的两类以上时，Agent MUST 拆开。页面入口同时负责数据获取、表格列、表单、弹窗和展示组件时，Agent MUST 拆开。Agent MUST NOT 新建没有职责边界的目录。

#### Scenario: 页面入口堆了多种职责
- **WHEN** 页面文件同时包含数据获取、列定义、表单状态、弹窗状态和展示标记
- **THEN** Agent 把这些职责拆到仓库已有的对应层

### Requirement: 按阶段拆函数
函数叠了多段流程、多类副作用或很深的分支时，Agent MUST 拆开。拆分时 Agent MUST 保持执行顺序、错误处理、兜底覆盖和副作用时机不变。函数只是行数长时，Agent MUST NOT 仅因此拆分。

#### Scenario: 长函数职责仍然单一
- **WHEN** 函数很长，但职责单一且结构清楚
- **THEN** Agent 保持该函数不动

### Requirement: 命名跟层走
自定义 Hook 的文件名和导出函数名 MUST 使用 `useXxx`。某一层已经有稳定命名风格时，该层的新文件 MUST 沿用该风格。

#### Scenario: 新增 hook
- **WHEN** Agent 新增自定义 hook
- **THEN** 文件名和导出函数名都以 `use` 开头
