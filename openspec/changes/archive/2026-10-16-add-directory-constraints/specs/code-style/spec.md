## MODIFIED Requirements

### Requirement: 导入按来源职责和命名风格分组
Agent MUST 按导入路径中的职责目录分组，顺序为 `types`、`containers`、`components`、`utils`、`api`、`hooks`、`assets`、`constants`、`styles`、`routes`。`web/`、`mobile/` 或页面旁边的同名目录 MUST 归入同一组。同一职责组内 MUST 再按 `PascalCase`、`camelCase`、`UPPER_SNAKE_CASE` 分组，类型导入 MUST 在值导入前面。组间 MUST 空一行。第三方包 MUST 单独成组并放在这些职责组之前。路径里没有这些职责目录的导入 MUST 单独成组并放在最后。Agent MUST 删除确认未使用的导入，MUST 保留仅用于副作用的裸导入。Agent MUST NOT 修改路径别名或导入语义。

#### Scenario: 混合导入
- **WHEN** 文件同时导入第三方包、`components`、`utils`，以及组件的类型和值
- **THEN** 第三方包在最前，随后 `components` 在 `utils` 之前；组件组里类型导入在值导入之前；组间空一行

### Requirement: 函数注释只用一种 JSDoc
新写的函数 Agent MUST 写一块 JSDoc。块的第一行 MUST 是摘要，说明行为或时序，且 MUST NOT 以句号结尾。每个参数 MUST 紧跟摘要，一行一个 `@param`，中间不空行，并写这个参数在这里做什么。没有参数时 MUST NOT 写 `@param`。

#### Scenario: 新写的函数
- **WHEN** Agent 新写一个带参数、且有多种返回结果的函数
- **THEN** 注释使用下面的形状

```ts
/**
 * 等文档加载完，再等页面登记的任务全部结束或超时
 * @param signal 组件卸载时用来中止等待
 * @return 页面任务全部结束时为 true，超时或 signal 已中止时为 false
 */
```

#### Scenario: 名字已经说清行为
- **WHEN** 函数名和参数类型已经说清行为、参数用途和返回内容
- **THEN** Agent 仍然写一块 JSDoc

### Requirement: 返回注释写明每一种结果
新写的函数有返回值时，Agent MUST 写一行 `@return`，写返回的内容，并简单解释返回的内容。只有一种结果时 MUST 写那一种。有多种结果时 MUST 逐种写明，包括成功、超时、空值和中止。没有返回值的函数 MUST NOT 写 `@return`。

#### Scenario: 两种结果
- **WHEN** 新写的函数在完成时返回一个值，在超时或中止时返回另一个值
- **THEN** `@return` 同时写明这两种结果

#### Scenario: 没有返回值
- **WHEN** 新写的函数没有返回值
- **THEN** JSDoc 块中没有 `@return`

## REMOVED Requirements

### Requirement: 空行表示语义分段
**Reason**: skill 只写了空行规则的作用范围，没有写出怎么空行
**Migration**: 不再要求按语义分段插入空行

### Requirement: Promise 透传保持直接
**Reason**: skill 只写了 Promise 规则的作用范围，没有写出 `async` 或 `return await` 的限制
**Migration**: 不再要求转发 Promise 时去掉 `async` 和 `return await`

## ADDED Requirements

### Requirement: 风格只约束新写的代码
导入、导出和注释规则 MUST 只应用于本次新写的代码。Agent MUST NOT 为了这些规则重排、补注释或调整已有代码里本次没有改写的部分。已有文件里为接上新代码而新增的导入或导出 MUST 按这些规则插入，该文件中原有的导入和导出 MUST 保持原样。

#### Scenario: 旧文件只加一条导入
- **WHEN** Agent 在已有文件中加入一条导入以使用新代码
- **THEN** 新导入按命名风格分组插入，该文件中原有导入的顺序和分组保持不变

#### Scenario: 新写的函数
- **WHEN** Agent 新写一个函数
- **THEN** 该函数的注释遵循风格规则

#### Scenario: 已有函数未改写
- **WHEN** 已有函数的行为、参数用途或返回内容从名字和类型上看不出来，且本次任务没有改写该函数
- **THEN** Agent 不给该函数补 JSDoc
