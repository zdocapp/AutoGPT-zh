# AutoGPT 智能体

[🔧 **安装配置**](setup/index.md)
&ensp;|&ensp;
[💻 **使用指南**](./usage.md)
&ensp;|&ensp;
[🐙 **GitHub**](https://github.com/Significant-Gravitas/AutoGPT/tree/master/autogpt)

**位置：** GitHub 仓库中的 `classic/original_autogpt/` 目录

**维护声明：** AutoGPT Classic 版本不再提供安全支持。
依赖项将不再更新，问题也不会修复。如果有人希望参与新功能开发，
我们将尽力合并通过现有 CI 检查的变更。

AutoGPT Classic 诞生于 OpenAI 发布 GPT-4 模型之际，伴随的论文阐述了该模型
先进的推理和任务解决能力。其概念（至今依然）相当简单：让大型语言模型反复决策，
同时将其行动结果反馈至提示词中。这使得程序能够以迭代和渐进的方式实现目标。

该程序能够代表用户执行操作，这使其成为一个**智能体**。在 AutoGPT Classic 中，
用户仍需授权每个操作，但随着项目进展，我们将赋予智能体更高自主权，
仅对特定操作需要用户确认。

AutoGPT Classic 是一个**通用型智能体**，这意味着它并非为特定任务而设计。相反，它被设计为能够在计算机上执行跨多个学科的广泛任务。

# AutoGPT Classic 文档

欢迎来到 AutoGPT Classic 文档。

AutoGPT Classic 项目包含四个主要组件：

- [智能体（Agent）](#agent) &ndash; 也称为 "AutoGPT Classic"
- [基准测试（Benchmark）](#benchmark) &ndash; 又名 `agbenchmark`
- [锻造厂（Forge）](#forge)
- [前端（Frontend）](#frontend)

为了将这些组件整合在一起，我们在项目根目录还提供了一个 [CLI]。

## 🤖 智能体

**[📖 关于 AutoGPT Classic](#autogpt-agent)**
&ensp;|&ensp;
**[🔧 设置](setup/index.md)**
&ensp;|&ensp;
**[💻 使用指南](./usage.md)**

这是 AutoGPT 的前核心项目，也是一切的起点：一个由大型语言模型驱动的半自主智能体，可为您执行任何任务*。

我们持续开发这个项目，目标是向大众提供人工智能辅助服务，并以透明和协作的方式共同构建未来。

- 💡 **探索** - 看看人工智能能做什么，并通过对未来的一瞥获得灵感。

- 🚀 **与我们共建** - 我们欢迎任何形式的贡献，无论是代码还是新功能或改进的想法！加入我们的 [Discord](https://discord.gg/autogpt)，了解如何参与其中。

如果您想了解后续发展，请查看 [AutoGPT 平台](../index.md)。

<small>* 虽然尚未完全实现，但这是我们持续追求的终极目标</small>

---

## 🎯 基准测试

**[🗒️ 说明文档](https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/benchmark/README.md)**

衡量您智能体的性能！`agbenchmark` 可与任何支持智能体协议的智能体配合使用，且与项目 [CLI] 的集成使其在 AutoGPT Classic 和基于 forge 的智能体上使用更加便捷。该基准测试提供严格的测试环境。我们的框架支持自主、客观的性能评估，确保您的智能体为实际应用做好充分准备。

<!-- TODO: insert visual demonstrating the benchmark -->

- 📦 [**`agbenchmark`**](https://pypi.org/project/agbenchmark/) 于 Pypi

- 🔌 **智能体协议标准化** - AutoGPT Classic 采用 AI Engineer Foundation 的智能体协议，确保与项目内外众多智能体的兼容性。

---

## 🏗️ Forge

**[📖 介绍](../forge/get-started.md)**
&ensp;|&ensp;
**[🚀 快速入门](https://github.com/Significant-Gravitas/AutoGPT/blob/master/QUICKSTART.md)**

<!-- TODO: have the guides all in one place -->

打造您自己的智能体！Forge 是一个即用型智能体应用模板。所有样板代码均已处理完毕，让您能将全部创造力专注于让*您的*智能体脱颖而出的核心特性。

- 🛠️ **轻松构建** - 我们已打好基础，让您能专注于智能体个性与能力开发。完整教程请参阅[此处](https://aiedge.medium.com/autogpt-forge-e3de53cc58ec)。

---

## 💻 前端

**[🗒️ 说明文档](https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/frontend/README.md)**

为任何符合智能体协议规范的智能体提供易用的开源前端界面。

- 🎮 **用户友好界面** - 轻松管理您的智能体

- 🔄 **无缝集成** - 实现智能体与基准测试系统的流畅连接

---

## 🔧 命令行界面

[CLI]: #cli

项目CLI可便捷调用代码库中AutoGPT Classic的所有组件，支持单独或组合使用。只需运行`./run setup`安装依赖，即可立即开始使用！

```shell
$ ./run
Usage: cli.py [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  agent      Commands to create, start and stop agents
  benchmark  Commands to start the benchmark and list tests and categories
  setup      Installs dependencies needed for your system.
```

常用命令：

* `./run agent start autogpt` &ndash; [运行](./usage.md#serve-agent-protocol-mode-with-ui) AutoGPT Classic智能体
* `./run agent create <名称>` &ndash; 在`agents/<名称>`路径创建基于Forge的新智能体项目
* `./run benchmark start <智能体>` &ndash; 对指定智能体进行基准测试

---

🤔 如有疑问请加入AutoGPT Discord服务器：
[discord.gg/autogpt](https://discord.gg/autogpt)

### 术语表

- **Repository**: 存储项目代码的仓库空间。
- **Forking**: 将仓库复制到您账户下的操作。
- **Cloning**: 创建仓库的本地副本。
- **Agent**: 您将创建和开发的 AutoGPT 智能体。
- **Benchmarking**: 在 Forge 环境中测试智能体能力的基准评估。
- **Forge**: 用于构建 AutoGPT 智能体的模板框架。
- **Frontend**: 用于任务管理、日志查看和历史记录的用户界面。