## Why

日常改 TypeScript / JavaScript 时，架构落点和代码风格如果拆成两套很细的规则，Agent 要么加载过长的上下文，要么在局部修改里顺手重构。需要一份精简规范，把仍会改变写法的约束收进同一个 skill。

## What Changes

- 新增一个 Cursor Agent Skill，在同一次源码改动里同时约束架构落点和代码风格。
- 架构约束只保留复用、目录、职责分层、依赖方向、公共入口、拆分和命名。
- 代码约束只保留导入分组、导出分组、注释、空行和 Promise 样板。
- 注释固定为一段 JSDoc：一行摘要、按需 `@param`、有返回值时用 `@return` 写明返回内容；多种返回结果逐种写明。
- 行数阈值、按字符长度排序、强制 `@example`、`if` 改 `switch`、helper 返回类型精简和 `prettier-ignore` 不纳入。

## 能力

### 新增能力

- `architecture-constraints`: 何时启用架构约束，以及复用、放置、分层、依赖、入口、拆分和命名。
- `code-style`: 导入、导出、注释、空行、Promise 样板，以及风格整理不得改变行为。

### 修改的能力

- 无。

## 影响

- 技能放在 `skills/mxcat-code-standards/`，供 `npx skills` 从 GitHub 安装。不安装到本仓库的 `.cursor/skills/` 里使用。
- 不改业务代码，不引入新的格式化依赖。目标仓库已有目录名、路径别名和 Prettier 配置优先于本 skill 未写明的细节。
