# code-rules Specification

## Purpose

规定新增 TypeScript 与 JavaScript 源码时，如何复用已有实现、按职责拆分、限制依赖方向，以及如何排列声明、语句和空行。

## Requirements

### Requirement: 先复用已有实现
新增函数、组件、hook、请求函数或依赖之前，Agent MUST 先搜索项目中是否已有可复用实现。现有实现经过小幅扩展即可满足需求时，Agent MUST 复用或扩展该实现，MUST NOT 另行实现。项目已经安装功能相符的依赖且已有稳定用法时，Agent MUST 沿用该依赖，MUST NOT 再引入功能重叠的依赖。

#### Scenario: 已有实现可以扩展
- **WHEN** 项目里已有实现，小幅扩展后就能满足需求
- **THEN** Agent 扩展该实现，不另写一份

#### Scenario: 已有依赖可用
- **WHEN** 项目已安装功能相符的依赖，且仓库里已有稳定用法
- **THEN** Agent 沿用该依赖，不引入功能重叠的另一个依赖

### Requirement: 按职责拆分且不改变行为
本次新增代码 MUST 依职责拆分，并保持原有的执行顺序、错误处理、兜底和副作用时机，MUST NOT 改变原有逻辑。函数只是行数较长、职责仍然单一时，Agent MUST NOT 拆分。Agent MUST NOT 因为已有文件承担多类职责或包含多段流程而拆开该文件。Agent MUST NOT 为了拆分而新建没有职责边界的目录。

#### Scenario: 长函数职责仍然单一
- **WHEN** 函数行数较长，但职责单一
- **THEN** Agent 不拆分该函数

#### Scenario: 已有文件职责混杂
- **WHEN** 已有文件承担多类职责或包含多段流程
- **THEN** Agent 不因此拆开该文件

### Requirement: 依赖方向
引用时，Agent MUST 优先通过该目录的 `index.*` 公开入口引用。`component` 可以依赖 `hook`、共享组件、`util` 和类型，MUST NOT 依赖页面或容器的实现。`hook` 可以依赖 `service` 或 `api`、`util`、类型，以及更底层的通用 `hook`，MUST NOT 依赖页面实现和具体页面组件。`util` MUST 只依赖其他 `util` 或纯类型，MUST NOT 依赖 React 组件、页面实现或请求层。`api` 可以依赖更底层的客户端、`util` 和类型，MUST NOT 依赖 `component`。纯展示组件 MUST NOT 绕过 `hook` 或 `api` 直接请求客户端。

#### Scenario: 纯展示组件需要数据
- **WHEN** 一个纯展示组件需要请求数据
- **THEN** 它通过 `hook` 或 `api` 取得数据，不直接请求客户端，也不依赖页面或容器的实现

#### Scenario: 新增工具函数
- **WHEN** Agent 新增一个 `util`
- **THEN** 它只依赖其他 `util` 或纯类型，不依赖 React 组件、页面实现或请求层

#### Scenario: 引用其他目录
- **WHEN** Agent 引用另一个目录对外提供的能力，且该目录有 `index.*` 公开入口
- **THEN** 引用优先走该入口

### Requirement: 新增代码的声明和语句顺序
本次新增的顶层声明、函数体和组件体 MUST 按下列顺序排列。已有声明的相对顺序 MUST 保持不变。语句之间已有数据依赖或副作用先后时，Agent MUST 保持该先后。文件顶层在 `import` 之后 MUST 依次是类型、常量、只在本文件使用的函数、导出的函数或组件。只在本文件使用的函数 MUST 写在导出函数前面。同一组函数内，被调用的函数 MUST 放在调用方前面。函数体 MUST 依次是读取输入、解析与归一、派生、校验、构造结果、写入及其它副作用。同一输入的读取和它的直接派生 MUST 写在一起。校验 MUST 跟在被校验的值后面。组件函数里，`useEffect` 和 `useLayoutEffect` MUST 写在函数体最前面。依赖数组或同步求值参数要读的绑定，MUST 先于该 effect 声明。

#### Scenario: 新文件的顶层顺序
- **WHEN** Agent 新写一个同时包含类型、常量、文件内函数和导出函数的文件
- **THEN** `import` 之后依次是类型、常量、文件内函数、导出函数，被调用的文件内函数位于调用方前面

#### Scenario: 已有声明保持相对顺序
- **WHEN** 文件里已有声明，且本次在其中新增代码
- **THEN** 已有声明的相对顺序保持不变

#### Scenario: 组件中的 effect
- **WHEN** Agent 新写一个带 `useEffect` 的组件，且该 effect 要读一个绑定
- **THEN** 该绑定先于这个 effect 声明，`useEffect` 写在组件函数体最前面

### Requirement: 空行只标语义边界
空行 MUST 只标语义边界。边界处 MUST 保留 1 个空行，MUST NOT 连续空行。函数体、方法体和分支体内同样适用。同一语义步骤内紧密派生的声明，以及同一目的下连续的单行副作用，MUST 保持紧邻。Agent MUST NOT 为了版面整齐插入空行。提取输入、解析、派生、校验、构造结果和写入属于不同步骤，步骤之间 MUST 保留 1 个空行。声明段结束后，若后续是控制流或副作用调用，MUST 保留 1 个空行。发起请求与紧随其后的响应读取属于同一步骤，MUST 保持紧邻。

#### Scenario: 不同步骤之间
- **WHEN** 新增函数里依次出现解析、校验和构造结果
- **THEN** 这些步骤之间各保留 1 个空行，不连续空行

#### Scenario: 请求和读取响应
- **WHEN** 新增代码发起请求后紧接着读取响应
- **THEN** 这两句保持紧邻，中间不插入空行

### Requirement: 跨多行结构与控制流之后空一行
跨多行的类型断言、对象字面量、调用参数或复杂初始化，与相邻的独立声明或语句之间 MUST 保留 1 个空行，结构内部 MUST NOT 拆开。查找流程中，跨多行候选与后面的候选 MUST 分开；同一语义步骤中的单行候选和聚合数组 MUST 保持紧邻。`if` / `else`、`for` / `while` / `do`、`try` / `catch` / `finally` 或 `switch` 整段结束后，若后面仍有语句，MUST 保留 1 个空行。空行 MUST 加在整段控制流之后，MUST NOT 加在 `else`、`catch`、`finally` 或 `case` 之间。Agent MUST 只排版已经确定的控制流，MUST NOT 规定分支数量，也 MUST NOT 改写分支形态。

#### Scenario: 控制流后面还有语句
- **WHEN** 一段 `if` / `else` 结束之后还有语句
- **THEN** 空行加在整段 `if` / `else` 之后，不加在 `else` 之前

#### Scenario: 跨多行调用挨着下一条语句
- **WHEN** 一次跨多行的调用后面还有独立语句
- **THEN** 两者之间保留 1 个空行，调用结构内部不拆开

### Requirement: 对象成员和顶层注释分组
在对象字面量、状态、配置、内联对象类型和 `return { ... }` 中，跨多行的属性值与相邻的独立属性之间 MUST 保留 1 个空行。对象方法与其他独立成员之间 MUST 保留 1 个空行。同一子结构中紧密相关的属性 MUST 保持紧邻。顶层单行摘要注释与紧邻的 `const` MUST 组成一个声明组，不同组之间 MUST 保留 1 个空行。该分组 MUST NOT 扩展到函数内部。声明没有注释时，Agent MUST NOT 补写说明，也 MUST NOT 因此强制空行。

#### Scenario: 对象方法与其他成员
- **WHEN** 对象里有一个方法，旁边还有独立属性
- **THEN** 该方法与其他独立成员之间保留 1 个空行

#### Scenario: 声明没有注释
- **WHEN** 一条新增声明没有注释
- **THEN** Agent 不补写说明，也不因此插入空行
