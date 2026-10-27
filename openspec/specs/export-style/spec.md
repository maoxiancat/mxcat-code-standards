# export-style Specification

## Purpose

规定整理 `index.ts`、`index.tsx`、`index.js`、`index.jsx` 以及其他集中导出文件时，导出如何按形态和命名风格分组、如何按长度排序。

## Requirements

### Requirement: 导出整理的改动范围
本规范适用于集中导出文件，包括 `index.ts`、`index.tsx`、`index.js`、`index.jsx`，以及其他以集中导出为职责的 barrel 文件。整理时 Agent MUST 只改 `export` 相关的顺序、分组、空行与格式，并且 MUST 保持最小 diff。无特殊要求时，Agent MUST NOT 改变业务语义，MUST NOT 新增或删除导出成员。

#### Scenario: 整理入口文件
- **WHEN** Agent 整理一个集中导出文件，且任务没有要求增删导出
- **THEN** 导出成员集合和业务语义不变，改动限于顺序、分组、空行和格式

### Requirement: 先按声明形态再按命名风格
单行 `export` MUST 在前，多行 `export` MUST 在后，单行与多行之间 MUST 保留 1 个空行。各形态内 MUST 按成员命名风格分组，组间保留 1 个空行，顺序为 `PascalCase`、`camelCase`、`UPPER_SNAKE_CASE`。单行 `camelCase` 导出 MUST 仍在多行 `PascalCase` 导出之前，MUST NOT 为命名分组跨越形态分区。

#### Scenario: 单行小驼峰与多行大驼峰并存
- **WHEN** 入口文件同时有单行 `camelCase` 导出和多行 `PascalCase` 导出
- **THEN** 单行 `camelCase` 在前，空一行后才是多行 `PascalCase`

### Requirement: 同组内按长度排序
仅在同一形态、同一命名风格组内，单行 `export` MUST 按整行字符长度从长到短排序。每个多行块内部成员 MUST 按成员名长度从长到短排序。同一命名风格组内的多行块 MUST 保持原有相对顺序，MUST NOT 按整个块的长度排序。单行 `export { ... }` 内成员 MUST 按成员名长度从长到短排序；长度相同按字母序（A-Z）排序。

#### Scenario: 同一组的单行导出
- **WHEN** 同一形态、同一命名风格组内有多条单行导出，且整行长度不同
- **THEN** 这些行按整行字符长度从长到短排列

#### Scenario: 同一组的多个多行块
- **WHEN** 同一命名风格组内有多个多行导出块
- **THEN** 每个块内部成员按名字长度从长到短排列，块与块之间保持原有相对顺序
