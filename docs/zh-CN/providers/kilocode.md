---
sidebar_label: Kilo Code
---

# 使用 Kilo Code 的 API 提供商

:::warning

此文档目前仅针对 Anthropic 和 Claude Sonnet 3.7，并不反映使用 Kilo Code 作为提供商的简单性 - 它只是一个占位符

:::

Anthropic 是一家 AI 安全和研究公司，致力于构建可靠、可解释和可控制的 AI 系统。他们的 Claude 模型以其强大的推理能力、帮助性和诚实性而闻名。

**网站：** [https://www.anthropic.com/](https://www.anthropic.com/)

## 获取 API 密钥

1. **注册/登录：** 前往 [Anthropic 控制台](https://console.anthropic.com/)。创建账户或登录。
2. **导航到 API 密钥：** 前往 [API 密钥](https://console.anthropic.com/settings/keys) 部分。
3. **创建密钥：** 点击“创建密钥”。为您的密钥命名（例如，“Kilo Code”）。
4. **复制密钥：** **重要：** 立即复制 API 密钥。您将无法再次查看它。请妥善保存。

## 支持的模型

Kilo Code 支持以下 Anthropic Claude 模型：

*   `claude-3-7-sonnet-20250219`

有关每个模型功能的更多详细信息，请参阅 [Anthropic 的模型文档](https://docs.anthropic.com/en/docs/about-claude/models)。

## 在 Kilo Code 中配置

1. **打开 Kilo Code 设置：** 点击 Kilo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2. **选择提供商：** 从 "API 提供商" 下拉菜单中选择 "Anthropic"。
3. **输入 API 密钥：** 将您的 Anthropic API 密钥粘贴到 "Anthropic API 密钥" 字段中。
4. **选择模型：** 从 "模型" 下拉菜单中选择您想要的 Claude 模型。
5. **（可选）自定义基础 URL：** 如果您需要为 Anthropic API 使用自定义基础 URL，请勾选“使用自定义基础 URL”并输入 URL。大多数人不需要调整此项。

## 提示和注意事项

*   **提示缓存：** Claude 3 模型支持 [提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)，这可以显著降低重复提示的成本和延迟。
*   **上下文窗口：** Claude 模型具有较大的上下文窗口（200,000 个 token），允许您在提示中包含大量代码和上下文。
*   **定价：** 请参阅 [Anthropic 定价](https://www.anthropic.com/pricing) 页面获取最新定价信息。
*   **速率限制：** Anthropic 根据 [使用层级](https://docs.anthropic.com/en/api/rate-limits#requirements-to-advance-tier) 设置了严格的速率限制。如果您反复遇到速率限制，请考虑联系 Anthropic 销售或通过 [OpenRouter](/providers/openrouter) 或 [Requesty](/providers/requesty) 等其他提供商访问 Claude。