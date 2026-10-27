# naming-rules Specification

## Purpose

规定项目还没有明确命名风格时，新文件、新目录和新标识符使用哪一套缺省形式；某一层已经有稳定风格时，新增文件和目录沿用该层风格。

## Requirements

### Requirement: 有稳定风格时沿用该层
仅当项目中没有明确命名风格时，Agent MUST 依照命名规范命名。仓库在某个目录层级已经形成稳定命名风格时，该层新增的文件和目录 MUST 沿用该风格，MUST NOT 在同一层再引入第二套命名系统。

#### Scenario: 同层已有另一种风格
- **WHEN** 某一层的文件已经稳定使用全小写加 `-`，且本次在该层新增文件
- **THEN** 新文件沿用该风格，不改用另一套命名

#### Scenario: 项目还没有命名风格
- **WHEN** 项目还没有明确的标识符命名风格，且本次新增一个组件
- **THEN** 组件名使用 PascalCase

### Requirement: 缺省标识符和目录形式
适用本规范时，Agent MUST 按下列形式命名。组件、类、接口与类型别名使用 PascalCase。泛型参数使用 PascalCase，或使用单个大写字母。变量、参数、函数、方法以及对象的一般属性使用 camelCase。模块级且声明后不再重新赋值的常量使用 `UPPER_SNAKE_CASE`。新建目录使用全小写，多个词之间用 `-` 连接。自定义 Hook 的文件名与导出函数名使用 `useXxx`。

#### Scenario: 新建多词目录
- **WHEN** 项目没有目录命名风格，且 Agent 为发票列表新建目录
- **THEN** 目录名使用全小写，词与词之间用 `-` 连接

#### Scenario: 新增自定义 Hook
- **WHEN** 项目没有 Hook 命名风格，且 Agent 新增一个自定义 Hook
- **THEN** 文件名和导出函数名使用 `useXxx`

#### Scenario: 新增模块级常量
- **WHEN** 项目没有常量命名风格，且 Agent 新增一个模块级、声明后不再重新赋值的常量
- **THEN** 该常量使用 `UPPER_SNAKE_CASE`
