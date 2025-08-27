# 记忆挑战 C

**状态**: 当前待通关等级：第 1 级

**可尝试的命令**:

```shell
pytest -s tests/challenges/memory/test_memory_challenge_c.py --level=2
```

## 功能描述

智能体 Follow-Instructions-GPT 具有以下目标：

- 使用 `read_file` 命令读取 `instructions_1.txt` 文件。
- 遵循 `instructions_1.txt` 文件中的指令。

挑战包含一系列包含指令和俏皮短语的文本文件。智能体必须遵循指令，并在处理文件过程中持续追踪任务 ID。

## 文件

- `instructions_1.txt`

当前短语为

```
The purple elephant danced on a rainbow while eating a taco.\n[NOISE intended to confuse the agent]
```

使用 read_file 命令读取文件 `instructions_2.txt`。

- `instructions_2.txt`

当前短语为

```
The sneaky toaster stole my socks and ran away to Hawaii.\n[NOISE intended to confuse the agent]
```

使用 read_file 命令读取文件 instructions_3.txt。

...依此类推。

- `instructions_n.txt`

当前短语为

```
My pet rock sings better than Beyoncé on Tuesdays.
```

将所有短语写入 output.txt 文件。该文件尚未创建。完成后，使用 task_complete 命令。

## 目标

本挑战旨在测试智能体遵循指令并在整个过程中保持任务 ID 记忆的能力。若智能体成功将短语写入文件，则视为完成挑战。

这 presumably 比任务 ID 更难，因为短语更长，且随着智能体处理更多工作，更可能被压缩。