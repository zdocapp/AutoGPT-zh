# 创建组件

## 最小组件

组件可用于实现各种功能，例如向提示提供消息、执行代码或与外部服务交互。

*组件*是一个继承自 `AgentComponent` 的类，或者实现一个或多个*协议*。每个*协议*都继承 `AgentComponent`，因此一旦继承任何*协议*，您的类就会自动成为*组件*。

```py
class MyComponent(AgentComponent):
    pass
```

这已经是一个有效的组件，但它还没有任何功能。要为其添加一些功能，您需要实现一个或多个*协议*。

让我们创建一个简单的组件，将"Hello World!"消息添加到代理的提示中。为此，我们需要在我们的组件中实现 `MessageProvider` *协议*。`MessageProvider` 是一个带有 `get_messages` 方法的接口：

```py
# No longer need to inherit AgentComponent, because MessageProvider already does it
class HelloComponent(MessageProvider):
    def get_messages(self) -> Iterator[ChatMessage]:
        yield ChatMessage.user("Hello World!")
```

现在我们可以将我们的组件添加到现有代理中，或者创建一个新的 Agent 类并将其添加到其中：

```py
class MyAgent(Agent):
    self.hello_component = HelloComponent()
```

`get_messages` 将在代理每次需要构建新提示时被调用，产生的消息将相应添加。

## 向组件传递数据及组件间传递数据

由于组件是常规类，您可以通过 `__init__` 方法向它们传递数据（包括其他组件）。
例如，我们可以传递一个配置对象，然后在需要时从中检索 API 密钥：

```py
class DataComponent(MessageProvider):
    def __init__(self, config: Config):
        self.config = config

    def get_messages(self) -> Iterator[ChatMessage]:
        if self.config.openai_credentials.api_key:
            yield ChatMessage.system("API key found!")
        else:
            yield ChatMessage.system("API key not found!")
```

!!! note
    组件特定的配置处理尚未实现。

## 配置组件

组件可以通过 pydantic 模型进行配置。
要使组件可配置，它必须继承自 `ConfigurableComponent[BM]`，其中 `BM` 是继承自 pydantic 的 `BaseModel` 的配置类。
您应该将配置实例传递给 `ConfigurableComponent` 的 `__init__` 方法，或者直接设置其 `config` 属性。
使用配置允许您从文件加载配置，并且可以轻松地为任何代理进行序列化和反序列化。
要了解更多关于配置的信息，包括存储敏感信息和序列化，请参阅[组件配置](./components.md#component-configuration)。

```py
# Example component configuration
class UserGreeterConfiguration(BaseModel):
    user_name: str

class UserGreeterComponent(MessageProvider, ConfigurableComponent[UserGreeterConfiguration]):
    def __init__(self):
        # Creating configuration instance
        # You could also pass it to the component constructor
        # e.g. `def __init__(self, config: UserGreeterConfiguration):`
        config = UserGreeterConfiguration(user_name="World")
        # Passing the configuration instance to the parent class
        UserGreeterComponent.__init__(self, config)
        # This has the same effect as the line above:
        # self.config = UserGreeterConfiguration(user_name="World")

    def get_messages(self) -> Iterator[ChatMessage]:
        # You can use the configuration like a regular model
        yield ChatMessage.system(f"Hello, {self.config.user_name}!")
```

## 提供命令

要扩展代理的功能，您需要使用 `CommandProvider` 协议提供命令。例如，为了让代理能够将两个数字相乘，您可以创建如下组件：

```py
class MultiplicatorComponent(CommandProvider):
    def get_commands(self) -> Iterator[Command]:
        # Yield the command so the agent can use it
        yield self.multiply

    @command(
    parameters={
        "a": JSONSchema(
            type=JSONSchema.Type.INTEGER,
            description="The first number",
            required=True,
        ),
        "b": JSONSchema(
            type=JSONSchema.Type.INTEGER,
            description="The second number",
            required=True,
        )})
    def multiply(self, a: int, b: int) -> str:
        """
        Multiplies two numbers.
        
        Args:
            a: First number
            b: Second number

        Returns:
            Result of multiplication
        """
        return str(a * b)
```

要了解更多关于命令的信息，请参阅[🛠️ 命令](./commands.md)。

## 提示结构

在组件提供所有必要数据后，代理需要构建最终将发送给大语言模型的提示。
目前，`PromptStrategy`（*不是*协议）负责构建最终提示。

如果您想要更改提示构建的方式，需要创建一个新的 `PromptStrategy` 类，然后在您的智能体类中调用相关方法。
您可以查看 AutoGPT 智能体使用的默认策略：[OneShotAgentPromptStrategy](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/agents/prompt_strategies/one_shot.py)，以及它在 [Agent](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/agents/agent.py) 中的使用方式（搜索 `self.prompt_strategy`）。

## 示例 `UserInteractionComponent`

让我们创建一个内置智能体使用的组件的简化版本。
它使智能体能够在终端中向用户请求输入。

1. 创建一个继承自 `CommandProvider` 的组件类。

    ```py
    class MyUserInteractionComponent(CommandProvider):
        """提供与用户交互的命令。"""
        pass
    ```

2. 实现一个命令方法，该方法将向用户询问输入并返回结果。

    ```py
    def ask_user(self, question: str) -> str:
        """如果你需要关于给定目标的更多细节或信息，
        可以向用户询问输入。"""
        print(f"\nQ: {question}")
        resp = input("A:")
        return f"用户的回答: '{resp}'"
    ```

3. 该命令需要使用 `@command` 装饰器进行装饰。

    ```py
    @command(
        parameters={
            "question": JSONSchema(
                type=JSONSchema.Type.STRING,
                description="向用户提出的问题或提示",
                required=True,
            )
        },
    )
    def ask_user(self, question: str) -> str:
        """如果你需要关于给定目标的更多细节或信息，
        可以向用户询问输入。"""
        print(f"\nQ: {question}")
        resp = input("A:")
        return f"用户的回答: '{resp}'"
    ```

4. 需要实现 `CommandProvider` 的 `get_commands` 方法来生成命令。

    ```py
    def get_commands(self) -> Iterator[Command]:
        yield self.ask_user
    ```

5. 由于代理并不总是在终端或交互模式下运行，当无法请求用户输入时，需要通过设置 `self._enabled=False` 来禁用该组件。

    ```py
    def __init__(self, interactive_mode: bool):
        self.config = config
        self._enabled = interactive_mode
    ```

最终组件应该看起来像这样：

```py
# 1.
class MyUserInteractionComponent(CommandProvider):
    """Provides commands to interact with the user."""

    # We pass config to check if we're in noninteractive mode
    def __init__(self, interactive_mode: bool):
        self.config = config
        # 5.
        self._enabled = interactive_mode

    # 4.
    def get_commands(self) -> Iterator[Command]:
        # Yielding the command so the agent can use it
        # This won't be yielded if the component is disabled
        yield self.ask_user

    # 3.
    @command(
        # We need to provide a schema for ALL the command parameters
        parameters={
            "question": JSONSchema(
                type=JSONSchema.Type.STRING,
                description="The question or prompt to the user",
                required=True,
            )
        },
    )
    # 2.
    # Command name will be its method name and description will be its docstring
    def ask_user(self, question: str) -> str:
        """If you need more details or information regarding the given goals,
        you can ask the user for input."""
        print(f"\nQ: {question}")
        resp = input("A:")
        return f"The user's answer: '{resp}'"
```

现在如果我们想要使用自己的用户交互*而不是*默认的交互，我们需要以某种方式移除默认的交互（如果我们的代理继承自 `Agent`，默认的交互会被继承）并添加我们自己的。我们可以简单地在 `__init__` 方法中重写 `user_interaction`：

```py
class MyAgent(Agent):
    def __init__(
        self,
        settings: AgentSettings,
        llm_provider: MultiProvider,
        file_storage: FileStorage,
        app_config: Config,
    ):
        # Call the parent constructor to bring in the default components
        super().__init__(settings, llm_provider, file_storage, app_config)
        # Disable the default user interaction component by overriding it
        self.user_interaction = MyUserInteractionComponent()
```

或者，我们可以通过将其设置为 `None` 来禁用默认组件：

```py
class MyAgent(Agent):
    def __init__(
        self,
        settings: AgentSettings,
        llm_provider: MultiProvider,
        file_storage: FileStorage,
        app_config: Config,
    ):
        # Call the parent constructor to bring in the default components
        super().__init__(settings, llm_provider, file_storage, app_config)
        # Disable the default user interaction component
        self.user_interaction = None
        # Add our own component
        self.my_user_interaction = MyUserInteractionComponent(app_config)
```

## 了解更多

查看更多示例的最佳位置是查看 [classic/original_autogpt/components](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/components/) 和 [classic/original_autogpt/commands](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/commands/) 目录中的内置组件。

关于如何扩展内置代理并构建自己的指南：[🤖 代理](./agents.md)  
某些组件的顺序很重要，请参阅 [🧩 组件](./components.md) 以了解更多关于组件及其自定义方式的信息。  
要查看内置协议及相应示例，请访问 [⚙️ 协议](./protocols.md)。