# 🧩 组件

组件是 [🤖 智能体](./agents.md) 的构建模块。它们是继承 `AgentComponent` 或实现一个或多个 [⚙️ 协议](./protocols.md) 的类，为智能体提供额外能力或处理功能。

组件可用于实现各种功能，如向提示提供消息、执行代码或与外部服务交互。
它们可以被启用或禁用、排序，并且可以相互依赖。

在智能体的 `__init__` 中通过 `self` 分配的组件会在智能体实例化时自动检测到。
例如在 `__init__` 中：`self.my_component = MyComponent()`。
您可以使用任何有效的 Python 变量名，组件被检测的关键在于其类型（`AgentComponent` 或任何继承自它的协议）。

访问 [内置组件](./built-in-components.md) 查看开箱即用的可用组件。

```py
from forge.agent import BaseAgent
from forge.agent.components import AgentComponent

class HelloComponent(AgentComponent):
    pass

class SomeComponent(AgentComponent):
    def __init__(self, hello_component: HelloComponent):
        self.hello_component = hello_component

class MyAgent(BaseAgent):
    def __init__(self):
        # These components will be automatically discovered and used
        self.hello_component = HelloComponent()
        # We pass HelloComponent to SomeComponent
        self.some_component = SomeComponent(self.hello_component)
```

## 组件配置

每个组件都可以使用常规的 pydantic `BaseModel` 来定义其自身的配置。
为确保配置能从文件正确加载，组件必须继承自 `ConfigurableComponent[BM]`，其中 `BM` 是其所使用的配置模型。
`ConfigurableComponent` 提供了一个 `config` 属性，用于保存配置实例。
可以直接设置 `config` 属性，或将配置实例传递给组件的构造函数。
额外的配置（即不属于智能体组成部分的组件配置）可以被传递，但会被静默忽略。即使稍后添加了组件，额外的配置也不会被应用。
要查看内置组件的配置，请访问[内置组件](./built-in-components.md)。

```py
from pydantic import BaseModel
from forge.agent.components import ConfigurableComponent

class MyConfig(BaseModel):
    some_value: str

class MyComponent(AgentComponent, ConfigurableComponent[MyConfig]):
    def __init__(self, config: MyConfig):
        super().__init__(config)
        # This has the same effect as above:
        # self.config = config

    def get_some_value(self) -> str:
        # Access the configuration like a regular model
        return self.config.some_value
```

### 敏感信息

虽然可以直接在代码中将敏感数据传递给配置，但建议对 API 密钥等敏感数据使用 `UserConfigurable(from_env="ENV_VAR_NAME", exclude=True)` 字段。
数据将从环境变量中加载，但请注意，代码中传递的值具有优先权。
所有字段，即使是排除的字段（`exclude=True`），在从文件加载配置时都会被加载。
排除允许您在*序列化*过程中跳过它们，未排除的 `SecretStr` 将按字面序列化为 `"**********"` 字符串。

```py
from pydantic import BaseModel, SecretStr
from forge.models.config import UserConfigurable

class SensitiveConfig(BaseModel):
    api_key: SecretStr = UserConfigurable(from_env="API_KEY", exclude=True)
```

### 配置序列化

`BaseAgent` 提供了两种方法：

1. `dump_component_configs`: 将所有组件的配置序列化为 JSON 字符串。
1. `load_component_configs`: 将 JSON 字符串反序列化为配置并应用。

### JSON 配置

您可以在启动代理时指定一个 JSON 文件（例如 `config.json`）用于配置。
该文件包含 AutoGPT 使用的各个[组件](../components/introduction.md)的设置。
使用 `--component-config-file` CLI 选项指定文件，例如使用 `config.json`：

```shell
./autogpt.sh run --component-config-file config.json
```

!!! note
    如果您使用 Docker 运行 AutoGPT，需要将配置文件挂载或复制到容器中。
    更多信息请参阅 [Docker 指南](../../classic/setup/docker.md)。

### JSON 配置示例

您可以复制想要更改的配置，例如到 `classic/original_autogpt/config.json` 并根据需要进行修改。
*大多数配置都有默认值，最好只设置您想要修改的值。*
您可以在[内置组件](./built-in-components.md)中查看可用的配置字段和默认值。
您也可以在 `.json` 文件中设置敏感变量，但建议使用环境变量代替。

```json
{
    "CodeExecutorConfiguration": {
        "execute_local_commands": false,
        "shell_command_control": "allowlist",
        "shell_allowlist": ["cat", "echo"],
        "shell_denylist": [],
        "docker_container_name": "agent_sandbox"
    },
    "FileManagerConfiguration": {
        "storage_path": "agents/AutoGPT/",
        "workspace_path": "agents/AutoGPT/workspace"
    },
    "GitOperationsConfiguration": {
        "github_username": null
    },
    "ActionHistoryConfiguration": {
        "llm_name": "gpt-3.5-turbo",
        "max_tokens": 1024,
        "spacy_language_model": "en_core_web_sm"
    },
    "ImageGeneratorConfiguration": {
        "image_provider": "dalle",
        "huggingface_image_model": "CompVis/stable-diffusion-v1-4",
        "sd_webui_url": "http://localhost:7860"
    },
    "WebSearchConfiguration": {
        "duckduckgo_max_attempts": 3
    },
    "WebSeleniumConfiguration": {
        "llm_name": "gpt-3.5-turbo",
        "web_browser": "chrome",
        "headless": true,
        "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36",
        "browse_spacy_language_model": "en_core_web_sm"
    }
}

```

## 组件排序

组件的执行顺序很重要，因为某些组件可能依赖于前一个组件的结果。
**默认情况下，组件按字母顺序排序。**

### 单个组件排序

您可以通过将其他组件（或其类型）传递给 `run_after` 方法来排序单个组件。这样可以确保该组件在指定组件之后执行。
`run_after` 方法返回组件本身，因此您可以在将组件分配给变量时调用它：

```py
class MyAgent(Agent):
    def __init__(self):
        self.hello_component = HelloComponent()
        self.calculator_component = CalculatorComponent().run_after(self.hello_component)
        # This is equivalent to passing a type:
        # self.calculator_component = CalculatorComponent().run_after(HelloComponent)
```

!!! warning
    排序组件时请确保不要产生循环依赖！

### 排序所有组件

您还可以通过在代理的 `__init__` 方法中设置 `self.components` 列表来排序所有组件。
这种方式确保没有循环依赖，并且任何 `run_after` 调用都会被忽略。

!!! warning
    请确保包含所有组件 - 通过设置 `self.components` 列表，您将覆盖自动发现组件的默认行为。由于这通常不是预期的做法，如果有组件被跳过，代理将在终端中通知您。

```py
class MyAgent(Agent):
    def __init__(self):
        self.hello_component = HelloComponent()
        self.calculator_component = CalculatorComponent()
        # Explicitly set components list
        self.components = [self.hello_component, self.calculator_component]
```

## 禁用组件

您可以通过设置组件的 `_enabled` 属性来控制哪些组件被启用。
组件默认情况下是*启用*的。
可以提供一个 `bool` 值或一个 `Callable[[], bool]`，每次组件即将执行时都会检查。这样您可以根据某些条件动态启用或禁用组件。
您还可以通过设置 `_disabled_reason` 来提供禁用组件的原因。该原因将在调试信息中可见。

```py
class DisabledComponent(MessageProvider):
    def __init__(self):
        # Disable this component
        self._enabled = False
        self._disabled_reason = "This component is disabled because of reasons."

        # Or disable based on some condition, either statically...:
        self._enabled = self.some_property is not None
        # ... or dynamically:
        self._enabled = lambda: self.some_property is not None

    # This method will never be called
    def get_messages(self) -> Iterator[ChatMessage]:
        yield ChatMessage.user("This message won't be seen!")

    def some_condition(self) -> bool:
        return False
```

如果您完全不需要该组件，可以直接从代理的 `__init__` 方法中移除它。如果您想移除从父类继承的组件，可以将相关属性设置为 `None`：

!!! Warning
    移除其他组件所需的组件时请务必小心。这可能导致错误和意外行为。

```py
class MyAgent(Agent):
    def __init__(self):
        super().__init__(...)
        # Disable WatchdogComponent that is in the parent class
        self.watchdog = None

```

## 异常处理

提供了自定义错误类，可用于在出现问题时控制执行流程。所有这些错误都可以在协议方法中抛出，并会被代理捕获。
默认情况下，代理会重试三次，如果问题仍未解决，则会重新抛出异常。所有传递的参数都会自动处理，并在需要时回滚值。
所有错误都接受可选的 `str` 类型消息。以下是按范围从小到大排序的错误类型：

1. `ComponentEndpointError`：单个端点方法执行失败。代理将重试该组件上此端点的执行。
2. `EndpointPipelineError`：管道执行失败。代理将重试所有组件的端点执行。
3. `ComponentSystemError`：多个管道执行失败。

**应用示例**

```py
from forge.agent.components import ComponentEndpointError
from forge.agent.protocols import MessageProvider

# Example of raising an error
class MyComponent(MessageProvider):
    def get_messages(self) -> Iterator[ChatMessage]:
        # This will cause the component to always fail 
        # and retry 3 times before re-raising the exception
        raise ComponentEndpointError("Endpoint error!")
```