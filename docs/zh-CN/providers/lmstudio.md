---
sidebar_label: LM Studio
---

# 使用 LM Studio 的 API 提供商

LM Studio 是一个本地运行的 AI 模型管理工具，允许您在本地计算机上运行各种开源 LLM 模型，并为其提供 API 访问。

**网站：** [https://lmstudio.ai/](https://lmstudio.ai/)

## 设置 LM Studio

1. **下载并安装：** 从 [LM Studio 网站](https://lmstudio.ai/) 下载并安装适用于您操作系统的版本。
2. **下载模型：** 在 LM Studio 中，从 Hugging Face 下载您想要使用的模型。建议使用经过测试的模型，如 Mistral 7B 或 Llama 2。
3. **启动服务器：** 在 LM Studio 中，点击“Server”选项卡，然后点击“Start Server”。默认情况下，服务器将在 `http://localhost:1234` 上运行。

## 支持的模型

Kilo Code 支持以下 LM Studio 模型：

*   `mistral-7b-instruct-v0.1`
*   `llama-2-7b-chat`
*   `llama-2-13b-chat`
*   `llama-2-70b-chat`

有关每个模型功能的更多详细信息，请参阅 [LM Studio 的文档](https://lmstudio.ai/docs)。

## 在 Kilo Code 中配置

1. **打开 Kilo Code 设置：** 点击 Kilo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2. **选择提供商：** 从 "API 提供商" 下拉菜单中选择 "LM Studio"。
3. **输入 API 密钥：** 将您的 LM Studio API 密钥粘贴到 "LM Studio API 密钥" 字段中。如果未设置 API 密钥，可以留空。
4. **选择模型：** 从 "模型" 下拉菜单中选择您想要的模型。
5. **（可选）自定义基础 URL：** 如果您需要为 LM Studio API 使用自定义基础 URL，请勾选“使用自定义基础 URL”并输入 URL。大多数人不需要调整此项。

## 提示和注意事项

*   **本地运行：** LM Studio 允许您在本地运行模型，这对于隐私和安全性至关重要。
*   **模型选择：** 选择适合您任务的模型大小。较大的模型需要更多的计算资源，但可能提供更好的结果。
*   **硬件要求：** 确保您的计算机具有足够的 RAM 和 GPU 内存来运行您选择的模型。
*   **速率限制：** LM Studio 没有严格的速率限制，但您的硬件可能会限制性能。