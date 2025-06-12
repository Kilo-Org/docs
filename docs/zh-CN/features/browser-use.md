# 浏览器使用

Kilo Code 提供了先进的浏览器自动化功能，让你可以直接在 VS Code 中与网站进行交互。该功能使你无需离开开发环境即可测试 Web 应用程序、自动化浏览器任务并捕获屏幕截图。

<video width="100%" controls>
  <source src="/docs/img/browser-use/Roo-Code-Browser-Use.mp4#t=0.001" type="video/mp4"></source>
  你的浏览器不支持视频标签。
</video>

:::info 模型支持要求
Kilo Code 中的浏览器功能需要使用 Claude Sonnet 3.5 或 3.7 模型
:::

## 浏览器功能的工作原理

默认情况下，Kilo Code 使用内置浏览器，该浏览器：
- 当你要求 Kilo 访问网站时会自动启动
- 捕获网页截图
- 允许 Kilo 与网页元素交互
- 在后台隐形运行

所有这些操作都在 VS Code 中直接完成，无需任何设置。

## 使用浏览器功能

典型的浏览器交互遵循以下模式：

1. 让 Kilo 访问一个网站
2. Kilo 启动浏览器并显示截图
3. 请求其他操作（点击、输入、滚动）
4. Kilo 在完成后关闭浏览器

例如：

```
打开浏览器并查看我们的网站。
```

```
你能检查一下我在 https://kilocode.ai 的网站是否显示正确吗？
```

```
浏览 http://localhost:3000，向下滚动到页面底部并检查页脚信息是否显示正确。
```

<img src="/docs/img/browser-use/browser-use-1.png" alt="浏览器使用示例" width="300" />

## 浏览器操作的工作原理

`browser_action` 工具控制一个浏览器实例，在每次操作后返回截图和控制台日志，让你可以看到交互结果。

关键特性：
- 每个浏览器会话必须以 `launch` 开始，以 `close` 结束
- 每条消息只能使用一个浏览器操作
- 当浏览器处于活动状态时，不能使用其他工具
- 在执行下一个操作之前，必须等待响应（截图和日志）

### 可用的浏览器操作

| 操作 | 描述 | 使用场景 |
|--------|-------------|------------|
| `launch` | 在 URL 处打开浏览器 | 开始新的浏览器会话 |
| `click` | 在特定坐标处点击 | 与按钮、链接等交互 |
| `type` | 在活动元素中输入文本 | 填写表单、搜索框 |
| `scroll_down` | 向下滚动一页 | 查看折叠内容 |
| `scroll_up` | 向上滚动一页 | 返回之前的内容 |
| `close` | 关闭浏览器 | 结束浏览器会话 |

## 浏览器功能配置/设置

:::info 默认浏览器设置
- **启用浏览器工具**：已启用
- **视口大小**：小桌面 (900x600)
- **截图质量**：75%
- **使用远程浏览器连接**：已禁用
:::

### 访问设置

要更改 Kilo 中的浏览器/电脑使用设置：

1. 点击齿轮图标 <Codicon name="gear" /> → 浏览器/电脑使用 打开设置

   <img src="/docs/img/browser-use/browser-use.png" alt="浏览器设置菜单" width="600" />

### 启用/禁用浏览器功能

**用途**：主开关，启用 Kilo 使用 Puppeteer 控制的浏览器与网站交互。

要更改此设置：
1. 在浏览器/电脑使用设置中勾选或取消勾选 "启用浏览器工具" 复选框

   <img src="/docs/img/browser-use/browser-use-2.png" alt="启用浏览器工具设置" width="300" />

### 视口大小

**用途**：确定 Kilo Code 使用的浏览器会话的分辨率。

**权衡**：更高的值提供更大的视口，但会增加 token 使用量。

要更改此设置：
1. 在浏览器/电脑使用设置中点击 "视口大小" 下的下拉菜单
2. 选择以下选项之一：
   - 大桌面 (1280x800)
   - 小桌面 (900x600) - 默认
   - 平板 (768x1024)
   - 手机 (360x640)
2. 选择所需分辨率。

   <img src="/docs/img/browser-use/browser-use-3.png" alt="视口大小设置" width="600" />

### 截图质量

**用途**：控制浏览器截图的 WebP 压缩质量。

**权衡**：更高的值提供更清晰的截图，但会增加 token 使用量。

要更改此设置：
1. 在浏览器/电脑使用设置中调整 "截图质量" 下的滑块
2. 设置 1-100% 之间的值（默认为 75%）
3. 更高的值提供更清晰的截图，但会增加 token 使用量：
   - 40-50%：适用于基本文本网站
   - 60-70%：适用于大多数一般浏览
   - 80%+：当需要精细视觉细节时使用

   <img src="/docs/img/browser-use/browser-use-4.png" alt="截图质量设置" width="600" />

### 远程浏览器连接

**用途**：将 Kilo 连接到现有的 Chrome 浏览器，而不是使用内置浏览器。

**优点**：
- 适用于容器化环境和远程开发工作流
- 在浏览器使用之间保持认证会话
- 消除重复登录步骤
- 允许使用具有特定扩展的自定义浏览器配置文件

**要求**：Chrome 必须启用远程调试。

要启用此功能：
1. 在浏览器/电脑使用设置中勾选 "使用远程浏览器连接" 框
2. 点击 "测试连接" 进行验证

   <img src="/docs/img/browser-use/browser-use-5.png" alt="远程浏览器连接设置" width="600" />

#### 常见用例

- **DevContainers**：从容器化的 VS Code 连接到主机 Chrome 浏览器
- **远程开发**：使用本地 Chrome 与远程 VS Code 服务器
- **自定义 Chrome 配置文件**：使用具有特定扩展和设置的配置文件

#### 连接到可见的 Chrome 窗口

连接到可见的 Chrome 窗口以实时观察 Kilo 的交互：

**macOS**
```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-debug --no-first-run
```

**Windows**
```bash
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir=C:\chrome-debug --no-first-run
```

**Linux**
```bash
google-chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-debug --no-first-run
```