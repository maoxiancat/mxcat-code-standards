## 背景

这是一份同时覆盖架构落点和代码风格的精简 skill。见 `proposal.md` 的 Why。

## 目标 / 非目标

**目标：**

- 用一份项目级 `SKILL.md` 同时表达架构约束和代码风格约束。
- Agent 先判断这次改动是否改变结构，再决定要不要执行架构段。
- 注释形状以探索中确认的 JSDoc 为准，并带上那段示例。

**非目标：**

- 不拆成 `references/` 规则层，不另建触发矩阵、checklist 和 evals。
- 不规定行数上限、字符长度排序、`@example`、`switch` 改写、helper 返回类型或 `prettier-ignore`。

## 决策

### 一个 skill 文件

本仓库是要发布到 GitHub 的技能源，摆放与 [mxcat-commit](https://github.com/maoxiancat/mxcat-commit) 相同。技能入口是 `skills/mxcat-code-standards/SKILL.md`，`name` 为 `mxcat-code-standards`。仓库根目录放 `README.md` 和 `.gitignore`，技能目录放安装说明 `README.md`。正文分启用、架构、代码三段。细节不再拆引用文件。

不放进本仓库的 `.cursor/skills/`。那是 OpenSpec 工作流技能的安装位置。

### 描述里写明触发

`description` 写明：新增或修改 TypeScript / JavaScript / JSX / TSX 源码，以及判断模块边界、拆分文件、整理导入注释和空行时使用。正文要求 Agent 只执行本文件列出的约束。

### 仓库惯例覆盖未写明的细节

目录名、路径别名、Prettier 能决定的换行，跟目标仓库的现有写法。本 skill 只补仓库没有写明、且规格里列出的那些约束。

### 注释示例原样放进 skill

非显而易见函数的示例使用规格里的那一块，摘要和 `@param` 保持探索时确认的措辞：

```ts
/**
 * 等文档加载完，再等页面登记的任务全部结束或超时
 * @param signal 组件卸载时中止
 * @return 页面任务全部结束时为 true，超时或 signal 已中止时为 false
 */
```

`@return` 这一行用来演示「多种结果都写出来」。没有返回值的函数不写 `@return`。显而易见的函数不写整块注释。

## 风险与取舍

- [规则被扩写成细规] → skill 描述和正文都写明只执行本文件列出的约束。
- [粗规则被忽略] → 架构段和注释段各带少量短反例，不另建规则文档。
- [局部修改被拆文件] → 启用条件写成硬门槛：只有新增文件、新职责或新公共出口才走架构段。

## 迁移

发布物是 `skills/mxcat-code-standards/`。回退时删除该目录，以及仓库根目录为它添加的 `README.md`。

## 未决问题

无。
