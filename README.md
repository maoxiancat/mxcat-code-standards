# mxcat-code-standards

公开的 GitHub 技能源：提供 `mxcat-code-standards`，在新写 TypeScript / JavaScript 时约束架构落点和代码风格，可通过 `npx skills` 安装到 Cursor、Claude Code 等 Agent。

源仓库：https://github.com/maoxiancat/mxcat-code-standards

## 安装

项目：

```bash
npx -y skills add maoxiancat/mxcat-code-standards --skill mxcat-code-standards
```

全局：

```bash
npx -y skills add maoxiancat/mxcat-code-standards --skill mxcat-code-standards -g
```

仅安装到 Cursor 和 Claude Code：

```bash
npx -y skills add maoxiancat/mxcat-code-standards --skill mxcat-code-standards -a cursor -a claude-code
```

## 验证

列出源里的技能（应出现 `mxcat-code-standards`，且不要安装）：

```bash
npx -y skills add maoxiancat/mxcat-code-standards --list
```

本仓库作为本地路径时也可以列出：

```bash
npx -y skills add /path/to/mxcat-code-standards --list
```
