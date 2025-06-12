# 终端 Shell 集成

终端 Shell 集成是 Kilo Code 的核心功能，使其能够在您的终端执行命令并智能处理输出。这种 AI 与开发环境的双向通信解锁了强大的自动化能力。

## Shell 集成是什么？

Shell 集成在 Kilo Code 中自动启用，无需任何设置即可直接连接至终端命令执行生命周期。该内置功能允许 Kilo Code：

- 通过 [`execute_command`](/features/tools/execute-command) 工具代您执行命令
- 实时读取命令输出，无需手动复制粘贴
- 自动检测并修复运行中的应用程序错误
- 通过退出代码判断命令执行成败
- 跟踪工作目录变化
- 智能响应终端输出而无需用户干预

当 Kilo Code 需要执行安装依赖、启动开发服务器或分析构建错误等任务时，Shell 集成会在后台确保交互流程顺畅高效。

## 开始使用 Shell 集成

Shell 集成已内置在 Kilo Code 中，多数情况下自动工作。若出现"Shell 集成不可用"提示或命令执行问题，可尝试以下解决方案：

1. **更新 VSCode/Cursor** 至最新版本（要求 VSCode 1.93+）
2. **选择兼容的 Shell**：命令面板（`Ctrl+Shift+P` 或 `Cmd+Shift+P`）→ "Terminal: Select Default Profile" → 选择 bash、zsh、PowerShell 或 fish
3. **Windows PowerShell 用户**：运行 `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` 后重启 VSCode
4. **WSL 用户**：在 `~/.bashrc` 中添加 `. "$(code --locate-shell-integration-path bash)"`

## 终端集成设置

Kilo Code 提供多项设置来微调 Shell 集成。通过 Kilo Code 侧边栏的 Settings → Terminal 访问这些设置。

### 基础设置

#### 终端输出限制
<img src="/docs/img/shell-integration/terminal-output-limit.png" alt="终端输出限制滑块设置为500行" width="500" />
控制从终端捕获的最大行数。超出限制时，保留开头20%和结尾80%的内容，中间显示截断提示。在保持上下文的同时避免过度消耗 token。默认值： 500 行。
控制从终端捕获的最大行数。超出限制时，系统会从内容中间删除部分行以节省 token。默认值：500 行。

#### Shell 集成超时
<img src="/docs/img/shell-integration/shell-integration-timeout.png" alt="Shell集成超时滑块设置为15秒" width="500" />

等待 Shell 集成初始化的最长时间。若遇到"Shell 集成不可用"错误可增加此值。默认：15秒。

#### 终端命令延迟
<img src="/docs/img/shell-integration/terminal-command-delay.png" alt="终端命令延迟滑块设置为0毫秒" width="500" />

在命令执行后添加短暂停顿，确保 Kilo Code 正确捕获所有输出。此设置对不同操作系统和 Shell 配置的可靠性影响显著：

- **默认值**：0毫秒
- **常用值**：
  * 0毫秒：新版 VSCode 用户最佳选择
  * 50毫秒：历史默认值，对多数用户仍有效
  * 150毫秒：PowerShell 用户推荐值
- **注意**：最佳值可能取决于：
  * VSCode 版本
  * Shell 定制程度（如 oh-my-zsh、powerlevel10k 等）
  * 操作系统和环境

### 高级设置

:::info 重要提示
**修改后需重启终端**

高级终端设置的变更仅在重启终端后生效。重启步骤：

1. 点击终端面板的垃圾桶图标关闭当前终端
2. 通过 Terminal → New Terminal 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> 新建终端

修改任何设置后请务必重启所有打开的终端。
:::

#### PowerShell 计数器解决方案
<img src="/docs/img/shell-integration/power-shell-workaround.png" alt="PowerShell计数器解决方案复选框" width="600" />

解决 PowerShell 连续执行相同命令的问题。若发现 Kilo Code 无法在 PowerShell 中连续运行相同命令，请启用此选项。

#### 清除 ZSH EOL 标记
<img src="/docs/img/shell-integration/clear-zsh-eol-mark.png" alt="清除ZSH行尾标记复选框" width="600" />

阻止 ZSH 在输出行尾添加可能干扰 Kilo Code 读取结果的特殊字符。

#### Oh My Zsh 集成
<img src="/docs/img/shell-integration/oh-my-zsh.png" alt="启用Oh My Zsh集成复选框" width="600" />

优化与流行 Shell 框架 [Oh My Zsh](https://ohmyz.sh/) 的兼容性。使用 Oh My Zsh 时若遇到终端问题请开启。

#### Powerlevel10k 集成
<img src="/docs/img/shell-integration/power10k.png" alt="启用Powerlevel10k集成复选框" width="600" />

提升与 ZSH 主题 Powerlevel10k 的兼容性。若花哨的终端提示符导致问题请启用。

#### ZDOTDIR 处理
<img src="/docs/img/shell-integration/zdotdir.png" alt="启用ZDOTDIR处理复选框" width="600" />

使 Kilo Code 能兼容自定义 ZSH 配置，同时不影响您的个人 Shell 设置。

## 故障排除

### PowerShell 执行策略（Windows）

PowerShell 默认限制脚本执行。配置方法：

1. 以管理员身份打开 PowerShell
2. 查看当前策略：`Get-ExecutionPolicy`
3. 设置合适策略：`Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`

常用策略：
- `Restricted`：禁止所有脚本（默认）
- `RemoteSigned`：本地脚本可运行，下载脚本需签名
- `Unrestricted`：所有脚本带警告运行
- `AllSigned`：所有脚本必须签名

### 手动安装 Shell 集成

若自动集成失败，请将对应代码添加到 Shell 配置中：

**Bash**（`~/.bashrc`）：
```bash
[[ "$TERM_PROGRAM" == "vscode" ]] && . "$(code --locate-shell-integration-path bash)"
```

**Zsh**（`~/.zshrc`）：
```bash
[[ "$TERM_PROGRAM" == "vscode" ]] && . "$(code --locate-shell-integration-path zsh)"
```

**PowerShell**（`$Profile`）：
```powershell
if ($env:TERM_PROGRAM -eq "vscode") { . "$(code --locate-shell-integration-path pwsh)" }
```

**Fish**（`~/.config/fish/config.fish`）：
```fish
string match -q "$TERM_PROGRAM" "vscode"; and . (code --locate-shell-integration-path fish)
```

## 终端定制问题

若使用终端定制工具：

**Powerlevel10k**：
```bash
# 在 ~/.zshrc 中 sourcing powerlevel10k 之前添加
typeset -g POWERLEVEL9K_TERM_SHELL_INTEGRATION=true
```

**替代方案**：在 Kilo Code 设置中启用 Powerlevel10k 集成选项

## Shell 集成状态验证

使用以下命令确认集成是否激活：

**Bash**：
```bash
set | grep -i '[16]33;'
echo "$PROMPT_COMMAND" | grep vsc
trap -p DEBUG | grep vsc
```

**Zsh**：
```zsh
functions | grep -i vsc
typeset -p precmd_functions preexec_functions
```

**PowerShell**：
```powershell
Get-Command -Name "*VSC*" -CommandType Function
Get-Content Function:\Prompt | Select-String "VSCode"
```

**Fish**：
```fish
functions | grep -i vsc
functions fish_prompt | grep -i vsc
```

集成激活的视觉指示：
1. 终端标题栏中的集成标识
2. 命令检测高亮显示
3. 终端标题中的工作目录更新
4. 命令执行时间和退出码显示

## WSL 终端集成方案

使用 Windows Subsystem for Linux (WSL) 时，有两种集成方式：

### 方案1：Windows版VSCode + WSL终端
特点：
- VSCode 运行于 Windows
- 使用 WSL 终端桥接
- 需手动确保 WSL 环境中加载了集成脚本：
  ```bash
  source "$(code --locate-shell-integration-path <shell>)"
  ```

### 方法二：在WSL中运行VSCode  

在此设置中：  
- 你可以使用 `code .` 命令直接从WSL中启动VSCode  
- VSCode服务端在Linux环境中本地运行  
- 可直接访问Linux文件系统和工具  
- 为Shell集成提供更好的性能和可靠性  
- 由于VSCode在Linux环境中本地运行，Shell集成会自动加载  
- 这是WSL开发的推荐方式  

要实现WSL的最佳Shell集成，我们建议：  
1. 打开你的WSL发行版  
2. 导航到项目目录  
3. 使用 `code .` 启动VSCode  
4. 使用VSCode内的集成终端

## 常见问题解决方案

### Windows 上 Fish + Cygwin 的 VS Code Shell 集成

对于在 Cygwin 环境中运行 Fish 终端的 Windows 用户，以下是配置 VS Code Shell 集成的方法：

1. **（可选）定位 Shell 集成脚本**  
   在 **VS Code 内**打开 Fish 终端并执行：
   ```bash
   code --locate-shell-integration-path fish
   ```
   该命令会输出 `shellIntegration.fish` 脚本的路径，请记录此路径。

2. **更新 Fish 配置**  
   编辑 `config.fish` 文件（通常位于 Cygwin 主目录下的 `~/.config/fish/config.fish`）。添加以下行，建议放在 `if status is-interactive` 块内或文件末尾：

   ```fish
   # 示例 config.fish 结构
   if status is-interactive
       # 其他交互式 shell 配置...
       # 自动定位集成脚本：
       string match -q "$TERM_PROGRAM" "vscode"; and . (code --locate-shell-integration-path fish)

       # 若上述方法失败：
       # 手动引入 VS Code shell 集成脚本
       # 重要：将下方示例路径替换为步骤 1 中获取的实际路径
       # 确保路径格式符合 Cygwin 规范（例如 /cygdrive/c/...）
       # source "/cygdrive/c/Users/你的用户名/.vscode/extensions/..../shellIntegration.fish"
   end
   ```
   *请将示例路径替换为步骤 1 中的实际路径，并确保格式符合 Cygwin 要求。*

3. **配置 VS Code 终端配置文件**  
   打开 VS Code 的 `settings.json` 文件（Ctrl+Shift+P → "Preferences: Open User Settings (JSON)"）。在 `terminal.integrated.profiles.windows` 下更新或添加 Fish 配置文件：

   ```json
   {
     // ... 其他设置 ...

     "terminal.integrated.profiles.windows": {
       // ... 其他配置文件 ...

       // 推荐：使用 bash.exe 以登录 shell 方式启动 fish
       "fish": {
         "path": "C:\\cygwin64\\bin\\bash.exe", // 或你的 Cygwin bash 路径
         "args": [
           "--login", // 确保执行登录脚本（对 Cygwin 环境很重要）
           "-i",      // 确保 bash 以交互模式运行
           "-c",
           "exec fish" // 用 fish 替换 bash 进程
         ],
         "icon": "terminal-bash" // 可选：使用可识别的图标
       },
       // 备选方案（若上述方法失败）：直接启动 fish
       "fish-direct": {
         "path": "C:\\cygwin64\\bin\\fish.exe", // 确保此路径在 Windows PATH 中或提供完整路径
         // 此处使用 'options' 而非 'args'，否则可能遇到 "终端进程终止，退出代码 1" 错误
         "options": ["-l", "-c"], // 示例：登录和交互标志
         "icon": "terminal-fish" // 可选：使用 fish 图标
       }
     },

     // 可选：如需将 fish 设置为默认终端
     // "terminal.integrated.defaultProfile.windows": "fish", // 或 "fish-direct"，取决于你使用的配置

     // ... 其他设置 ...
   }
   ```
   *注意：在 Cygwin 环境中，使用 `bash.exe --login -i -c "exec fish"` 通常能更可靠地确保在启动 `fish` 前正确设置环境。但若此方法无效，请尝试 `fish-direct` 配置文件。*

4. **重启 VS Code**  
   完全关闭并重新打开 Visual Studio Code 以应用更改。

5. **验证配置**  
   在 VS Code 中打开新的 Fish 终端。Shell 集成功能（如命令装饰、更便捷的命令历史导航等）应已激活。你可以通过运行简单命令（如 `echo "Hello from integrated Fish!"`）测试基本功能。  
   <img src="/img/shell-integration/shell-integration-8.png" alt="Fish Cygwin 集成示例" width="600" />

此配置在使用 Cygwin、Fish 和 Starship 提示符的 Windows 系统上运行稳定，适用于类似环境的用户。

### VSCode 1.98 之后 Shell 集成失败问题

**问题描述**：在将 VSCode 更新到 1.98 版本之后，Shell 集成功能可能会失效，并显示错误信息 "VSCE output start escape sequence (]633;C or ]133;C) not received"。

**解决方案**：
1. **设置终端命令延迟**
   - 在 Kilo Code 设置中将终端命令延迟（Terminal Command Delay）设为 50ms
   - 修改设置后重启所有终端
   - 该设置模拟了旧版本的默认行为，可能会解决问题。不过部分用户反馈设为 0ms 效果更好。这是针对 VSCode 上游问题的临时解决方案

2. **回退 VSCode 版本**
   - 从 [VSCode 更新页面](https://code.visualstudio.com/updates/v1_98) 下载 VSCode v1.98 版本
   - 替换当前的 VSCode 安装
   - 无需备份 Kilo 设置

3. **WSL 专用解决方案**
   - 如果使用 WSL，请确保通过 `code .` 命令从 WSL 内部启动 VSCode

4. **ZSH 用户**
   - 尝试在 Kilo Code 设置中启用部分或全部与 ZSH 相关的解决方案
   - 无论使用何种操作系统，这些设置都可能有所帮助

## 已知问题与解决方法

### Ctrl+C 行为问题

**问题描述**：当 Kilo Code 尝试在终端中执行命令时，如果终端中已有输入的文本，Kilo Code 会先按下 Ctrl+C 清除当前行，这可能会中断正在运行的进程。

**解决方法**：在让 Kilo Code 执行终端命令之前，确保终端提示符为空（没有输入任何部分命令）。

### 多行命令问题

**问题描述**：跨越多行的命令可能会让 Kilo Code 产生混淆，导致当前命令的输出中混入之前命令的输出。

**解决方法**：避免使用多行命令，而是使用 `&&` 进行命令链，将所有操作放在同一行（例如，使用 `echo a && echo b` 代替分行输入每个命令）。

### PowerShell 特定问题

1. **过早完成**：PowerShell 有时会在命令输出全部显示之前就告诉 Kilo Code 命令已完成。
2. **重复命令拒绝执行**：PowerShell 可能会拒绝连续两次执行相同的命令。

**解决方法**：启用 "PowerShell counter workaround" 设置，并在设置中将终端命令延迟（Terminal Command Delay）设为 150ms，以给命令更多的执行时间。

### 终端输出不完整

**问题描述**：有时 VS Code 不会显示或捕获命令的全部输出。

**解决方法**：如果发现输出缺失，尝试关闭并重新打开终端标签，然后再次运行命令。这会刷新终端连接。

## 故障排除资源

### 查看调试日志
当发生 shell 集成问题时，检查调试日志：
1. 打开 "帮助" → "切换开发者工具" → "控制台"
2. 设置 "显示所有级别" 以查看所有日志消息
3. 查找包含 `[Terminal Process]` 的消息
4. 检查错误消息中的 `preOutput` 内容：
   - 空的 preOutput (`''`) 表示 VSCode 没有发送任何数据
   - 这可能表示存在 VSCode shell 集成问题，或无法控制的上游错误
   - 缺少 shell 集成标记可能需要调整设置，以解决可能的上游错误或与 shell 初始化和 VSCode 加载特殊 shell 集成钩子相关的本地工作站配置问题

### 使用 VSCode 终端集成测试扩展

[VSCode 终端集成测试扩展](https://github.com/KJ7LNW/vsce-test-terminal-integration) 可以通过测试不同的设置组合来帮助诊断 shell 集成问题：

1. **命令停滞时**：
   - 如果你看到 "command already running" 警告，点击 "Reset Stats" 重置终端状态
   - 这些警告表明 shell 集成未正常工作
   - 尝试不同的设置组合，直到找到有效的配置
   - 如果终端确实卡住了，通过关闭窗口并按 F5 重启扩展

2. **测试设置**：
   - 系统地尝试不同的组合：
     * 终端命令延迟（Terminal Command Delay）
     * Shell 集成设置
   - 记录哪些组合成功或失败
   - 这有助于识别 shell 集成问题的模式

3. **报告问题**：
   - 一旦发现有问题的配置
   - 记录确切的设置组合
   - 记录你的环境信息（操作系统、VSCode 版本、shell 类型以及任何 shell 提示符自定义设置）
   - 使用这些详细信息创建一个问题，以帮助改进 shell 集成

## 支持

如果你已按照上述步骤操作但仍然遇到问题，请：

1. 查看 [Kilo Code GitHub 问题](https://github.com/Kilo-Org/kilocode/issues)，看是否有其他用户报告过类似问题
2. 如果没有，请创建一个新问题，详细说明你的操作系统、VSCode/Cursor 版本以及你尝试过的步骤

如需更多帮助，请加入我们的 [Discord](https://kilocode.ai/discord)。