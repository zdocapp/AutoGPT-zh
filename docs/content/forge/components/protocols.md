# ⚙️ 协议

协议是由[组件](./components.md)实现的*接口*，用于将相关功能分组。每个协议都需要在执行的某个时刻由智能体显式处理。我们提供了内置协议的完整列表，这些协议已在内置的 `Agent` 中得到处理，因此当您继承基础智能体时，所有内置协议都将正常工作！

**协议按默认执行顺序列出。**

## 顺序无关协议

仅实现顺序无关协议的组件可以按任意顺序添加，包括在有序协议之间添加。

### `DirectiveProvider`

为智能体提供约束条件、资源和最佳实践。这对其他协议没有直接影响；纯粹是信息性的，在构建提示时将传递给大语言模型。

```py
class DirectiveProvider(AgentComponent):
    def get_constraints(self) -> Iterator[str]:
        return iter([])

    def get_resources(self) -> Iterator[str]:
        return iter([])

    def get_best_practices(self) -> Iterator[str]:
        return iter([])
```

**示例** 网络搜索组件可以提供资源信息。请注意，这实际上并不允许智能体访问互联网。要实现这一点，需要提供相关的 `Command`。

```py
class WebSearchComponent(DirectiveProvider):
    def get_resources(self) -> Iterator[str]:
        yield "Internet access for searches and information gathering."
    # We can skip "get_constraints" and "get_best_practices" if they aren't needed
```

### `CommandProvider`

提供可由智能体执行的命令。

```py
class CommandProvider(AgentComponent):
    def get_commands(self) -> Iterator[Command]:
        ...
```

提供命令的最简单方式是在组件方法上使用 `command` 装饰器，然后 yield 该方法。每个命令都需要名称、描述和使用 `JSONSchema` 的参数模式。默认情况下，方法名称用作命令名称，文档字符串的第一部分（在 `Args:` 或 `Returns:` 之前）用作描述，模式可以在装饰器中提供。

**示例** 能够执行乘法运算的计算器组件。如果与当前任务相关，代理能够调用此命令，并将看到返回的结果。

```py
from forge.agent import CommandProvider, Component
from forge.command import command
from forge.models.json_schema import JSONSchema


class CalculatorComponent(CommandProvider):
    get_commands(self) -> Iterator[Command]:
        yield self.multiply

    @command(parameters={
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

智能体将能够调用这个名为 `multiply` 的命令，它有两个参数，并将收到结果。命令描述将是：`将两个数字相乘。`

要了解更多关于命令的信息，请参阅 [🛠️ 命令](./commands.md)。

## 顺序依赖协议

实现顺序依赖协议的组件顺序很重要。
某些组件可能依赖于它们之前组件的结果。

### `MessageProvider`

生成将被添加到代理提示中的消息。您可以使用 `ChatMessage.user()`：这将被解释为用户发送的消息，或 `ChatMessage.system()`：这将更加重要。

```py
class MessageProvider(AgentComponent):
    def get_messages(self) -> Iterator[ChatMessage]:
        ...
```

**示例** 向代理提示提供消息的组件。

```py
class HelloComponent(MessageProvider):
    def get_messages(self) -> Iterator[ChatMessage]:
        yield ChatMessage.user("Hello World!")
```

### `AfterParse`

在响应解析后调用的协议。

```py
class AfterParse(AgentComponent):
    def after_parse(self, response: ThoughtProcessOutput) -> None:
        ...
```

**示例** 在响应解析后记录日志的组件。

```py
class LoggerComponent(AfterParse):
    def after_parse(self, response: ThoughtProcessOutput) -> None:
        logger.info(f"Response: {response}")
```

### `ExecutionFailure`

当命令执行失败时调用的协议。

```py
class ExecutionFailure(AgentComponent):
    @abstractmethod
    def execution_failure(self, error: Exception) -> None:
        ...
```

**示例** 在命令失败时记录错误的组件。

```py
class LoggerComponent(ExecutionFailure):
    def execution_failure(self, error: Exception) -> None:
        logger.error(f"Command execution failed: {error}")
```

### `AfterExecute`

协议在代理成功执行命令后被调用。

```py
class AfterExecute(AgentComponent):
    def after_execute(self, result: ActionResult) -> None:
        ...
```

**示例** 在执行命令后记录结果的组件。

```py
class LoggerComponent(AfterExecute):
    def after_execute(self, result: ActionResult) -> None:
        logger.info(f"Result: {result}")
```