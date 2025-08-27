# 🤖 智能体

智能体由[🧩 组件](./components.md)构成，负责执行流水线及一些附加逻辑。所有智能体的基类是 `BaseAgent`，它具备收集组件和执行协议的必要逻辑。

## 重要方法

`BaseAgent` 提供了所有智能体正常运行所需的两个抽象方法：

1. `propose_action`：该方法负责根据智能体当前状态提出行动建议，返回 `ThoughtProcessOutput`。
2. `execute`：该方法负责执行提出的行动，返回 `ActionResult`。

## AutoGPT 智能体

`Agent` 是 AutoGPT 提供的主智能体。它是 `BaseAgent` 的子类，包含所有[内置组件](./built-in-components.md)。`Agent` 实现了 `BaseAgent` 的核心抽象方法：`propose_action` 和 `execute`。

## 构建自定义智能体

构建自定义智能体的最简单方法是扩展 `Agent` 类并添加额外组件。通过这种方式，您可以复用现有组件以及执行[⚙️ 协议](./protocols.md)的默认逻辑。

```py
class MyComponent(AgentComponent):
    pass

class MyAgent(Agent):
    def __init__(
        self,
        settings: AgentSettings,
        llm_provider: MultiProvider
        file_storage: FileStorage,
        app_config: AppConfig,
    ):
        # Call the parent constructor to bring in the default components
        super().__init__(settings, llm_provider, file_storage, app_config)
        # Add your custom component
        self.my_component = MyComponent()
```

如需更多自定义选项，您可以重写 `propose_action` 和 `execute` 方法，甚至直接继承 `BaseAgent` 类。通过这种方式，您可以完全控制智能体的组件和行为。请查看 [Agent 的实现](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/agents/agent.py) 获取更多详细信息。