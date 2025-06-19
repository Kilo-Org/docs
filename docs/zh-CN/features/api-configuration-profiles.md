# API 配置档案

API 配置档案允许你创建和切换不同的 AI 设置集合。每个档案可以为每个模式配置不同的设置，让你根据任务优化使用体验。

:::info
拥有多个配置档案可以让你快速在不同 AI 提供商、模型和设置之间切换，而无需每次更改设置时重新配置所有内容。
:::

## 工作原理

配置档案可以包含以下内容：
- API 提供商（OpenAI、Anthropic、OpenRouter、Glama 等）
- API 密钥和认证信息
- 模型选择（o3-mini-high、Claude 3.7 Sonnet、DeepSeek R1 等）
- [温度设置](/features/model-temperature)，用于控制响应随机性
- 思考预算
- 提供商特定设置

请注意，可用设置因提供商和模型而异。每个提供商提供不同的配置选项，即使在同一提供商中，不同模型也可能支持不同的参数范围或功能。

## 创建和管理档案

### 创建档案

1. 打开设置，点击齿轮图标 <Codicon name="gear" /> → 提供商
2. 点击档案选择器旁边的 "+" 按钮

   <img src="/docs/img/api-configuration-profiles/api-configuration-profiles-1.png" alt="带有加号按钮的档案选择器" width="550" />
3. 输入新档案的名称

   <img src="/docs/img/api-configuration-profiles/api-configuration-profiles.png" alt="创建新档案对话框" width="550" />
4. 配置档案设置：
   - 选择你的 API 提供商

      <img src="/docs/img/api-configuration-profiles/api-configuration-profiles-2.png" alt="提供商选择下拉菜单" width="550" />
   - 输入 API 密钥

      <img src="/docs/img/api-configuration-profiles/api-configuration-profiles-3.png" alt="API 密钥输入字段" width="550" />
   - 选择模型

      <img src="/docs/img/api-configuration-profiles/api-configuration-profiles-8.png" alt="模型选择界面" width="550" />
   - 调整模型参数

      <img src="/docs/img/api-configuration-profiles/api-configuration-profiles-5.png" alt="模型参数调整控件" width="550" />

### 切换档案

切换档案的两种方式：
1. 从设置面板：从下拉菜单中选择不同的档案

   <img src="/docs/img/api-configuration-profiles/api-configuration-profiles-7.png" alt="设置中的档案选择下拉菜单" width="550" />
2. 在聊天过程中：访问聊天界面中的 API 配置下拉菜单

   <img src="/docs/img/api-configuration-profiles/api-configuration-profiles-6.png" alt="聊天界面中的 API 配置下拉菜单" width="550" />

### 固定和排序档案

API 配置下拉菜单现在支持固定常用档案以便快速访问：

1. 将鼠标悬停在下拉菜单中的任何档案上以显示固定图标
2. 点击固定图标将档案添加到固定列表
3. 固定的档案将按字母顺序显示在下拉菜单的顶部
4. 未固定的档案将显示在分隔符下方，同样按字母顺序排序
5. 你可以再次点击该图标取消固定档案

<img src="/docs/img/api-configuration-profiles/api-configuration-profiles-4.png" alt="固定 API 配置档案" width="550" />

此功能使你更容易在常用配置之间切换，特别是在你拥有许多配置时。

### 编辑和删除档案

<img src="/docs/img/api-configuration-profiles/api-configuration-profiles-10.png" alt="档案编辑界面" width="550" />
- 在设置中选择档案以修改任何设置
- 点击铅笔图标重命名档案
- 点击垃圾桶图标删除档案（不能删除最后一个档案）

## 将档案与模式关联

在 <Codicon name="notebook" /> 提示标签中，你可以显式地将特定配置档案与每个模式关联。系统还会自动记住你上次与每个模式一起使用的档案，从而提高工作效率。

<img src="/docs/img/api-configuration-profiles/api-configuration-profiles-11.png" alt="提示标签中的档案-模式关联界面" width="550" />

## 安全提示

API 密钥安全地存储在 VSCode 的 Secret Storage 中，永远不会以明文形式暴露。

## 相关功能

- 与你创建的[自定义模式](/features/custom-modes)配合使用
- 集成[本地模型](/advanced-usage/local-models)以进行离线工作
- 支持每个模式的[温度设置](/features/model-temperature)
- 通过[速率限制和使用跟踪](/advanced-usage/rate-limits-costs)增强成本管理