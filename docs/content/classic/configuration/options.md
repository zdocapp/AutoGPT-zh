# 配置

敏感设置（如 API 凭据）的配置通过环境变量完成。
您可以通过 `.env` 文件设置配置变量。如果您没有 `.env` 文件，请在 `AutoGPT` 文件夹中创建 `.env.template` 的副本，并将其命名为 `.env`。

## 环境变量

- `AUTHORISE_COMMAND_KEY`: 授权命令时接受的关键响应。默认值：y
- `ANTHROPIC_API_KEY`: 如需在 AutoGPT 中使用 Anthropic 模型，请设置此参数
- `AZURE_CONFIG_FILE`: Azure 配置文件相对于 AutoGPT 根目录的位置。默认值：azure.yaml
- `COMPONENT_CONFIG_FILE`: 智能体组件配置文件的路径（json 格式）。可选
- `DISABLED_COMMANDS`: 要禁用的命令。使用逗号分隔的命令名称。内置组件命令列表请参见[此处](../../forge/components/components.md)。默认值：None
- `ELEVENLABS_API_KEY`: ElevenLabs API 密钥。可选。
- `ELEVENLABS_VOICE_ID`: ElevenLabs 语音 ID。可选。
- `EMBEDDING_MODEL`: 用于嵌入任务的 LLM 模型。默认值：`text-embedding-3-small`
- `EXIT_KEY`: 退出时接受的退出键。默认值：n
- `FAST_LLM`: 用于大多数任务的 LLM 模型。默认值：`gpt-3.5-turbo-0125`
- `GITHUB_API_KEY`: [GitHub API 密钥](https://github.com/settings/tokens)。可选。
- `GITHUB_USERNAME`: GitHub 用户名。可选。
- `GOOGLE_API_KEY`: Google API 密钥。可选。
- `GOOGLE_CUSTOM_SEARCH_ENGINE_ID`: [Google 自定义搜索引擎 ID](https://programmablesearchengine.google.com/controlpanel/all)。可选。
- `GROQ_API_KEY`: 如需在 AutoGPT 中使用 Groq 模型，请设置此参数
- `HUGGINGFACE_API_TOKEN`: HuggingFace API 令牌，用于图像生成和语音转文本。可选。
- `HUGGINGFACE_IMAGE_MODEL`: 用于图像生成的 HuggingFace 模型。默认值：CompVis/stable-diffusion-v1-4
- `LLAMAFILE_API_BASE`: Llamafile API 基础 URL。默认值：`http://localhost:8080/v1`
- `OPENAI_API_KEY`: 如需使用 OpenAI 模型，请设置此参数；[OpenAI API 密钥](https://platform.openai.com/account/api-keys)。
- `OPENAI_ORGANIZATION`: OpenAI 中的组织 ID。可选。
- `PLAIN_OUTPUT`: 纯文本输出，禁用旋转器动画。默认值：False
- `RESTRICT_TO_WORKSPACE`: 将文件读写限制在工作空间目录内。默认值：True
- `SD_WEBUI_AUTH`: Stable Diffusion Web UI 用户名:密码对。可选。
- `SMART_LLM`: 用于"智能"任务的 LLM 模型。默认值：`gpt-4-turbo-preview`
- `STREAMELEMENTS_VOICE`: 使用的 StreamElements 语音。默认值：Brian
- `TEMPERATURE`: 提供给 OpenAI 的温度值。取值范围 0 到 2。数值越低确定性越强，数值越高随机性越大。参见 https://platform.openai.com/docs/api-reference/completions/create#completions/create-temperature
- `TEXT_TO_SPEECH_PROVIDER`: 文本转语音提供商。可选值：`gtts`、`macos`、`elevenlabs` 和 `streamelements`。默认值：gtts
- `USE_AZURE`: 使用 Azure 的 LLM 默认值：False