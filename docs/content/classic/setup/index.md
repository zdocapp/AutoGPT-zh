# AutoGPT 代理设置

[🐋 **使用 Docker 设置和运行**](./docker.md)
&ensp;|&ensp;
[👷🏼 **开发者指南**](./for-developers.md)

## 📋 系统要求

### Linux / macOS

- Python 3.10 或更高版本
- Poetry ([安装说明](https://python-poetry.org/docs/#installation))

### Windows (WSL)

- WSL 2
  - 另请参阅 [Linux 和 macOS 的要求](#linux-macos)
- [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)

### Windows 平台

!!! attention
    我们推荐使用 WSL 设置 AutoGPT。某些功能在 Windows 上的运行方式不完全相同，
    目前我们无法为所有特殊情况提供专门的说明。

- Python 3.10 或更高版本 ([安装说明](https://www.tutorialspoint.com/how-to-install-python-in-windows))
- Poetry ([安装说明](https://python-poetry.org/docs/#installation))
- [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)

## 设置 AutoGPT

### 获取 AutoGPT

由于我们不将 AutoGPT 作为桌面应用程序分发，您需要从 GitHub 下载
[项目]并在计算机上为其指定位置。

![克隆或下载仓库的对话框截图](get-repo-dialog.png)

- 要获取最新的前沿版本，请使用 `master` 分支
- 如果您需要更稳定的版本，请查看最新的 AutoGPT [发布版本][releases]

[project]: https://github.com/Significant-Gravitas/AutoGPT

[releases]: https://github.com/Significant-Gravitas/AutoGPT/releases

!!! note
    如果您希望以 Docker 镜像方式运行 AutoGPT，这些说明不适用。
    请查阅 [Docker 设置](./docker.md) 指南。

### 完成设置

克隆或下载项目后，您可以在 `original_autogpt/` 文件夹中找到 AutoGPT Agent。
在此文件夹内，您可以通过 `.env` 文件和（可选）JSON 配置文件来配置 AutoGPT 应用程序：

- `.env` 用于环境变量，主要用于敏感数据（如 API 密钥）
- JSON 配置文件用于自定义 AutoGPT [组件](../../forge/components/introduction.md) 的某些功能

有关可用环境变量的列表，请参阅 [配置](../configuration/options.md) 参考文档。

1. 找到名为 `.env.template` 的文件。该文件可能
    在某些操作系统中因点前缀而默认隐藏。要显示
    隐藏文件，请按照适用于您特定操作系统的说明操作：
    [Windows][显示隐藏文件/Windows] 和 [macOS][显示隐藏文件/macOS]。
2. 创建 `.env.template` 的副本并命名为 `.env`；
    如果您已在命令提示符/终端窗口中：
    ```shell
    cp .env.template .env
    ```
3. 在文本编辑器中打开 `.env` 文件。
4. 为您想要使用的 LLM 提供商设置 API 密钥：请参阅[下文](#setting-up-llm-providers)。
5. 输入您想要使用的任何其他服务的 API 密钥或令牌。

    !!! note
        要激活和调整设置，请移除 `# ` 前缀。

6. 保存并关闭 `.env` 文件。
7. _可选：运行 `poetry install` 以安装所有必需的依赖项。_ 该
    应用程序在启动时也会检查并安装任何必需的依赖项。
8. _可选：使用您所需的设置配置 JSON 文件（例如 `config.json`）。_
    如果您不提供 JSON 配置文件，应用程序将使用默认设置。
    了解如何[设置 JSON 配置文件](../../forge/components/components.md#json-configuration)

您现在应该能够探索 CLI (`./autogpt.sh --help`) 并运行应用程序了。

有关进一步说明，请参阅[用户指南](../usage.md)。

[show hidden files/Windows]: https://support.microsoft.com/en-us/windows/view-hidden-files-and-folders-in-windows-97fbc472-c603-9d90-91d0-1166d1d9f4b5

[show hidden files/macOS]: https://www.pcmag.com/how-to/how-to-access-your-macs-hidden-files

## 设置 LLM 提供商

您可以使用以下任意 LLM 提供商运行 AutoGPT。
每个提供商都有其特定的设置说明。

AutoGPT 最初基于 OpenAI 的 GPT-4 构建，但现在您也可以使用其他模型/提供商获得类似且有趣的结果。
如果您不确定选择哪个，可以放心选择 OpenAI*。

<small>* 可能发生变化</small>

### OpenAI

!!! attention
    要使用 AutoGPT 搭配 GPT-4（推荐），您需要设置一个已充值付费的 OpenAI 账户。
    请参考 OpenAI 获取进一步说明（[链接][openai/help-gpt-4-access]）。
    免费账户[受限][openai/api-limits]于 GPT-3.5，每分钟仅允许 3 次请求。

1. 确保您已设置具有可用额度的付费账户：[Settings > Organization > Billing][openai/billing]
1. 从以下位置获取您的 OpenAI API 密钥：[API keys][openai/api-keys]
2. 打开 `.env` 文件
3. 找到显示 `OPENAI_API_KEY=` 的行
4. 在等号后直接插入您的 OpenAI API 密钥，无需引号或空格：
    ```ini
    OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

    !!! info "使用 GPT Azure 实例"
        如果您想在 Azure 实例上使用 GPT，请将 `USE_AZURE` 设置为 `True` 并
        创建 Azure 配置文件。

        将 `azure.yaml.template` 重命名为 `azure.yaml` 并提供相关的
        `azure_api_base`、`azure_api_version` 以及您想要使用的模型的部署 ID。

        例如，如果您想使用 `gpt-3.5-turbo` 和 `gpt-4-turbo`：

        ```yaml
        # 请将所有值指定为双引号字符串
        # 将尖括号 (<>) 中的字符串替换为您自己的部署名称
        azure_model_map:
            gpt-3.5-turbo: "<gpt-35-turbo-deployment-id>"
            gpt-4-turbo: "<gpt-4-turbo-deployment-id>"
            ...
        ```

        详细信息可在 [openai/python-sdk/azure] 中找到，以及 [Azure OpenAI 文档] 中的嵌入模型部分。
        如果您使用的是 Windows 系统，可能需要安装 [MSVC 库](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)。

!!! important
    请在[使用情况页面][openai/usage]上密切关注您的API成本。

[openai/api-keys]: https://platform.openai.com/account/api-keys

[openai/billing]: https://platform.openai.com/account/billing/overview

[openai/usage]: https://platform.openai.com/account/usage

[openai/api-limits]: https://platform.openai.com/docs/guides/rate-limits/free-tier-rate-limits

[openai/help-gpt-4-access]: https://help.openai.com/en/articles/7102672-how-can-i-access-gpt-4-gpt-4-turbo-and-gpt-4o#h_9bddcd317c

[openai/python-sdk/azure]: https://github.com/openai/openai-python?tab=readme-ov-file#microsoft-azure-openai

### Anthropic

1. 确保您的账户中有余额：[设置 > 套餐与账单][anthropic/billing]
2. 从[设置 > API密钥][anthropic/api-keys]获取您的Anthropic API密钥
3. 打开`.env`文件
4. 找到显示`ANTHROPIC_API_KEY=`的行
5. 在等号后直接插入您的Anthropic API密钥，无需引号或空格：
    ```ini
    ANTHROPIC_API_KEY=sk-ant-api03-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```
6. 将`SMART_LLM`和/或`FAST_LLM`设置为您想要使用的Claude 3模型。
   有关可用模型的信息，请参阅Anthropic的[模型概览][anthropic/models]。
   示例：
    ```ini
    SMART_LLM=claude-3-opus-20240229
    ```

!!! important
    请在[使用情况页面][anthropic/usage]上密切关注您的API成本。

[anthropic/api-keys]: https://console.anthropic.com/settings/keys

[anthropic/billing]: https://console.anthropic.com/settings/plans

[anthropic/usage]: https://console.anthropic.com/settings/usage

[anthropic/models]: https://docs.anthropic.com/en/docs/models-overview

### Groq

!!! note
    虽然支持Groq，但其内置的函数调用API尚不成熟。
    使用此API的任何功能可能会遇到性能下降的情况。
    请告诉我们您的使用体验！

1. 从 [Settings > API keys][groq/api-keys] 获取您的 Groq API 密钥
2. 打开 `.env` 文件
3. 找到显示 `GROQ_API_KEY=` 的行
4. 在等号后直接插入您的 Groq API 密钥，无需引号或空格：
    ```ini
    GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```
5. 将 `SMART_LLM` 和/或 `FAST_LLM` 设置为您想要使用的 Groq 模型。
   有关可用模型的信息，请参阅 Groq 的 [models overview][groq/models]。
   示例：
    ```ini
    SMART_LLM=llama3-70b-8192
    ```

[groq/api-keys]: https://console.groq.com/keys

[groq/models]: https://console.groq.com/docs/models

### Llamafile

使用 llamafile 可以在本地运行模型，这意味着无需设置账单，
并保证数据隐私。

有关更多信息和详细文档，请查看 [llamafile documentation]。

!!! warning
    目前，llamafile 一次只能服务一个模型。这意味着您不能
    将 `SMART_LLM` 和 `FAST_LLM` 设置为两个不同的 llamafile 模型。

!!! warning
    由于以下链接的问题，llamafiles 在 WSL 中无法工作。要在 WSL 中使用 llamafile
    与 AutoGPT，您必须在 Windows（WSL 外部）中运行 llamafile。

    <details>
    <summary>Instructions</summary>

    1. Get the `llamafile/serve.py` script through one of these two ways:
        1. Clone the AutoGPT repo somewhere in your Windows environment,
           with the script located at `classic/original_autogpt/scripts/llamafile/serve.py`
        2. Download just the [serve.py] script somewhere in your Windows environment
    2. Make sure you have `click` installed: `pip install click`
    3. Run `ip route | grep default | awk '{print $3}'` *inside WSL* to get the address
       of the WSL host machine
    4. Run `python3 serve.py --host {WSL_HOST_ADDR}`, where `{WSL_HOST_ADDR}`
       is the address you found at step 3.
       If port 8080 is taken, also specify a different port using `--port {PORT}`.
    5. In WSL, set `LLAMAFILE_API_BASE=http://{WSL_HOST_ADDR}:8080/v1` in your `.env`.
    6. Follow the rest of the regular instructions below.

    [serve.py]: https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/original_autogpt/scripts/llamafile/serve.py
    </details>

    * [Mozilla-Ocho/llamafile#356](https://github.com/Mozilla-Ocho/llamafile/issues/356)
    * [Mozilla-Ocho/llamafile#100](https://github.com/Mozilla-Ocho/llamafile/issues/100)

!!! note
    这些指令将下载并使用 `mistral-7b-instruct-v0.2.Q5_K_M.llamafile`。
    `mistral-7b-instruct-v0.2` 是目前唯一经过测试和受支持的模型。
    如果您想尝试其他模型，必须将其添加到 [`llamafile.py`][classic/forge/llamafile.py] 中的 `LlamafileModelName`。
    为获得最佳效果，您可能还需要添加一些逻辑来适配消息格式，
    就像 `LlamafileProvider._adapt_chat_messages_for_mistral_instruct(..)` 所做的那样。

1. 运行 llamafile 服务脚本：
   ```shell
   python3 ./scripts/llamafile/serve.py
   ```
   首次运行时，将下载包含模型和运行时的文件，这可能需要一些时间并占用几GB的磁盘空间。

   要强制使用 GPU 加速，请在命令中添加 `--use-gpu`。

3. 在 `.env` 文件中，将 `SMART_LLM`/`FAST_LLM` 或两者都设置为 `mistral-7b-instruct-v0.2`

4. 如果服务器运行在 `http://localhost:8080/v1` 以外的地址，
   请在 `.env` 中将 `LLAMAFILE_API_BASE` 设置为正确的基础 URL

[llamafile documentation]: https://github.com/Mozilla-Ocho/llamafile#readme

[classic/forge/llamafile.py]: https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/forge/llm/providers/llamafile/llamafile.py