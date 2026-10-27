# comment-style Specification

## Purpose

规定为新增源码编写注释时使用哪种注释形态、JSDoc 如何写摘要、参数和返回值，以及已有注释在未要求重写时保持原样。

## Requirements

### Requirement: 注释形态
单行注释 MUST 使用 `//`。多行注释 MUST 使用 `/** ... */`。

#### Scenario: 为新增函数写多行注释
- **WHEN** Agent 为新增函数写多行注释
- **THEN** 注释使用 `/** ... */`

#### Scenario: 写单行注释
- **WHEN** Agent 为新增源码写单行注释
- **THEN** 注释使用 `//`

### Requirement: JSDoc 的摘要、参数和返回
摘要 MUST 采用短语式说明，默认 MUST NOT 以句号结尾，MUST NOT 重复标识符的字面含义。`@param` MUST 解释需要文档的参数以及参数的含义、作用，一参数一行，中间不空行。返回标签 MUST 使用 `@return`，并解释返回值含义、边界。分支、异步或副作用示例 MUST 说明真正需要理解的输入输出或影响，MUST NOT 为覆盖所有分支堆示例。

#### Scenario: 需要说明参数和返回值
- **WHEN** Agent 为新增函数编写 JSDoc，且该函数有需要说明的参数和返回值
- **THEN** 第一行是不以句号结尾的短语摘要，每个需要文档的参数紧跟一行 `@param` 且中间不空行，返回使用 `@return` 说明含义和边界

#### Scenario: 摘要会重复名字
- **WHEN** 摘要将要复述函数名的字面含义
- **THEN** 摘要不写这层字面重复

### Requirement: 只为新增源码增加注释
Agent MUST 仅为新增源码增加注释。未要求重写文案时，Agent MUST 保守现有注释的格式，MUST NOT 无故重写已有注释。

#### Scenario: 已有注释未被要求重写
- **WHEN** 已有函数带有注释，且本次任务没有要求重写文案
- **THEN** Agent 保持该注释的格式，不重写它
