# 记忆挑战 D

**状态**: 当前待挑战级别：第 1 级

**可尝试的命令**:

```shell
pytest -s tests/challenges/memory/test_memory_challenge_d.py --level=1
```

## 功能描述

提供的代码是一个单元测试，旨在验证 AI 在涉及移动物体（特别是弹珠）的故事中跟踪角色事件和信念的能力。该场景是经典"莎莉-安妮测试"的高级形式，这是一种用于衡量儿童社会认知能力的心理测试，以理解他人观点和信念可能与自己不同。

以下是挑战的说明：

AI 将接收一系列涉及角色 Sally、Anne、Bob 和 Charlie 以及不同弹珠移动的事件。这些事件被设计为复杂度递增的测试级别。

对于每个级别，AI 需要跟踪事件以及每个角色对每个弹珠位置的最终信念。这些信念受角色在事件发生时是否在房间内的影响，因为在房间内的角色知晓这些动作，而在房间外的角色则不知情。

在 AI 处理事件并生成每个角色的信念后，它会将这些信念以 JSON 格式写入输出文件。

随后，check_beliefs 函数将 AI 的信念与该级别的预期信念进行比对。预期信念是预定义的，代表了每个级别事件的正确解释。

如果AI的信念与预期信念相符，意味着AI正确解读了事件以及每个角色的视角。这表明AI已通过该级别的测试。

测试会运行至AI已成功通过的最高级别，或用户指定的级别。

## 文件

- `instructions_1.txt`

```
Sally has a marble (marble A) and she puts it in her basket (basket S), then leaves the room. Anne moves marble A from Sally's basket (basket S) to her own basket (basket A).
```

- `instructions_2.txt`

```
Sally gives a new marble (marble B) to Bob who is outside with her. Bob goes into the room and places marble B into Anne's basket (basket A). Anne tells Bob to tell Sally that he lost the marble b. Bob leaves the room and speaks to Sally about the marble B. Meanwhile, after Bob left the room, Anne moves marble A into the green box, but tells Charlie to tell Sally that marble A is under the sofa. Charlie leaves the room and speak to Sally about the marble A as instructed by Anne.
```

...依此类推。

- `instructions_n.txt`

所有角色的预期信念以列表形式给出：

```json
expected_beliefs = {
    1: {
        'Sally': {
            'marble A': 'basket S',
        },
        'Anne': {
            'marble A': 'basket A',
        }
    },
    2: {
        'Sally': {
            'marble A': 'sofa',  # Because Charlie told her
        },
        'Anne': {
            'marble A': 'green box',  # Because she moved it there
            'marble B': 'basket A',  # Because Bob put it there and she was in the room
        },
        'Bob': {
            'B': 'basket A',  # Last place he put it
        },
        'Charlie': {
            'A': 'sofa',  # Because Anne told him to tell Sally so
        }
    },...
```

## 目标

该测试本质上检验的是AI能否根据角色对事件的认知，准确建模并追踪不同角色的信念——这是理解和生成类人叙事的关键能力。这种能力对于故事创作、对话系统等任务大有裨益。