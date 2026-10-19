## 1. Skill 入口

- [x] 1.1 创建 `skills/mxcat-code-standards/SKILL.md`，`name` 为 `mxcat-code-standards`，`description` 写明在新增或修改 TypeScript / JavaScript / JSX / TSX，以及判断模块边界、拆分文件、整理导入、注释和空行时使用
- [x] 1.2 写启用条件：新增文件、新职责或新公共出口时先走架构再写实现；只改现有函数内部时跳过架构，只对碰到的源码走代码约束

## 2. 架构约束

- [x] 2.1 写入复用、沿用仓库目录、私有就近与共享上提，并写明不规定 `pages` 或 `api` 这类目录名
- [x] 2.2 写入职责分层、依赖向下、跨目录走 `index.*` 且入口不堆实现
- [x] 2.3 写入一文件一主职责、按阶段拆函数且不因行数拆、Hook 使用 `useXxx`、同层沿用既有命名，并各附一条短反例

## 3. 代码约束

- [x] 3.1 写入风格整理不改变执行顺序、错误处理、兜底和副作用时机
- [x] 3.2 写入导入与导出按 `PascalCase`、`camelCase`、`UPPER_SNAKE_CASE` 分组；类型导入在值导入前；删除确认未使用的导入；导出不按字符长度排序
- [x] 3.3 写入注释形状：一行摘要、紧跟的 `@param`、有返回值时的 `@return`，并原样放入规格中的 JSDoc 示例；写明无返回值不写 `@return`，显而易见的函数不写整块注释
- [x] 3.4 写入空行表示语义分段，以及 Promise 透传不加 `async`、无意义的 `return await` 去掉

## 4. 边界核对

- [x] 4.1 通读 `SKILL.md`，确认没有行数阈值、`@example`、`prettier-ignore`、`switch` 改写和 helper 返回类型精简
