# 🧠 使用 AutoGPT 运行 AI/ML API

按照以下步骤将 **AI/ML API** 与 **AutoGPT** 平台连接，实现高性能 AI 文本生成。

---

## ✅ 前提条件

1. 确保您已阅读并完成 [AutoGPT 设置指南](https://docs.agpt.co/platform/getting-started/)，且 AutoGPT 已在本地运行于 `http://localhost:3000`。
2. 您已从 [AI/ML API](https://aimlapi.com/app/keys?utm_source=autogpt&utm_medium=github&utm_campaign=integration) 获取 **API 密钥**。

---

## ⚙️ 设置步骤

### 1. 本地启动 AutoGPT

遵循官方指南：
[📖 AutoGPT 入门指南](https://docs.agpt.co/platform/getting-started/)

确保 AutoGPT 正在运行并可通过以下地址访问：
[http://localhost:3000](http://localhost:3000)

> 💡 在整个会话期间，请保持 AutoGPT 在终端或 Docker 中运行。

![步骤 1 AutoGPT 运行中](../imgs/aimlapi/Step%201%20AutoGPT%20Running.png)

---

### 2. 打开可视化构建器

打开浏览器并访问：
[http://localhost:3000/build](http://localhost:3000/build)

或在导航栏中点击 **"构建"**。

![步骤 2 构建界面](../imgs/aimlapi/Step%202%20Build%20Screen.png)

---

### 3. 添加 AI 文本生成器模块

1. 点击左侧边栏的 **"模块"** 按钮。

![步骤 3 AI 模块](../imgs/aimlapi/Step%203%20AI%20Block.png)

2. 在搜索栏中输入 `AI Text Generator`。
3. 将模块拖入画布。

![步骤 4 AI 生成器模块](../imgs/aimlapi/Step%204%20AI%20Generator%20Block.png)

---

### 4. 选择 AI/ML API 模型

点击 AI Text Generator 模块进行配置。

在 **LLM 模型** 下拉菜单中，从 AI/ML API 选择一个支持的模型：

![步骤 5 AIMLAPI 模型](../imgs/aimlapi/Step%205%20AIMLAPI%20Models.png)

| 模型 ID                                       | 速度   | 推理质量        | 最佳用途                 |
| ---------------------------------------------- | ------ | ----------------- | ------------------------ |
| `Qwen/Qwen2.5-72B-Instruct-Turbo`              | 中等   | 高                | 基于文本的任务         |
| `nvidia/llama-3.1-nemotron-70b-instruct`       | 中等   | 高                | 分析与推理              |
| `meta-llama/Llama-3.3-70B-Instruct-Turbo`      | 低     | 非常高            | 复杂多步骤任务          |
| `meta-llama/Meta-Llama-3.1-70B-Instruct-Turbo` | 低     | 非常高            | 深度推理                |
| `meta-llama/Llama-3.2-3B-Instruct-Turbo`       | 高     | 中等              | 快速响应                |

> ✅ 这些模型可通过 [AI/ML API](https://aimlapi.com/app/?utm_source=autogpt&utm_medium=github&utm_campaign=integration) 的 OpenAI 兼容 API 获得

---

### 5. 配置提示词和 API 密钥

在 **AI Text Generator** 模块内：

1. 在 **Prompt** 字段中输入您的提示文本。
2. 在指定字段中输入您的 **AI/ML API Key**。

🔐 您可以从以下地址获取密钥：
[https://aimlapi.com/app/keys/](https://aimlapi.com/app/keys?utm_source=autogpt&utm_medium=github&utm_campaign=integration)

![密钥占位符](../imgs/aimlapi/Step%206.1%20Key%20Placeholder.png)

![密钥为空](../imgs/aimlapi/Step%206.2%20No%20Fill%20Key%20Placeholder.png)

![密钥已填写](../imgs/aimlapi/Step%206.3%20Filled%20Key%20Placeholder.png)

![概览](../imgs/aimlapi/Step%206.4%20Overview.png)

---

### 6. 保存您的智能体

点击构建器界面右上角的 **"Save"** 按钮：

1. 为您的智能体命名（例如 `aimlapi_test_agent`）。
2. 点击 **"Save Agent"** 确认保存。

![保存智能体](../imgs/aimlapi/Step%207.1%20Save.png)

> 💡 保存后可在更大工作流中重复使用、调度和链接。

---

### 7. 运行您的智能体

在工作区中：

1. 点击已保存智能体旁边的 **"Run"**。
2. 请求将发送至选定的 AI/ML API 模型。

![运行智能体](../imgs/aimlapi/Step%208%20Run.png)

---

### 8. 查看输出结果

1. 滚动至 **AI Text Generator** 模块。
2. 查看下方的 **Output** 面板。
3. 可复制、导出结果或传递至后续模块。

![智能体输出](../imgs/aimlapi/Step%209%20Output.png)

---

## 🔄 扩展您的智能体

AI/ML API 连接成功后，通过链式调用其他功能块来扩展您的工作流：

* 🔧 **工具** – 获取URL、调用API、抓取数据
* 🧠 **记忆** – 在交互间保持上下文
* ⚙️ **动作/链** – 创建完整流水线

---

🎉 您现在正使用 **AutoGPT** 中的企业级 **AI/ML API** 模型生成AI响应！