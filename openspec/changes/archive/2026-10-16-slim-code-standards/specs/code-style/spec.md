## Purpose

约束 Agent 在改动 TypeScript / JavaScript 源码时，用少量稳定的风格规则整理导入、导出、注释、空行和 Promise 样板，并且不改变行为。

## ADDED Requirements

### Requirement: 风格整理不改变行为
应用风格约束时，Agent MUST 保持执行顺序、错误处理、兜底覆盖和副作用时机不变。

#### Scenario: 整理一个函数
- **WHEN** Agent 调整函数里的导入、注释或空行
- **THEN** 函数的返回结果、抛出的错误和副作用保持不变

### Requirement: 导入按命名风格分组
Agent MUST 按 `PascalCase`、`camelCase`、`UPPER_SNAKE_CASE` 的顺序分组导入，组间空一行。同一组内类型导入 MUST 放在值导入前面。Agent MUST 删除确认未使用的导入，MUST 保留仅用于副作用的裸导入。Agent MUST NOT 修改路径别名或导入语义。

#### Scenario: 混合导入
- **WHEN** 文件同时导入三种命名风格的类型和值
- **THEN** 导入分成这三组，每组里类型导入在值导入之前，组间空一行

### Requirement: 集中导出按命名风格分组
只做再导出的文件中，Agent MUST 按同样的命名风格分组，单行导出在前，多行导出在后，组间空一行。Agent MUST NOT 按字符长度重排导出。

#### Scenario: 入口文件
- **WHEN** Agent 修改再导出符号的 `index` 文件
- **THEN** 导出按命名风格分组，且不按导出行的字符长度排序

### Requirement: 函数注释只用一种 JSDoc
函数的名字和类型还没有说清行为、参数用途或返回内容时，Agent MUST 写一块 JSDoc。块的第一行 MUST 是摘要，说明行为或时序，且 MUST NOT 以句号结尾。需要说明的参数 MUST 紧跟摘要，每个参数一行 `@param`，中间不空行，并写这个参数在这里做什么。`@param` MUST NOT 重复签名里已有的类型。名字和类型已经说清行为、参数用途和返回内容时，Agent MUST NOT 添加这块注释。

#### Scenario: 行为看不出来的函数
- **WHEN** Agent 编写的函数，其时序和参数用途不能从名字和类型看出来
- **THEN** 注释使用下面的形状

```ts
/**
 * 等文档加载完，再等页面登记的任务全部结束或超时
 * @param signal 组件卸载时中止
 * @return 页面任务全部结束时为 true，超时或 signal 已中止时为 false
 */
```

#### Scenario: 行为已经明显的函数
- **WHEN** 函数名和参数类型已经说清行为、参数用途和返回内容
- **THEN** Agent 不添加 JSDoc 块

### Requirement: 返回注释写明每一种结果
被注释的函数有返回值时，Agent MUST 写一行 `@return`，说明返回的内容。只有一种结果时 MUST 写那一种。有多种结果时 MUST 逐种写明，包括成功、超时、空值和中止。没有返回值的函数 MUST NOT 写 `@return`。

#### Scenario: 两种结果
- **WHEN** 被注释的函数在完成时返回一个值，在超时或中止时返回另一个值
- **THEN** `@return` 同时写明这两种结果

#### Scenario: 没有返回值
- **WHEN** 被注释的函数没有返回值
- **THEN** JSDoc 块中没有 `@return`

### Requirement: 空行表示语义分段
声明段和后面的控制流之间、控制块后面还有语句时、对象或配置里的语义组之间，Agent MUST 空一行。只是前一个声明的紧密派生时，声明 MUST 保持紧邻。Agent MUST NOT 只为了看起来整齐而插入空行。

#### Scenario: 函数里的两个阶段
- **WHEN** 函数先读取输入，再进入单独的校验阶段
- **THEN** 两个阶段之间空一行

### Requirement: Promise 透传保持直接
函数只是把一个 Promise 交出去时，MUST NOT 声明为 `async`。`async` 函数里，除非 `try`、`catch`、`finally` 或后续控制流依赖 awaited 的拒绝，否则 MUST NOT 使用 `return await`。

#### Scenario: 转发 Promise
- **WHEN** 函数体只返回另一个函数的 Promise
- **THEN** 该函数不是 `async`，也不使用 `return await`
