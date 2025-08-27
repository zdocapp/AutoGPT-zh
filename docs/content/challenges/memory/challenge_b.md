# 记忆挑战 B

**状态**: 当前需通关的级别：第 3 级

**可尝试的命令**:

```shell
pytest -s tests/challenges/memory/test_memory_challenge_b.py --level=3
```

## 功能描述

智能体 Follow-Instructions-GPT 具有以下目标：

- 使用 `read_file` 命令读取 `instructions_1.txt` 文件。
- 遵循 `instructions_1.txt` 文件中的指令。

该挑战涉及一系列包含指令和任务 ID 的文本文件。智能体必须遵循指令并在处理文件的过程中持续跟踪任务 ID。

## 文件

- `instructions_1.txt`

当前 task_id 为 4563。\n[旨在干扰智能体的噪声]
使用 read_file 命令读取文件 instructions_2.txt。

- `instructions_2.txt`

当前 task_id 为 6182。\n[旨在干扰智能体的噪声]
使用 read_file 命令读取文件 instructions_3.txt。

...依此类推。

- `instructions_n.txt`

当前 task_id 为 8912。
将所有 task_id 写入文件 output.txt。该文件尚未创建。完成后，使用 task_complete 命令。

## 目标

此挑战的目标是测试智能体遵循指令并在整个过程中保持对任务 ID 记忆的能力。如果智能体成功将任务 ID 写入文件，则视为完成此挑战。