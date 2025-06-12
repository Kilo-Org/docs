---
sidebar_label: 'Footgun Prompting'
---

# Footgun Prompting：覆盖系统提示

Footgun Prompting，也称为覆盖系统提示，允许高级用户完全替换特定 Kilo Code 模式的默认系统提示。这提供了对 AI 行为的精细控制，但会绕过内置的安全机制。

:::info **footgun** *(名词)*

1. *(编程俚语，幽默，贬义)* 任何可能导致程序员搬起石头砸自己脚的功能。

> 系统提示覆盖被称为 "footgun"，因为在不深入了解其核心指令的情况下修改它可能会导致意外或损坏的行为，特别是在工具使用和响应一致性方面。

:::

## 工作原理

1. **覆盖文件：** 在项目根目录下创建一个名为 `.kilo/system-prompt-{mode-slug}` 的文件（例如，对于 Code 模式，文件名为 `.kilo/system-prompt-code`）。
2. **内容：** 该文件的内容将成为该特定模式的新系统提示。
3. **激活：** Kilo Code 会自动检测此文件。当文件存在时，它将替换大部分标准系统提示部分。
4. **保留部分：** 只有核心的 `roleDefinition` 和你为该模式设置的 `customInstructions` 会与覆盖内容一起保留。标准部分如工具描述、规则和功能将被绕过。
5. **构建：** 最终发送给模型的提示如下：
    ```
    ${roleDefinition}

    ${content_of_your_override_file}

    ${customInstructions}
    ```

## 访问该功能

你可以在 Kilo Code UI 中找到该功能和说明：

1. 点击 Kilo Code 顶部菜单栏中的 <Codicon name="notebook" /> 图标。
2. 展开 **"高级：覆盖系统提示"** 部分。
3. 点击说明中的文件路径链接，将在 VS Code 中打开或创建当前所选模式的覆盖文件。

<img src="/docs/img/footgun-prompting/footgun-prompting.png" alt="UI 显示高级：覆盖系统提示部分" width="500" />


## 关键注意事项与警告

-   **目标用户：** 最适合对 Kilo Code 提示系统和修改核心指令的影响有深入了解的用户。
-   **对功能的影响：** 自定义提示会覆盖标准指令，包括工具使用和响应一致性的指令。如果不小心处理，可能会导致意外行为或错误。
-   **模式特定：** 每个覆盖文件仅适用于文件名中指定的模式（`{mode-slug}`）。
-   **无文件，无覆盖：** 如果 `.kilo/system-prompt-{mode-slug}` 文件不存在，Kilo Code 将使用该模式的标准系统提示生成过程。
-   **目录创建：** Kilo Code 会在读取或创建覆盖文件之前确保 `.kilo` 目录存在。

请谨慎使用此功能。虽然它对于自定义非常强大，但错误的实现可能会显著降低 Kilo Code 在受影响模式下的性能和可靠性。