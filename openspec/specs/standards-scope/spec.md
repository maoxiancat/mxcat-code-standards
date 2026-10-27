# standards-scope Specification

## Purpose

规定 mxcat-code-standards 按任务读取哪一份规则，以及默认只约束本次新增代码，并在项目已有明确风格时沿用该风格。

## Requirements

### Requirement: 按任务读取对应规则
Agent MUST 根据任务读取对应的规范文档，并且只在该文档标明的时机使用它。

| 规范类型 | 读取时机 | 文件 |
| --- | --- | --- |
| 目录规范 | 新增文件或目录时 | `rules/directory-rules.md` |
| 代码规范 | 新增源码时 | `rules/code-rules.md` |
| 注释规范 | 为新增源码编写注释时 | `rules/comment-style.md` |
| 导入规范 | 新增或整理 `import` 时 | `rules/import-style.md` |
| 导出规范 | 新增或整理 barrel、`index.*` 等集中导出时 | `rules/export-style.md` |
| 命名规范 | 需要为新文件、目录或标识符命名时 | `rules/naming-rules.md` |

#### Scenario: 新增源码
- **WHEN** Agent 新增 TypeScript、JavaScript、JSX 或 TSX 源码
- **THEN** Agent 读取代码规范，并按该规范约束这次新增的源码

#### Scenario: 只整理导入
- **WHEN** Agent 新增或整理 `import`，且本次没有新增文件或目录
- **THEN** Agent 读取导入规范，不因这次整理去套用目录规范

#### Scenario: 新增文件
- **WHEN** Agent 新增文件或目录
- **THEN** Agent 读取目录规范

#### Scenario: 为新标识符命名
- **WHEN** Agent 需要为新文件、目录或标识符命名
- **THEN** Agent 读取命名规范

### Requirement: 默认只约束本次新增代码
无特殊要求时，Agent MUST 只约束本次新增代码。

#### Scenario: 任务没有要求整理已有代码
- **WHEN** 任务没有要求重写或整理已有代码
- **THEN** Agent 只把规范用在本次新增的代码上

### Requirement: 项目已有风格优先
某类规范在项目里已有明确风格时，Agent MUST 沿用项目已有风格。项目没有覆盖到的部分，Agent MUST 仍按本技能对应规范约束。

#### Scenario: 项目已有该类风格
- **WHEN** 项目对某一类规范已有明确风格，且本次新增的代码落在该类规范上
- **THEN** 新增部分沿用项目已有风格

#### Scenario: 项目没有该类风格
- **WHEN** 项目对某一类规范还没有明确风格，且本次新增的代码落在该类规范上
- **THEN** Agent 按本技能对应规范约束这部分
