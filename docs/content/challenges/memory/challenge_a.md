# 记忆挑战 A

**状态**: 挑战已完成

## 功能描述

智能体 Follow-Instructions-GPT 具有以下目标：

- 使用 `read_file` 命令读取 `instructions_1.txt` 文件
- 遵循 `instructions_1.txt` 文件中的指令

本挑战涉及一系列包含指令的文本文件。智能体必须遵循指令并在处理文件过程中持续跟踪任务 ID。

## 文件

- `instructions_1.txt`

任务 ID 为 1234
读取文件 instructions_1.txt

- `instructions_2.txt`

使用 read_file 命令读取 instructions_2.txt 文件

...依此类推

- `instructions_n.txt`

将 task_id 写入 output.txt 文件

## 目标

本挑战的目标是测试智能体遵循指令并在整个过程中保持对任务 ID 记忆的能力。如果智能体成功将任务 ID 写入文件，则视为完成本挑战。