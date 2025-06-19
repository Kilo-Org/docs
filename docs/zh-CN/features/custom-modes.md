# 自定义模式

Kilo Code允许你创建**自定义模式**来定制Kilo的行为以适应特定任务或工作流程。自定义模式可以是**全局的**（在所有项目中可用）或**项目特定的**（在单个项目中定义）。

## 为什么使用自定义模式？

* **专业化**：创建针对特定任务优化的模式，如"文档编写者"、"测试工程师"或"重构专家"
* **安全性**：限制模式对敏感文件或命令的访问。例如，"审查模式"可以限制为只读操作
* **实验性**：安全地尝试不同的提示和配置，而不会影响其他模式
* **团队协作**：与团队共享自定义模式以标准化工作流程

<img src="/docs/img/custom-modes/custom-modes.png" alt="自定义模式界面概览" width="400" />
*Kilo Code创建和管理自定义模式的界面*

## 自定义模式包含什么？

自定义模式允许你定义：

* **唯一名称和slug**：便于识别
* **角色定义**：放置在系统提示的开头，定义Kilo在该模式下的核心专业知识和个性。这个位置至关重要，因为它塑造了Kilo对任务的基本理解和处理方法
* **自定义指令**：添加到系统提示末尾，提供修改或优化Kilo行为的特定指南。与`.clinerules`文件（仅在末尾添加规则）不同，这种角色和指令的结构化放置允许更精细地控制Kilo的响应
* **允许的工具**：该模式可以使用的Kilo Code工具（例如：读取文件、写入文件、执行命令）
* **文件限制**：（可选）将文件访问限制为特定文件类型或模式（例如：仅允许编辑`.md`文件）

## 自定义模式配置（JSON格式）

全局和项目特定配置使用相同的JSON格式。每个配置文件包含一个模式定义的`customModes`数组：

```json
{
  "customModes": [
    {
      "slug": "mode-name",
      "name": "模式显示名称",
      "roleDefinition": "模式的角色和能力",
      "groups": ["read", "edit"],
      "customInstructions": "附加指南"
    }
  ]
}
```

### 必需属性

#### `slug`
* 模式的唯一标识符
* 使用小写字母、数字和连字符
* 保持简短和描述性
* 示例：`"docs-writer"`, `"test-engineer"`

#### `name`
* 在UI中显示的名称
* 可以包含空格和适当的大小写
* 示例：`"文档编写者"`, `"测试工程师"`

#### `roleDefinition`
* 模式角色和能力的详细描述
* 定义Kilo在该模式下的专业知识和个性
* 示例：`"你是一位专注于编写清晰文档的技术写作者"`

#### `groups`
* 允许的工具组数组
* 可用组：`"read"`, `"edit"`, `"browser"`, `"command"`, `"mcp"`
* 可以为`"edit"`组包含文件限制

##### 文件限制格式
```json
["edit", {
  "fileRegex": "\\.md$",
  "description": "仅限Markdown文件"
}]
```

### 理解文件限制

`fileRegex`属性使用正则表达式来控制模式可以编辑哪些文件：

* `\\.md$` - 匹配以".md"结尾的文件
* `\\.(test|spec)\\.(js|ts)$` - 匹配测试文件（例如"component.test.js"）
* `\\.(js|ts)$` - 匹配JavaScript和TypeScript文件

常见正则表达式模式：
* `\\.` - 匹配字面量点
* `(a|b)` - 匹配"a"或"b"
* `$` - 匹配文件名结尾

[了解更多关于正则表达式的知识](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

### 可选属性

#### `customInstructions`
* 模式的附加行为指南
* 示例：`"专注于解释概念和提供示例"`

#### `apiConfiguration`
* 自定义该模式的AI模型和参数的可选设置
* 允许针对特定任务优化模型选择
* 示例：`{"model": "gpt-4", "temperature": 0.2}`

### 模式特定的自定义指令文件

除了JSON中的`customInstructions`属性外，你还可以使用专门的文件来存储模式特定的指令：

1. 在工作区根目录创建名为`.clinerules-{mode-slug}`的文件
   * 将`{mode-slug}`替换为你的模式slug（例如`.clinerules-docs-writer`）
2. 将你的自定义指令添加到此文件
3. Kilo Code会自动将这些指令应用于指定模式

这种方法特别适用于：
* 保持冗长的指令与模式配置分离
* 使用版本控制管理指令
* 允许非技术团队成员修改指令而无需编辑JSON

注意：如果同时存在`.clinerules-{mode-slug}`和`customInstructions`属性，它们将被合并，文件内容会附加在JSON属性之后。

## 配置优先级

模式配置按以下顺序应用：

1. 项目级模式配置（来自`.kilocodemodes`）
2. 全局模式配置（来自`custom_modes.json`）
3. 默认模式配置

这意味着项目特定的配置将覆盖全局配置，而全局配置又将覆盖默认配置。

## 创建自定义模式

你有三种创建自定义模式的选项：

### 1. 询问Kilo!（推荐）

你可以通过要求Kilo Code快速创建一个基本自定义模式。例如：
```
创建一个名为"文档编写者"的新模式。它应该只能读取文件和编写Markdown文件。
```
Kilo Code将引导你完成整个过程。但是，对于微调模式或进行特定调整，你需要使用下面描述的"提示"标签页或手动配置方法。

:::info
#### 自定义模式创建设置
启用后，Kilo允许你使用类似'Make me a custom mode that...'的提示创建自定义模式。禁用此功能可在不需要此功能时减少约700 tokens的系统提示。禁用时，你仍然可以使用上方的+按钮或编辑相关配置JSON手动创建自定义模式。
<img src="/docs/img/custom-modes/custom-modes-1.png" alt="启用通过提示创建自定义模式的设置" width="600" />
你可以通过点击Kilo Code顶部菜单栏中的<Codicon name="notebook" />图标在提示设置中找到此设置。
:::

### 2. 使用提示标签页

1. **打开提示标签页**：点击Kilo Code顶部菜单栏中的<Codicon name="notebook" />图标
2. **创建新模式**：点击模式标题右侧的<Codicon name="add" />按钮
3. **填写字段**：

        <img src="/docs/img/custom-modes/custom-modes-2.png" alt="提示标签页中的自定义模式创建界面" width="600" />
        *显示名称、slug、保存位置、角色定义、可用工具和自定义指令字段的自定义模式创建界面*

    * **名称**：输入模式的显示名称
    * **Slug**：输入小写标识符（仅限字母、数字和连字符）
    * **保存位置**：选择全局（通过`custom_modes.json`，在所有工作区可用）或项目特定（在项目根目录的`.kilocodemodes`文件中）
    * **角色定义**：定义Kilo在该模式下的专业知识和个性（出现在系统提示的开头）
    * **可用工具**：选择该模式可以使用的工具
    * **自定义指令**：（可选）添加该模式特定的行为指南（出现在系统提示的末尾）
4. **创建模式**：点击"创建模式"按钮保存你的新模式

注意：文件类型限制只能通过手动配置添加。

### 3. 手动配置

你可以通过编辑提示标签页中的JSON文件来配置自定义模式：

全局和项目特定的配置都可以通过提示标签页编辑：

1. **打开提示标签页**：点击Kilo Code顶部菜单栏中的<Codicon name="notebook" />图标
2. **访问设置菜单**：点击模式标题右侧的<Codicon name="bracket" />按钮
3. **选择配置**：
    * 选择"编辑全局模式"来编辑`custom_modes.json`（在所有工作区可用）
    * 选择"编辑项目模式"来编辑项目根目录中的`.kilocodemodes`文件
4. **编辑配置**：修改打开的JSON文件
5. **保存更改**：Kilo Code会自动检测更改

## 配置示例

每个示例展示模式配置的不同方面：

### 基本文档编写者
```json
{
  "customModes": [{
    "slug": "docs-writer",
    "name": "文档编写者",
    "roleDefinition": "你是一位专注于编写清晰文档的技术写作者",
    "groups": [
      "read",
      ["edit", { "fileRegex": "\\.md$", "description": "仅限Markdown文件" }]
    ],
    "customInstructions": "专注于清晰的解释和示例"
  }]
}
```

### 具有文件限制的测试工程师
```json
{
  "customModes": [{
    "slug": "test-engineer",
    "name": "测试工程师",
    "roleDefinition": "你是一位专注于代码质量的测试工程师",
    "groups": [
      "read",
      ["edit", { "fileRegex": "\\.(test|spec)\\.(js|ts)$", "description": "仅限测试文件" }]
    ]
  }]
}
```

### 项目特定的模式覆盖
```json
{
  "customModes": [{
    "slug": "code",
    "name": "代码（项目特定）",
    "roleDefinition": "你是具有项目特定约束的软件工程师",
    "groups": [
      "read",
      ["edit", { "fileRegex": "\\.(js|ts)$", "description": "仅限JS/TS文件" }]
    ],
    "customInstructions": "专注于项目特定的JS/TS开发"
  }]
}
```

按照这些说明，你可以创建和管理自定义模式以增强与Kilo Code的工作流程。

## 理解自定义模式中的正则表达式

自定义模式中的正则表达式模式让你可以精确控制Kilo可以编辑哪些文件：

### 基本语法

当你在自定义模式中指定`fileRegex`时，你创建了一个文件路径必须匹配的模式：

```json
["edit", { "fileRegex": "\\.md$", "description": "仅限Markdown文件" }]
```

### 重要规则

- **双反斜杠**：在JSON中，反斜杠必须用另一个反斜杠转义。所以`\.md$`变成`\\.md$`
- **路径匹配**：模式匹配完整文件路径，而不仅仅是文件名
- **大小写敏感**：正则表达式模式默认区分大小写

### 常见模式示例

| 模式 | 匹配 | 不匹配 |
|---------|---------|---------------|
| `\\.md$` | `readme.md`, `docs/guide.md` | `script.js`, `readme.md.bak` |
| `^src/.*` | `src/app.js`, `src/components/button.tsx` | `lib/utils.js`, `test/src/mock.js` |
| `\\.(css\|scss)$` | `styles.css`, `theme.scss` | `styles.less`, `styles.css.map` |
| `docs/.*\\.md$` | `docs/guide.md`, `docs/api/reference.md` | `guide.md`, `src/docs/notes.md` |
| `^(?!.*(test\|spec)).*\\.js$` | `app.js`, `utils.js` | `app.test.js`, `utils.spec.js` |

### 模式构建块

- `\\.` - 匹配字面量点（句号）
- `$` - 匹配字符串结尾
- `^` - 匹配字符串开头
- `.*` - 匹配任何字符（换行符除外）零次或多次
- `(a|b)` - 匹配"a"或"b"
- `(?!...)` - 否定前瞻（排除匹配）

### 测试你的模式

在将正则表达式模式应用于自定义模式之前：
1. 在示例文件路径上测试它以确保它匹配你期望的内容
2. 记住在JSON中每个反斜杠都需要加倍（`\d`变成`\\d`）
3. 从更简单的模式开始，逐步增加复杂性

:::tip
### 让Kilo构建你的正则表达式模式
与其手动编写复杂的正则表达式模式，你可以要求Kilo为你创建它们！只需描述你想要包含或排除哪些文件：
```
创建一个匹配JavaScript文件但排除测试文件的正则表达式模式
```
Kilo将生成带有适当JSON转义的适当模式。
:::

## 社区画廊
准备好探索更多？查看[Show and Tell](https://github.com/Kilo-Org/kilocode/discussions/categories/show-and-tell)来发现和分享社区创建的自定义模式！