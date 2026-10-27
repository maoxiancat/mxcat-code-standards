# 注释规范

## 注释形态

- 单行注释使用 `//`，多行注释使用 `/** ... */`。

## JSDoc 字段

- 摘要采用短语式说明，默认不以句号结尾；不重复标识符的字面含义。
- `@param` 解释需要文档的参数以及参数的含义、作用，一参数一行，中间不空行
- 返回标签使用 `@return`。解释返回值含义、边界
- 分支、异步或副作用示例应说明真正需要理解的输入输出或影响，不为覆盖所有分支堆示例。

## 修改范围

- 仅为新增源码增加注释，未要求重写文案时保守现有注释的格式，不无故重已有注释。

## 示例：

```ts
/**
 * 等文档加载完，再等页面登记的任务全部结束或超时
 * @param signal 组件卸载时用来中止等待
 * @return 页面任务全部结束时为 true，超时或 signal 已中止时为 false
 */
async function waitForPageTasks(signal: AbortSignal): Promise<boolean> {
  if (signal.aborted) {
    return false;
  }

  await whenDocumentLoaded();
  return waitUntilTasksFinishOrTimeout(signal);
}
```