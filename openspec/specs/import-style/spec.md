# import-style Specification

## Purpose

规定新增或整理 `import` 时如何分组、排序、空行，以及哪些未使用导入可以删除。整理不得改路径和导入语义。

## Requirements

### Requirement: 导入整理的改动范围
整理导入时，Agent MUST 只改 `import` 相关的顺序、分组、空行、格式，以及删除确认未使用的导入，并且 MUST 保持最小 diff。Agent MUST NOT 改路径别名与路径文本。Agent MUST NOT 改默认导入、命名导入、类型导入的语义。Agent MUST NOT 引入与 lint 无关的重构。

#### Scenario: 整理一个文件的导入
- **WHEN** Agent 整理文件中的 `import`
- **THEN** 路径文本、路径别名和导入语义保持不变，改动限于顺序、分组、空行、格式和确认未使用的导入

### Requirement: 按命名风格分组
导入 MUST 按命名风格分组，顺序为 `PascalCase`、`camelCase`、`UPPER_SNAKE_CASE`。组间 MUST 保留 1 个空行。

#### Scenario: 三种命名同时出现
- **WHEN** 文件同时导入 `PascalCase`、`camelCase` 和 `UPPER_SNAKE_CASE` 的绑定
- **THEN** 三者按这个顺序分成三组，组间各空一行

### Requirement: 子组内先单行后多行
每个值导入或类型导入子组内，Agent MUST 先放单行 import，再放多行 import，单行区与多行区之间 MUST 保留 1 个空行。单行 import MUST 按整行字符长度从长到短排序；长度相同则按主要导入成员名字母序（A-Z）排序。多行 import 块 MUST 放在对应子组的单行 import 区之后，多行块之间 MUST 保留 1 个空行。多行 import 块之间 MUST 按主要导入成员名字母序（A-Z）保持稳定顺序。多行 import 块内部成员 MUST 按成员名长度从长到短排序；成员名长度相同按字母序（A-Z）。存在初始化依赖的导入和裸副作用导入 MUST 放在最后。

#### Scenario: 单行与多行并存
- **WHEN** 同一子组里既有单行 import 也有多行 import
- **THEN** 单行在前并按整行长度从长到短排列，空一行之后是多行块，多行块之间再空一行

#### Scenario: 裸副作用导入
- **WHEN** 文件包含一条裸副作用导入
- **THEN** 该导入放在最后

### Requirement: 值导入位于类型导入之前
同一命名组内，值导入 MUST 位于 `import type` 之前。值导入和类型导入各自作为一个排序子组，并继续适用单行在前、多行在后以及对应的长度、成员排序。值导入与类型导入之间 MUST NOT 额外拆分空行。

#### Scenario: 同组同时有值和类型
- **WHEN** 同一个命名风格组里既有值导入也有 `import type`
- **THEN** 值导入在前，类型导入在后，两者之间不额外空行

### Requirement: 单行导入的成员顺序
单行 `import { ... } from '...'` 内的成员 MUST 按成员名长度从长到短排序。成员名长度相同则按字母序（A-Z）排序。

#### Scenario: 一条导入里的多个成员
- **WHEN** 一条单行命名导入包含多个成员，且名字长度不同
- **THEN** 成员按名字长度从长到短排列

### Requirement: 只删除确认未使用且无必要副作用的导入
已确认未使用的成员可以移除。删除最后一个运行时绑定或整条导入之前，Agent MUST 确认不会丢失必要的模块执行；只有已确认无必要副作用时，才可以删除整条导入。Agent MUST NOT 删除仍有必要的裸副作用导入，也 MUST NOT 借整理改变初始化顺序。Agent MUST NOT 为了看起来整洁而删除用途不明确、由装饰器、宏或构建插件隐式消费，或无法在当前上下文确认的导入。

#### Scenario: 确认未使用且没有副作用
- **WHEN** 一个导入成员已确认未使用，且删除整条导入不会丢失必要的模块执行
- **THEN** Agent 可以删除该导入

#### Scenario: 仍有必要的副作用
- **WHEN** 一条裸导入仍有必要的副作用
- **THEN** Agent 保留该导入，不改变初始化顺序

#### Scenario: 用途无法确认
- **WHEN** 一个导入可能由装饰器、宏或构建插件隐式消费，且当前上下文无法确认
- **THEN** Agent 保留该导入
