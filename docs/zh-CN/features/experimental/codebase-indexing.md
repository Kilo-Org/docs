import Codicon from '@site/src/components/Codicon';

# 代码库索引

**⚠️ 实验性功能：** 该功能正在积极开发中，可能在未来的版本中发生重大变化。

代码库索引功能使用 AI 嵌入实现对整个项目的语义代码搜索。它不局限于精确的文本匹配，而是理解查询的_含义_，帮助 Kilo Code 即使在你不知道具体函数名或文件位置的情况下也能找到相关代码。

<img src="/docs/img/codebase-indexing/codebase-indexing.png" alt="代码库索引设置" width="800" />

## 功能概述

启用后，索引系统会：

1. **解析代码**：使用 Tree-sitter 识别语义块（函数、类、方法）
2. **创建嵌入**：使用 AI 模型为每个代码块生成嵌入
3. **存储向量**：将向量存储在 Qdrant 数据库中以便快速相似性搜索
4. **提供 [`codebase_search`](/advanced-usage/available-tools/codebase-search) 工具**：为 Kilo Code 提供智能代码发现功能

这使得你可以使用自然语言查询，如“用户认证逻辑”或“数据库连接处理”，在整个项目中找到相关代码。

## 主要优势

- **语义搜索**：通过含义而非关键字查找代码
- **增强的 AI 理解**：Kilo Code 能更好地理解和处理你的代码库
- **跨项目发现**：搜索所有文件，而不仅仅是打开的文件
- **模式识别**：定位相似的实现和代码模式

## 设置要求

### 嵌入提供者

选择以下选项之一来生成嵌入：

**OpenAI（推荐）**

- 需要 OpenAI API 密钥
- 支持所有 OpenAI 嵌入模型
- 默认：`text-embedding-3-small`
- 每批最多处理 100,000 个 token

**Ollama（本地）**

- 需要本地 Ollama 安装
- 无需 API 费用或网络依赖
- 支持任何与 Ollama 兼容的嵌入模型
- 需要配置 Ollama 基础 URL

### 向量数据库

**Qdrant** 是存储和搜索嵌入的必需组件：

- **本地**：`http://localhost:6333`（推荐用于测试）
- **云端**：Qdrant Cloud 或自托管实例
- **认证**：可选的 API 密钥，用于安全部署

## 设置 Qdrant

### 快速本地设置

**使用 Docker：**

```bash
docker run -p 6333:6333 qdrant/qdrant
```

**使用 Docker Compose：**

```yaml
version: '3.8'
services:
  qdrant:
    image: qdrant/qdrant
    ports:
      - '6333:6333'
    volumes:
      - qdrant_storage:/qdrant/storage
volumes:
  qdrant_storage:
```

### 生产部署

对于团队或生产环境：

- [Qdrant Cloud](https://cloud.qdrant.io/) - 托管服务
- 在 AWS、GCP 或 Azure 上自托管
- 本地服务器并配置网络访问以支持团队共享

## 配置

1. 打开 Kilo Code 设置（<Codicon name="gear" /> 图标）
2. 导航到 **实验性** 部分
3. 启用 **“启用代码库索引”**
4. 配置嵌入提供者：
   - **OpenAI**：输入 API 密钥并选择模型
   - **Ollama**：输入基础 URL 并选择模型
5. 设置 Qdrant URL 和可选的 API 密钥
6. 点击 **保存** 开始初始索引

## 理解索引状态

界面会显示实时状态，并用颜色标识：

- **待机**（灰色）：未运行，等待配置
- **索引中**（黄色）：正在处理文件
- **已索引**（绿色）：最新且可进行搜索
- **错误**（红色）：需要关注失败状态

## 文件处理方式

### 智能代码解析

- **Tree-sitter 集成**：使用 AST 解析识别语义代码块
- **语言支持**：Tree-sitter 支持的所有语言
- **回退**：对不支持的文件类型使用基于行的分块
- **块大小**：
  - 最小：100 字符
  - 最大：1,000 字符
  - 智能分割大型函数

### 自动文件过滤

索引器会自动排除：

- 二进制文件和图像
- 大文件（>1MB）
- Git 仓库（`.git` 文件夹）
- 依赖项（`node_modules`、`vendor` 等）
- 符合 `.gitignore` 和 `.kilocode` 模式的文件

### 增量更新

- **文件监控**：监视工作区的变化
- **智能更新**：仅重新处理修改过的文件
- **基于哈希的缓存**：避免重新处理未更改的内容
- **分支切换**：自动处理 Git 分支变更

## 最佳实践

### 模型选择

**对于 OpenAI：**

- **`text-embedding-3-small`**：性能与成本的最佳平衡
- **`text-embedding-3-large`**：更高的准确性，成本增加 5 倍
- **`text-embedding-ada-002`**：旧版模型，成本较低

**对于 Ollama：**

- **`mxbai-embed-large`**：最大且最高质量的嵌入模型。
- **`nomic-embed-text`**：性能与嵌入质量的最佳平衡。
- **`all-minilm`**：紧凑模型，质量较低但性能更快。

### 安全考虑

- **API 密钥**：安全存储在 VS Code 的加密存储中
- **代码隐私**：仅发送小段代码片段进行嵌入（非完整文件）
- **本地处理**：所有解析均在本地进行
- **Qdrant 安全性**：生产部署时使用认证

## 当前限制

- **文件大小**：每个文件最大 1MB
- **Markdown**：由于解析复杂性，目前不支持
- **单一工作区**：一次只能处理一个工作区
- **依赖项**：需要外部服务（嵌入提供者 + Qdrant）
- **语言覆盖**：仅限于 Tree-sitter 支持的语言

## 使用搜索功能

索引完成后，Kilo Code 可以使用 [`codebase_search`](/advanced-usage/available-tools/codebase-search) 工具查找相关代码：

**示例查询：**

- “用户认证是如何处理的？”
- “数据库连接设置”
- “错误处理模式”
- “API 端点定义”

该工具为 Kilo Code 提供：

- 相关代码片段
- 文件路径和行号
- 相似度分数
- 上下文信息

## 隐私与安全

- **代码保留在本地**：仅发送小段代码片段进行嵌入
- **嵌入为数值**：非人类可读的表示
- **安全存储**：API 密钥在 VS Code 存储中加密
- **本地选项**：使用 Ollama 实现完全本地处理
- **访问控制**：尊重现有文件权限

## 未来改进

计划中的改进包括：

- 增加更多嵌入提供者
- 改进 Markdown 和文档支持
- 多工作区索引
- 增强过滤和配置选项
- 团队共享功能
- 与 VS Code 原生搜索集成