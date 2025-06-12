---
侧边栏标签: Ollama
---

# 在Kilo Code中使用Ollama  

Kilo Code 支持通过 Ollama 在本地运行模型。这提供了隐私保护、离线访问能力，并可能降低成本，但需要更多设置且依赖高性能计算机。  

**官方网站**：[https://ollama.com/](https://ollama.com/)  

## 安装配置Ollama  

1. **下载并安装Ollama**：从[Ollama官网](https://ollama.com/)下载对应操作系统的安装程序，按指引完成安装。确保Ollama已启动：

   ```bash  
   ollama serve  
   ```  

2. **下载模型**：Ollama 支持多种模型，可在[Ollama模型库](https://ollama.com/library)查看可用列表。以下是推荐用于编码任务的模型：  

   * `codellama:7b-code`（入门首选，模型较小）  
   * `codellama:13b-code`（质量更佳，模型较大）  
   * `codellama:34b-code`（质量最优，模型极大）  
   * `qwen2.5-coder:32b`  
   * `mistralai/Mistral-7B-Instruct-v0.1`（通用型优质模型）  
   * `deepseek-coder:6.7b-base`（适合编码任务）  
   * `llama3:8b-instruct-q5_1`（适合常规任务）  

   在终端运行以下命令下载模型：  

   ```bash  
   ollama pull <模型名称>  
   ```  

   示例：  

   ```bash  
   ollama pull qwen2.5-coder:32b  
   ```  

3. **配置模型**：默认情况下，Ollama 使用 2048 tokens的上下文窗口，这对 Kilo Code 的请求来说过小。建议至少设置为12k，理想情况下为32k。配置模型需设置参数并保存副本：  

   - 加载模型（以`qwen2.5-coder:32b`为例）：  

     ```bash  
     ollama run qwen2.5-coder:32b  
     ```  

   - 修改上下文大小参数：  

     ```bash  
     /set parameter num_ctx 32768  
     ```  

   - 使用新名称保存模型： 

     ```bash  
     /save 你的模型名称  
     ```  

4. **配置Kilo Code**：  
   * 打开Kilo Code侧边栏（<img src="/docs/img/kilo-v1.svg" width="12" />图标）。  
   * 点击设置齿轮图标（<Codicon name="gear" />）。  
   * 选择“ollama”作为API提供商。  
   * 输入上一步设置的模型名称（如`你的模型名称`）。  
   * （可选）若Ollama运行在其他设备上，可配置基础URL（默认：`http://localhost:11434`）。  
   * （可选）在高级设置中配置模型上下文大小，以便Kilo Code管理滑动窗口。  

## 注意事项  

* **资源要求**：本地运行大型语言模型需要较高资源配置，请确保计算机满足所选模型的最低要求。  
* **模型选择**：建议尝试不同模型，找到最符合需求的选项。  
* **离线使用**：模型下载完成后，Kilo Code 可离线使用该模型。  
* **Ollama文档**：更多关于安装、配置和使用Ollama的信息，请参考[Ollama官方文档](https://ollama.com/docs)。  
