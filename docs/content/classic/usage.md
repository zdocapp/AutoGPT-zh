# AutoGPT Agent 用户指南

!!! note
    本指南假设您位于 `autogpt` 文件夹中，即 AutoGPT Agent 所在的位置。

## 命令行界面

运行 `./autogpt.sh`（或其任何子命令）并加上 `--help` 参数，将列出所有可用的子命令和参数：

```shell
$ ./autogpt.sh --help
Usage: python -m autogpt [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  run    Sets up and runs an agent, based on the task specified by the...
  serve  Starts an Agent Protocol compliant AutoGPT server, which creates...
```

!!! important "Windows 用户请注意"
    在 Windows 上，请使用 `.\autogpt.bat` 代替 `./autogpt.sh`。
    其他所有内容（子命令、参数）的工作方式应该相同。

!!! info "Docker 使用说明"
    如需与 Docker 一起使用，请将示例中的脚本替换为
    `docker compose run --rm auto-gpt`：

    ```shell
    docker compose run --rm auto-gpt --ai-settings <filename>
    docker compose run --rm auto-gpt serve
    ```

### `run` &ndash; CLI 模式

`run` 子命令使用传统的 CLI 界面启动 AutoGPT。

<details>
<summary>
<code>./autogpt.sh run --help</code>
</summary>

```shell
$ ./autogpt.sh run --help
Usage: python -m autogpt run [OPTIONS]

  Sets up and runs an agent, based on the task specified by the user, or
  resumes an existing agent.

Options:
  -c, --continuous                Enable Continuous Mode
  -y, --skip-reprompt             Skips the re-prompting messages at the
                                  beginning of the script
  -l, --continuous-limit INTEGER  Defines the number of times to run in
                                  continuous mode
  --speak                         Enable Speak Mode
  --debug                         Enable Debug Mode
  --gpt3only                      Enable GPT3.5 Only Mode
  --gpt4only                      Enable GPT4 Only Mode
  --skip-news                     Specifies whether to suppress the output of
                                  latest news on startup.
  --install-plugin-deps           Installs external dependencies for 3rd party
                                  plugins.
  --ai-name TEXT                  AI name override
  --ai-role TEXT                  AI role override
  --constraint TEXT               Add or override AI constraints to include in
                                  the prompt; may be used multiple times to
                                  pass multiple constraints
  --resource TEXT                 Add or override AI resources to include in
                                  the prompt; may be used multiple times to
                                  pass multiple resources
  --best-practice TEXT            Add or override AI best practices to include
                                  in the prompt; may be used multiple times to
                                  pass multiple best practices
  --override-directives           If specified, --constraint, --resource and
                                  --best-practice will override the AI's
                                  directives instead of being appended to them
  --component-config-file TEXT    Path to the json configuration file.
  --help                          Show this message and exit.
```

</details>

此模式允许运行单个代理，并在终止时保存代理的状态。
这意味着您可以在以后*恢复*代理。另请参阅 [agent state]。

!!! note
    由于历史遗留原因，当未指定子命令时，CLI 将默认使用 `run` 子命令：
    运行 `./autogpt.sh run [OPTIONS]` 与 `./autogpt.sh [OPTIONS]` 效果相同，
    但这在未来可能会改变。

#### 💀 连续模式 ⚠️

在**无需**用户授权的情况下运行AI，完全自动化。
不建议使用连续模式。
这可能存在危险，可能导致您的AI无限期运行或执行您通常不会授权的操作。
请自行承担风险。

```shell
./autogpt.sh --continuous
```

要退出程序，请按 ++ctrl+c++

### `serve` &ndash; 带UI的Agent Protocol模式

使用 `serve` 时，应用程序会暴露一个符合Agent Protocol的API并提供前端界面，默认在 `http://localhost:8000` 上运行。您可以通过 `AP_SERVER_PORT` 环境变量配置服务端口。

<details>
<summary>
<code>./autogpt.sh serve --help</code>
</summary>

```shell
$ ./autogpt.sh serve --help
Usage: python -m autogpt serve [OPTIONS]

  Starts an Agent Protocol compliant AutoGPT server, which creates a custom
  agent for every task.

Options:
  --debug                     Enable Debug Mode
  --gpt3only                  Enable GPT3.5 Only Mode
  --gpt4only                  Enable GPT4 Only Mode
  --install-plugin-deps       Installs external dependencies for 3rd party
                              plugins.
  --help                      Show this message and exit.
```

</details>

有关应用程序API的更多信息，请参见 [agentprotocol.ai](https://agentprotocol.ai)。

<!-- TODO: add guide/manual for frontend -->

### 参数说明

!!! attention
    大多数参数等同于配置选项。有关所有可用配置选项，请参见 [`.env.template`][.env.template]。

!!! note
    将尖括号(<>)中的任何内容替换为您要指定的值

以下是运行AutoGPT时可以使用的一些常见参数：

* 使用不同的AI设置文件运行AutoGPT

    ```shell
    ./autogpt.sh --ai-settings <filename>
    ```

* 使用不同的提示设置文件运行AutoGPT

    ```shell
    ./autogpt.sh --prompt-settings <filename>
    ```

!!! note
    其中一些标志有简写形式，例如 `-P` 对应 `--prompt-settings`。  
    使用 `./autogpt.sh --help` 获取更多信息。

[.env.template]: https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/.env.template

## 代理状态

[agent state]: #agent-state

单个代理的状态存储在 `data/agents` 文件夹中。您可以通过多种方式使用此功能：

* 稍后恢复您的代理。
* 为您的代理创建"检查点"，以便随时回退到其历史记录中的特定时间点。
* 分享您的代理！

## 工作区

[workspace]: #workspace

代理可以读写文件。此操作发生在 `workspace` 文件夹中，该文件夹位于 `data/agents/<agent_id>/`。除非将 `RESTRICT_TO_WORKSPACE` 设置为 `False`，否则代理无法访问此文件夹之外的文件。

!!! warning
    我们不建议禁用 `RESTRICT_TO_WORKSPACE`，除非 AutoGPT 运行在无法造成任何损害的沙盒环境中（例如 Docker 或虚拟机）。

## 日志

活动日志、错误日志和调试日志位于 `logs` 目录中。

!!! tip
    是否注意到您的代理有异常行为？是否有有趣的用例？是否有要报告的 bug？
    按照以下步骤启用日志记录。在提交问题报告或与我们讨论问题时，可以包含这些日志。

要打印调试日志：

```shell
./autogpt.sh --debug
```

## 禁用命令

禁用命令的最佳方式是禁用或移除提供这些命令的[组件][components]。  
但如果您希望有选择性地禁用某些命令，可以在 `.env` 文件中使用 `DISABLED_COMMANDS` 配置项。  
将要禁用的命令名称以逗号分隔填入即可。  
内置组件中的命令列表可[在此查看][commands]。

例如，若要禁用 Python 编码功能，请将其设置为以下值：

```ini
DISABLED_COMMANDS=execute_python_code,execute_python_file
```

[components]: ../forge/components/components.md

[commands]: ../forge/components/built-in-components.md