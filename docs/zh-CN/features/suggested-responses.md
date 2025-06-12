---
sidebar_label: 建议响应
---

import Codicon from '@site/src/components/Codicon';

# 建议响应

当 Kilo Code 需要更多信息来完成任务时，它会使用 [`ask_followup_question` 工具](/features/tools/ask-followup-question)。为了更轻松、更快速地响应，Kilo Code 通常会提供建议答案。

## 概述

建议响应以可点击按钮的形式出现在 Kilo Code 问题下方的聊天界面中。它们提供了与问题相关的预制答案，帮助你快速提供输入。

<img src="/docs/img/suggested-responses/suggested-responses.png" alt="Kilo Code 提问并附有建议响应按钮的示例" width="500" />

## 工作原理

1.  **问题出现**：Kilo Code 使用 `ask_followup_question` 工具提问。
2.  **显示建议**：如果 Kilo Code 提供了建议，它们会以按钮的形式出现在问题下方。
3.  **交互**：你可以通过两种方式与这些建议进行交互。

## 与建议交互

你有两种使用建议响应的选项：

1.  **直接选择**：
     *   **操作**：只需点击包含你想要提供的答案的按钮。
     *   **结果**：选定的答案会立即作为你的响应发送回 Kilo Code。如果某个建议完全符合你的意图，这是最快的回复方式。

2.  **发送前编辑**：
     *   **操作**：
         *   按住 `Shift` 并点击建议按钮。
         *   *或者*，将鼠标悬停在建议按钮上，点击出现的铅笔图标（<Codicon name="edit" />）。
     *   **结果**：建议的文本会复制到聊天输入框中。然后你可以根据需要修改文本，然后按下 Enter 键发送自定义的响应。当建议接近但需要稍作调整时，这很有用。

<img src="/docs/img/suggested-responses/suggested-responses-1.png" alt="聊天输入框显示从建议响应复制的文本，准备编辑" width="600" />

## 优势

*   **速度**：无需输入完整答案即可快速响应。
*   **清晰度**：建议通常阐明了 Kilo Code 所需的信息类型。
*   **灵活性**：在需要时编辑建议以提供精确、自定义的答案。

此功能在 Kilo Code 需要澄清时简化了交互，让你能够以最少的努力有效地指导任务。