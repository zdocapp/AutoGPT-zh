# 信息检索挑战 A

**状态**: 当前需超越的级别：第 2 级

**尝试命令**:

```
pytest -s tests/challenges/information_retrieval/test_information_retrieval_challenge_a.py --level=2
```

## 功能描述

智能体的目标是查找特斯拉的营收数据：

- 第 1 级要求查询特斯拉 2022 年的营收，并明确要求搜索 'tesla revenue 2022'
- 第 2 级要求相同，但不要求搜索 'tesla revenue 2022'
- 第 3 级要求查询特斯拉自成立以来按年份统计的营收

需将结果写入名为 output.txt 的文件中

智能体必须能够持续通过此测试（这是最困难的部分）

## 目标

本挑战旨在测试智能体以一致方式检索信息的能力