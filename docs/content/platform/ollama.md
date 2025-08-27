# 使用 AutoGPT 运行 Ollama

> **Important**: Ollama 集成仅在自托管 AutoGPT 平台时可用。无法在云托管版本中使用。

按照以下步骤设置并使用 AutoGPT 平台运行 Ollama。

## 前置条件

1. 确保您已完成 [AutoGPT 设置](/platform/getting-started) 步骤，如果尚未完成，请先完成再继续本指南。
2. 开始前，请确保您的计算机上已 [安装 Ollama](https://ollama.com/download)。

## 设置步骤

### 1. 启动 Ollama

要为网络访问正确设置 Ollama，请按照以下步骤操作：

1. **设置主机环境变量：**

   **Windows (命令提示符)：**
   ```
   set OLLAMA_HOST=0.0.0.0:11434
   ```
   
   **Linux/macOS (终端)：**
   ```bash
   export OLLAMA_HOST=0.0.0.0:11434
   ```

2. 启动 Ollama 服务器：
   ```
   ollama serve
   ```

3. **打开一个新的终端/命令窗口** 并下载您所需的模型：
   ```
   ollama pull llama3.2
   ```

> **Note**: 这将下载 [llama3.2](https://ollama.com/library/llama3.2) 模型。请在整个会话期间保持运行 `ollama serve` 的终端在后台运行。

### 2. 启动后端

打开一个新的终端并导航到 autogpt_platform 目录：

```bash
cd autogpt_platform
docker compose up -d --build
```

### 3. 启动前端

打开一个新的终端并导航到前端目录：

```bash
cd autogpt_platform/frontend
corepack enable
pnpm i
pnpm dev
```

然后访问 [http://localhost:3000](http://localhost:3000) 查看运行中的前端界面，注册账户/登录后，导航至构建页面 [http://localhost:3000/build](http://localhost:3000/build)

### 4. 将 Ollama 与 AutoGPT 结合使用

现在 Ollama 和 AutoGPT 平台都已运行，我们可以开始将 Ollama 与 AutoGPT 结合使用：

1. 在工作区添加 AI 文本生成器模块（可与任何 AI LLM 模块配合使用，本例将使用 AI 文本生成器模块）：
   ![添加 AI 文本生成器模块](../imgs/ollama/Select-AI-block.png)

2. 在 "LLM 模型" 下拉菜单中，选择 "llama3.2"（这是我们之前下载的模型）
   ![选择 Ollama 模型](../imgs/ollama/Ollama-Select-Llama32.png)

   > **兼容模型**：并非所有模型都能在 AutoGPT 中与 Ollama 配合使用。以下是确认可用的模型：
   > - `llama3.2`
   > - `llama3`
   > - `llama3.1:405b`
   > - `dolphin-mistral:latest`

3. **在 "Ollama 主机" 字段中设置您的本地 IP 地址**：

   **查找本地 IP 地址的方法：**

   **Windows (命令提示符)：**
   ```
   ipconfig
   ```

   **Linux/macOS (终端)：**
   ```bash
   ip addr show
   ```
   或
   ```bash
   ipconfig
   ```

   找到您的 IPv4 地址（例如 `192.168.0.39`），然后在 "Ollama 主机" 字段中输入该地址并加上端口号 `11434`：
   ```
   192.168.0.39:11434
   ```

   ![Ollama 远程主机](../imgs/ollama/Ollama-Remote-Host.png)

4. 现在我们需要添加一些提示词，然后保存并运行图谱：
   ![添加提示词](../imgs/ollama/Ollama-Add-Prompts.png)

完成！您已成功设置 AutoGPT 平台并向 Ollama 发起了 LLM 调用。
![Ollama 输出](../imgs/ollama/Ollama-Output.png)

### 在远程服务器上使用 Ollama 与 AutoGPT

要在远程服务器上运行 Ollama，只需确保 Ollama 服务器正在运行，并且可以通过网络端口 11434 从网络上的其他设备或远程访问。

**要查找运行 Ollama 系统的本地 IP 地址：**

**Windows (命令提示符)：**

```
ipconfig
```

**Linux/macOS (终端)：**

```bash
ip addr show
```

或

```bash
ipconfig
```

查找您的 IPv4 地址（例如 `192.168.0.39`）。

然后您可以按照上述相同步骤操作，但需要在区块设置中将 Ollama 服务器的 IP 地址添加到 "Ollama Host" 字段中，如下所示：

```
192.168.0.39:11434
```

![Ollama 远程主机](../imgs/ollama/Ollama-Remote-Host.png)

## 故障排除

如果遇到任何问题，请验证：

- Ollama 已正确安装并正在运行
- 所有终端在操作期间保持打开状态
- 在启动后端之前 Docker 正在运行

常见错误处理：

1. **连接被拒绝**：确保 Ollama 正在运行且主机地址正确（同时确保端口正确，默认端口为 11434）
2. **模型未找到**：请先尝试手动运行 `ollama pull llama3.2`
3. **Docker 问题**：使用 `docker ps` 确保 Docker 守护进程正在运行