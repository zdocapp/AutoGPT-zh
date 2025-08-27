# 内置组件

本页面列出了所有原生提供的 [🧩 组件](./components.md) 及其实现的 [⚙️ 协议](./protocols.md)。这些组件由 AutoGPT 代理使用。
部分组件在表格中列出了额外的配置选项，详见[组件配置](./components.md/#component-configuration)了解更多信息。

!!! note
    如果配置字段使用环境变量，仍可通过配置模型传递。### 配置中的值优先于环境变量！仅当配置中未设置值时才会应用环境变量。

## `SystemComponent`

允许代理完成任务的基础组件。

### DirectiveProvider

- API 预算约束

### MessageProvider

- 当前时间和日期
- 剩余 API 预算及预算不足时的警告

### CommandProvider

- 任务完成时使用的 `finish` 命令

## `UserInteractionComponent`

增加在 CLI 中与用户交互的能力。

### CommandProvider

- 用于向用户请求输入的 `ask_user` 命令

## `FileManagerComponent`

增加向本地存储、Google Cloud Storage 或 Amazon S3 读写持久化文件的能力。
用于保存和加载代理状态（保持会话）的必要组件。

### `FileManagerConfiguration`

| Config variable  | 详情                                 | 类型  | 默认值                             |
| ---------------- | ------------------------------------ | ----- | ---------------------------------- |
| `storage_path`   | 代理文件路径，例如状态文件             | `str` | `agents/{agent_id}/`[^1]           |
| `workspace_path` | 代理可访问文件的路径                   | `str` | `agents/{agent_id}/workspace/`[^1] |

[^1] 此选项在组件构建期间动态设置，而非在配置模型内部默认设置，`{agent_id}` 会被替换为代理的唯一标识符。

### DirectiveProvider

- 可读取和写入文件的资源信息

### CommandProvider

- `read_file` 用于读取文件
- `write_file` 用于写入文件
- `list_folder` 列出文件夹中的所有文件

## `CodeExecutorComponent`

允许代理执行非交互式 Shell 命令和 Python 代码。Python 执行仅在 Docker 可用时工作。

### `CodeExecutorConfiguration`

| 配置变量                 | 详情                                               | 类型                        | 默认值            |
| ------------------------ | -------------------------------------------------- | --------------------------- | ----------------- |
| `execute_local_commands` | 启用 shell 命令执行                                | `bool`                      | `False`           |
| `shell_command_control`  | 控制使用哪个列表                                   | `"allowlist" \| "denylist"` | `"allowlist"`     |
| `shell_allowlist`        | 允许的 shell 命令列表                              | `List[str]`                 | `[]`              |
| `shell_denylist`         | 禁止的 shell 命令列表                              | `List[str]`                 | `[]`              |
| `docker_container_name`  | 用于代码执行的 Docker 容器名称                     | `str`                       | `"agent_sandbox"` |

所有 shell 命令配置仅用于便利性考虑。该组件并不安全，不应在生产环境中使用。建议使用更合适的沙箱方案。

### CommandProvider

- `execute_shell` 执行 shell 命令
- `execute_shell_popen` 使用 popen 执行 shell 命令
- `execute_python_code` 执行 Python 代码
- `execute_python_file` 执行 Python 文件

## `ActionHistoryComponent`

跟踪代理的操作及其结果，并向提示提供其摘要。

### `ActionHistoryConfiguration`

| 配置变量               | 详情                                                 | 类型        | 默认值             |
| ---------------------- | ---------------------------------------------------- | ----------- | ------------------ |
| `llm_name`             | 用于压缩历史记录的LLM模型名称                        | `ModelName` | `"gpt-3.5-turbo"`  |
| `max_tokens`           | 历史摘要使用的最大令牌数                             | `int`       | `1024`             |
| `spacy_language_model` | 使用spacy进行摘要分块的语言模型                      | `str`       | `"en_core_web_sm"` |
| `full_message_count`   | 提示中包含未摘要的循环次数                           | `int`       | `4`                |

### MessageProvider

- 代理进度摘要

### AfterParse

- 注册代理操作

### ExecutionFailure

- 回滚代理操作，使其不被保存

### AfterExecute

- 将代理操作结果保存到历史记录中

## `GitOperationsComponent`

添加与git仓库和GitHub交互的能力。

### `GitOperationsConfiguration`

| Config variable   | Details                                   | Type  | Default |
| ----------------- | ----------------------------------------- | ----- | ------- |
| `github_username` | GitHub 用户名, *ENV:* `GITHUB_USERNAME` | `str` | `None`  |
| `github_api_key`  | GitHub API 密钥, *ENV:* `GITHUB_API_KEY`   | `str` | `None`  |

### CommandProvider

- `clone_repository` 用于克隆 git 仓库

## `ImageGeneratorComponent`

添加使用各种提供商生成图像的能力。

### Hugging Face

要使用 Hugging Face 的文本到图像模型，您需要一个 Hugging Face API 令牌。
相应设置页面的链接：[Hugging Face > Settings > Tokens](https://huggingface.co/settings/tokens)

### Stable Diffusion WebUI

可以使用您自己托管的 Stable Diffusion WebUI 与 AutoGPT 配合使用。### 确保您运行 WebUI 时启用了 `--api` 参数。

### `ImageGeneratorConfiguration`

| 配置变量                  | 详情                                                          | 类型                                    | 默认值                            |
| ------------------------- | ------------------------------------------------------------- | --------------------------------------- | --------------------------------- |
| `image_provider`          | 图像生成提供商                                                | `"dalle" \| "huggingface" \| "sdwebui"` | `"dalle"`                         |
| `huggingface_image_model` | Hugging Face 图像模型，参见[可用模型]                         | `str`                                   | `"CompVis/stable-diffusion-v1-4"` |
| `huggingface_api_token`   | Hugging Face API 令牌，*环境变量:* `HUGGINGFACE_API_TOKEN`    | `str`                                   | `None`                            |
| `sd_webui_url`            | 自托管 Stable Diffusion WebUI 的 URL                          | `str`                                   | `"http://localhost:7860"`         |
| `sd_webui_auth`           | Stable Diffusion WebUI 的基础认证，*环境变量:* `SD_WEBUI_AUTH` | `str` 格式为 `{username}:{password}`    | `None`                            |

[available models]: https://huggingface.co/models?pipeline_tag=text-to-image

### CommandProvider

- `generate_image` 用于根据提示生成图像

## `WebSearchComponent`

允许代理搜索网络。DuckDuckGo 无需 Google 凭据。[如何设置 Google API 密钥的说明](../../classic/configuration/search.md)

### `WebSearchConfiguration`

| 配置变量                  | 详情                                                                 | 类型                        | 默认值 |
| -------------------------------- | ----------------------------------------------------------------------- | --------------------------- | ------- |
| `google_api_key`                 | Google API 密钥, *环境变量:* `GOOGLE_API_KEY`                                 | `str`                       | `None`  |
| `google_custom_search_engine_id` | Google 自定义搜索引擎 ID, *环境变量:* `GOOGLE_CUSTOM_SEARCH_ENGINE_ID` | `str`                       | `None`  |
| `duckduckgo_max_attempts`        | 使用 DuckDuckGo 搜索的最大尝试次数                   | `int`                       | `3`     |
| `duckduckgo_backend`             | 用于 DDG SDK 的后端                                          | `"api" \| "html" \| "lite"` | `"api"` |

### DirectiveProvider

- 资源信息表明可以搜索网络

### CommandProvider

- `search_web` 用于通过 DuckDuckGo 搜索网络
- `google` 用于通过 Google 搜索网络，需要 API 密钥

## `WebSeleniumComponent`

允许代理使用 Selenium 读取网站。

### `WebSeleniumConfiguration`

| 配置变量                      | 详情                                     | 类型                                          | 默认值                                                                                                                       |
| ----------------------------- | ---------------------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `llm_name`                    | 用于读取网站的LLM模型名称                | `ModelName`                                   | `"gpt-3.5-turbo"`                                                                                                            |
| `web_browser`                 | Selenium使用的Web浏览器                  | `"chrome" \| "firefox" \| "safari" \| "edge"` | `"chrome"`                                                                                                                   |
| `headless`                    | 以无头模式运行浏览器                     | `bool`                                        | `True`                                                                                                                       |
| `user_agent`                  | 浏览器使用的用户代理                     | `str`                                         | `"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36"` |
| `browse_spacy_language_model` | 用于文本分块的Spacy语言模型              | `str`                                         | `"en_core_web_sm"`                                                                                                           |
| `selenium_proxy`              | 与Selenium一起使用的HTTP代理             | `str`                                         | `None`                                                                                                                       |

### DirectiveProvider

- 可读取网站的资源信息

### CommandProvider

- `read_website` 用于读取特定网址并查找特定主题或回答问题

## `ContextComponent`

添加在提示中保持最新文件和文件夹内容的能力。

### MessageProvider

- 上下文中的元素内容

### CommandProvider

- `open_file` 用于将文件打开到上下文中
- `open_folder` 用于将文件夹打开到上下文中
- `close_context_item` 从上下文中移除项目

## `WatchdogComponent`

监视代理是否循环，必要时切换到智能模式。

### AfterParse

- 调查发生的情况，必要时切换到智能模式