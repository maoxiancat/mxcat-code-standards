---
name: mxcat-code-standards
description: mxcat-code-standards 约束新增 TypeScript、JavaScript、JSX、TSX 源码的架构落点和代码风格。为源码新增、修改、迁移和重构提供编码规范，覆盖复目录与模块边界、代码风格。
---

# mxcat-code-standards

## 安装

安装与验证见本技能目录 [README.md](README.md)。

## 约束范围

- 根据任务读取对应的规范文档进行约束。
- 无特殊要求时，只约束本次新增代码。
- 某类规范项目里已有明确风格时，沿用项目已有风格；项目没有的部分，仍按本 skill 对应规范约束。

## 规则选择

| 规范类型 | 读取时机 | 文件位置 |
| --- | --- | --- |
| 目录规范 | 新增文件或目录时 | [rules/directory-rules.md](rules/directory-rules.md) |
| 代码规范 | 新增源码时 | [rules/code-rules.md](rules/code-rules.md) |
| 注释规范 | 为新增源码编写注释时 | [rules/comment-style.md](rules/comment-style.md) |
| 导入规范 | 新增或整理 `import` 时 | [rules/import-style.md](rules/import-style.md) |
| 导出规范 | 新增或整理 barrel、`index.*` 等集中导出时 | [rules/export-style.md](rules/export-style.md) |
| 命名规范 | 需要为新文件、目录或标识符命名时 | [rules/naming-rules.md](rules/naming-rules.md) |
