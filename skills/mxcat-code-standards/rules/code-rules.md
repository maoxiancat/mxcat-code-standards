# 代码规范

## 复用

- 新增函数、组件、hook、请求函数或依赖之前，先搜索项目中是否已有可复用实现。
- 如果现有实现经过小幅扩展即可满足需求，则复用或扩展该实现，不得另行实现。
- 如果项目已经安装功能相符的依赖，且已有稳定用法，则沿用该依赖，不得再引入功能重叠的依赖。

## 拆分

- 本次新增代码应依职责拆分，并保持原有的执行顺序、错误处理、兜底和副作用时机，不得改变原有逻辑。
- 如果函数只是行数较长、职责仍然单一，则不得拆分。
- 不得因为已有文件承担多类职责或包含多段流程而拆开该文件。
- 不得为了拆分而新建没有职责边界的目录。

## 依赖

- 引用时优先通过该目录的 `index.*` 公开入口引用。
- `component` 可以依赖 `hook`、共享组件、`util` 和类型，不得依赖页面或容器的实现。
- `hook` 可以依赖 `service` 或 `api`、`util`、类型，以及更底层的通用 `hook`，不得依赖页面实现和具体页面组件。
- `util` 只可以依赖其他 `util` 或纯类型，不得依赖 React 组件、页面实现或请求层。
- `api` 可以依赖更底层的客户端、`util` 和类型，不得依赖 `component`。
- 纯展示组件不得绕过 `hook` 或 `api`，直接请求客户端。

## 顺序

- 本次新增顶层声明、函数体和组件体按下面的顺序排列。已有声明的相对顺序保持不变。语句之间已有数据依赖或副作用先后时，保持该先后。
- 文件顶层在 `import` 之后依次是类型、常量、只在本文件使用的函数、导出的函数或组件。
- 只在本文件使用的函数写在导出函数前面。同一组函数内，被调用的函数放在调用方前面。
- 函数体依次是读取输入、解析与归一、派生、校验、构造结果、写入及其它副作用。同一输入的读取和它的直接派生写在一起。校验跟在被校验的值后面。
- 组件函数里，`useEffect` 和 `useLayoutEffect` 写在函数体最前面。依赖数组或同步求值参数要读的绑定，先于该 effect 声明。

示例：

```tsx
type Input = {
  name: string;
};

const empty = '';

function normalize(name: string) {
  return name.trim() || empty;
}

function readName(input: Input) {
  return normalize(input.name);
}

export async function publish(input: Input) {
  const name = readName(input);

  if (!name) {
    return null;
  }

  const label = name.toUpperCase();

  await writeResult(label);
}

export function Panel({ id }: { id: string }) {
  useEffect(() => {
    const timer = subscribe(id);
    return () => timer.stop();
  }, [id]);

  const [open, setOpen] = useState(false);
  const label = open ? '收起' : '展开';

  function toggle() {
    setOpen((value) => !value);
  }

  return <button onClick={toggle}>{label}</button>;
}
```

## 空行

- 空行只标语义边界，边界处保留 1 个空行，不得连续空行。函数体、方法体和分支体内同样适用。同一语义步骤内紧密派生的声明，以及同一目的下连续的单行副作用，保持紧邻。不得为了版面整齐插入空行。
- 提取输入、解析、派生、校验、构造结果和写入属于不同步骤，步骤之间保留 1 个空行。声明段结束后，若后续是控制流或副作用调用，保留 1 个空行。发起请求与紧随其后的响应读取属于同一步骤，保持紧邻。
- 跨多行的类型断言、对象字面量、调用参数或复杂初始化，与相邻的独立声明或语句保留 1 个空行，结构内部不得拆开。查找流程中，跨多行候选与后面的候选分开；同一语义步骤中的单行候选和聚合数组保持紧邻。
- `if` / `else`、`for` / `while` / `do`、`try` / `catch` / `finally` 或 `switch` 整段结束后，若后面仍有语句，保留 1 个空行。空行加在整段控制流之后，不加在 `else`、`catch`、`finally` 或 `case` 之间。只排版已经确定的控制流，不规定分支数量，也不改写分支形态。
- 在对象字面量、状态、配置、内联对象类型和 `return { ... }` 中，跨多行的属性值与相邻的独立属性保留 1 个空行。对象方法与其他独立成员之间保留 1 个空行。同一子结构中紧密相关的属性保持紧邻。
- 顶层单行摘要注释与紧邻的 `const` 组成一个声明组，不同组之间保留 1 个空行。该分组不扩展到函数内部。声明没有注释时，不得补写说明，也不因此强制空行。

示例：

```ts
// 发布地址
const publishUrl = '/v1/publish';

// 预加载地址
const preloadUrl = '/v1/preload';

function resolve(input: Input) {
  const raw = readInput(input);
  const name = raw.name.trim();

  const path = parsePath(raw.url);

  const options = {
    name,
    path
  };

  if (!name) {
    return null;
  }

  const exact = findExact(name, {
    root: path
  });

  const fuzzy = findFuzzy(name);
  const matches = [exact, fuzzy].filter(Boolean);

  return build(options, matches);
}

async function save(input: Input, outputDir: string) {
  const payload = buildPayload(input);

  const response = await request(payload);
  const text = await response.text();

  assertOk(response);

  const data = parseData(text);

  await mkdir(outputDir);
  await writeSummary(data.id);

  await writeResult({
    id: data.id,
    body: text
  });

  await writeCache(data.id);
}

const state = {
  expiresAt: null,

  token: loadToken({
    key: 'session',
    fallback: null
  }),

  hydrate() {
    apply(this.token);
  },

  clear() {
    this.token = null;
  }
};
```
