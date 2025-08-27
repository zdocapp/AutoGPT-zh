# 为 AutoGPT 创建挑战

🏹 我们正在寻找才华横溢的挑战创作者！🎯

加入我们，通过设计测试 AutoGPT 极限的挑战来塑造其未来。您的意见对于指导我们的进展和确保我们走在正确的轨道上将非常宝贵。我们正在寻找具备多样化技能的人才，包括：

🎨 UX 设计：您的专业知识将提升尝试攻克我们挑战的用户体验。在您的帮助下，我们将在维基中开发一个专门的部分，甚至可能推出一个独立的网站。

💻 编码技能：熟练掌握 Python、pytest 和 VCR（一个记录 OpenAI 调用并存储的库）对于创建引人入胜且稳健的挑战至关重要。

⚙️ DevOps 技能：在 GitHub 以及可能的 Google Cloud Platform 中拥有 CI 流水线经验将有助于简化我们的操作。

准备好扮演 AutoGPT 旅程中的关键角色了吗？立即通过提交 PR 申请成为挑战创作者！🚀

# 入门指南

克隆原始的 AutoGPT 仓库并切换到 master 分支

挑战并非使用特定框架编写，力求保持高度通用性。
挑战模拟了一个希望完成某事的用户：
输入：

- 用户需求
- 文件及其他输入

输出 => 产物（文件、图像、代码等）

## 定义您的智能体

前往 https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/original_autogpt/tests/integration/agent_factory.py

创建您的代理装置。

```python
def kubernetes_agent(
    agent_test_config, workspace: Workspace
):
    # Please choose the commands your agent will need to beat the challenges, the full list is available in the main.py
    # (we 're working on a better way to design this, for now you have to look at main.py)
    command_registry = CommandRegistry()
    command_registry.import_commands("autogpt.commands.file_operations")
    command_registry.import_commands("autogpt.app")

    # Define all the settings of our challenged agent
    ai_profile = AIProfile(
        ai_name="Kubernetes",
        ai_role="an autonomous agent that specializes in creating Kubernetes deployment templates.",
        ai_goals=[
            "Write a simple kubernetes deployment file and save it as a kube.yaml.",
        ],
    )
    ai_profile.command_registry = command_registry

    system_prompt = ai_profile.construct_full_prompt()
    agent_test_config.set_continuous_mode(False)
    agent = Agent(
        command_registry=command_registry,
        config=ai_profile,
        next_action_count=0,
        triggering_prompt=DEFAULT_TRIGGERING_PROMPT,
    )

    return agent
```

## 创建您的挑战

转到 `tests/challenges` 并创建一个名为 `test_your_test_description.py` 的文件，并将其添加到相应的文件夹中。如果不存在类别，您可以创建一个新的。

您的测试可能类似于这样

```python
import contextlib
from functools import wraps
from typing import Generator

import pytest
import yaml

from autogpt.commands.file_operations import read_file, write_to_file
from tests.integration.agent_utils import run_interaction_loop
from tests.challenges.utils import run_multiple_times

def input_generator(input_sequence: list) -> Generator[str, None, None]:
    """
    Creates a generator that yields input strings from the given sequence.

    :param input_sequence: A list of input strings.
    :return: A generator that yields input strings.
    """
    yield from input_sequence


@pytest.mark.skip("This challenge hasn't been beaten yet.")
@pytest.mark.vcr
@pytest.mark.requires_openai_api_key
def test_information_retrieval_challenge_a(kubernetes_agent, monkeypatch) -> None:
    """
    Test the challenge_a function in a given agent by mocking user inputs
    and checking the output file content.

    :param get_company_revenue_agent: The agent to test.
    :param monkeypatch: pytest's monkeypatch utility for modifying builtins.
    """
    input_sequence = ["s", "s", "s", "s", "s", "EXIT"]
    gen = input_generator(input_sequence)
    monkeypatch.setattr("autogpt.utils.session.prompt", lambda _: next(gen))

    with contextlib.suppress(SystemExit):
        run_interaction_loop(kubernetes_agent, None)

    # here we load the output file
    file_path = str(kubernetes_agent.workspace.get_path("kube.yaml"))
    content = read_file(file_path)

    # then we check if it's including keywords from the kubernetes deployment config
    for word in ["apiVersion", "kind", "metadata", "spec"]:
        assert word in content, f"Expected the file to contain {word}"

    content = yaml.safe_load(content)
    for word in ["Service", "Deployment", "Pod"]:
        assert word in content["kind"], f"Expected the file to contain {word}"


```