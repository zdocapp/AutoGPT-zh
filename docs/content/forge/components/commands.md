# 🛠️ 命令

命令是智能体执行各种操作的方式；例如与用户或 API 交互以及使用工具。它们由实现 `CommandProvider` [⚙️ 协议](./protocols.md) 的组件提供。命令是可由智能体调用的函数，它们可以包含参数和返回值，这些都将被智能体看到。

```py
class CommandProvider(Protocol):
    def get_commands(self) -> Iterator[Command]:
        ...
```

## `command` 装饰器

提供命令最简单且推荐的方式是在组件方法上使用 `command` 装饰器，然后在提供者的 `get_commands` 中将其作为一部分 yield 出来。每个命令都需要一个名称、描述和参数模式 - `JSONSchema`。默认情况下，方法名称用作命令名称，文档字符串的第一部分（在第一个双换行符之前）用作描述，模式可以在装饰器中提供。

### `command` 装饰器使用示例

```py
# Assuming this is inside some component class
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

智能体将能够调用这个名为 `multiply` 的命令，它有两个参数，并将收到结果。命令描述将是：`将两个数字相乘。`

我们可以在装饰器中提供 `names` 和 `description`，上述命令等同于：

```py
@command(
    names=["multiply"],
    description="Multiplies two numbers.",
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
    def multiply_command(self, a: int, b: int) -> str:
        return str(a * b)
```

为了向智能体提供 `multiply` 命令，我们需要在 `get_commands` 中将其 yield 出来：

```py
def get_commands(self) -> Iterator[Command]:
    yield self.multiply
```

## 直接创建 `Command`

如果您不想使用装饰器，可以直接创建 `Command` 对象。

```py

def multiply(self, a: int, b: int) -> str:
        return str(a * b)

def get_commands(self) -> Iterator[Command]:
    yield Command(
        names=["multiply"],
        description="Multiplies two numbers.",
        method=self.multiply,
        parameters=[
            CommandParameter(name="a", spec=JSONSchema(
                type=JSONSchema.Type.INTEGER,
                description="The first number",
                required=True,
            )),
            CommandParameter(name="b", spec=JSONSchema(
                type=JSONSchema.Type.INTEGER,
                description="The second number",
                required=True,
            )),
        ],
    )
```