# GitHub Issues

## Github 评论

### 功能说明

一个可在 GitHub issues 或 pull requests 上发布评论的功能块。

### 功能描述

该功能块允许用户使用 GitHub API 向现有的 GitHub issues 或 pull requests 添加评论。

### 工作原理

该功能块接收 GitHub 凭据、issue 或 pull request 的 URL 以及评论文本作为输入。随后它会向 GitHub API 发送请求，在指定的 issue 或 pull request 上发布评论。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| Credentials | GitHub 认证信息 |
| Issue URL | 将要发布评论的 GitHub issue 或 pull request 的 URL |
| Comment | 要发布的评论文本内容 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| ID | 已创建评论的唯一标识符 |
| URL | 指向 GitHub 上已发布评论的直接链接 |
| Error | 如果评论发布失败，显示任何错误信息 |

### 可能的使用场景

自动化响应 GitHub 仓库中的 issues，例如感谢贡献者的提交或提供已报告 bug 的状态更新。

---

## Github 创建 Issue

### 功能说明

一个可在 GitHub 仓库中创建新 issues 的功能块。

### 功能描述

该功能块允许用户在指定的 GitHub 仓库中创建带有标题和正文内容的新 issues。

### 工作原理

该模块接收 GitHub 凭据、仓库 URL、议题标题和议题正文作为输入。然后向 GitHub API 发送请求，使用提供的信息创建新议题。

### 输入

| 输入 | 描述 |
|-------|-------------|
| Credentials | GitHub 认证信息 |
| Repo URL | 将在其中创建议题的 GitHub 仓库 URL |
| Title | 新议题的标题 |
| Body | 新议题的主要内容或描述 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| Number | GitHub 分配的议题编号 |
| URL | 新创建议题在 GitHub 上的直接链接 |
| Error | 如果议题创建失败，返回任何错误消息 |

### 可能的使用场景

为通过外部系统或表单提交的错误报告或功能请求自动创建议题。

---

## Github 读取议题

### 功能说明

一个用于检索特定 GitHub 议题信息的模块。

### 功能描述

该模块获取给定 GitHub 议题的详细信息，包括其标题、正文内容以及创建该议题的用户信息。

### 工作原理

该模块接收 GitHub 凭据和议题 URL 作为输入。然后向 GitHub API 发送请求以获取议题的详细信息，并返回相关信息。

### 输入

| 输入 | 描述 |
|-------|-------------|
| Credentials | GitHub 认证信息 |
| Issue URL | 待读取的 GitHub issue 的 URL |

### 输出

| 输出 | 描述 |
|--------|-------------|
| Title | issue 的标题 |
| Body | issue 的主要内容或描述 |
| User | 创建 issue 的用户名 |
| Error | 如果读取 issue 失败，显示任何错误消息 |

### 可能的使用场景

收集已报告 issue 的信息用于分析或在仪表板上显示。

---

## Github 列出 Issues

### 功能说明

一个从 GitHub 仓库检索 issue 列表的块。

### 功能描述

此块从指定的 GitHub 仓库获取所有打开的 issue，并提供它们的标题和 URL。

### 工作原理

该块接收 GitHub 凭据和仓库 URL 作为输入。然后向 GitHub API 发送请求以获取 issue 列表并返回其详细信息。

### 输入

| 输入 | 描述 |
|-------|-------------|
| Credentials | GitHub 认证信息 |
| Repo URL | 要列出 issue 的 GitHub 仓库的 URL |

### 输出

| 输出 | 描述 |
|--------|-------------|
| Issue | 一个 issue 列表，每个包含： |
| - Title | issue 的标题 |
| - URL | 指向 GitHub 上 issue 的直接链接 |
| Error | 如果列出 issue 失败，显示任何错误消息 |

### 可能的用例

为项目状态报告创建未解决问题的摘要，或在项目管理仪表板上显示它们。

---

## Github 添加标签

### 功能说明

一个用于向 GitHub issue 或 pull request 添加标签的功能块。

### 功能描述

该功能块允许用户向现有的 GitHub issue 或 pull request 添加指定标签。

### 工作原理

该功能块接收 GitHub 凭据、issue 或 pull request 的 URL 以及要添加的标签作为输入。然后向 GitHub API 发送请求，将标签添加到指定的 issue 或 pull request。

### 输入参数

| 输入项 | 描述 |
|-------|-------------|
| 凭据 | GitHub 认证信息 |
| Issue URL | 要添加标签的 GitHub issue 或 pull request 的 URL |
| 标签 | 要添加的标签名称 |

### 输出结果

| 输出项 | 描述 |
|--------|-------------|
| 状态 | 指示标签是否成功添加的消息 |
| 错误 | 添加标签失败时的错误信息 |

### 可能的用例

根据内容自动分类问题，或为新创建的问题分配优先级标签。

---

## Github 移除标签

### 功能说明

一个用于从 GitHub issue 或 pull request 移除标签的功能块。

### 功能描述

该功能块允许用户从现有的 GitHub issue 或 pull request 移除指定标签。

### 工作原理

该模块接收 GitHub 凭据、议题或拉取请求的 URL 以及要移除的标签作为输入。然后向 GitHub API 发送请求，从指定的议题或拉取请求中移除标签。

### 输入

| 输入 | 描述 |
|-------|-------------|
| Credentials | GitHub 认证信息 |
| Issue URL | 要移除标签的 GitHub 议题或拉取请求的 URL |
| Label | 要移除的标签名称 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| Status | 指示标签是否成功移除的消息 |
| Error | 移除标签失败时的错误信息 |

### 可能的使用场景

在议题通过工作流程进展时更新其状态，例如在议题完成时移除"进行中"标签。

---

## Github 分配议题

### 功能说明

一个将用户分配到 GitHub 议题的功能模块。

### 功能描述

该模块允许用户将特定的 GitHub 用户分配到现有议题。

### 工作原理

该模块接收 GitHub 凭据、议题的 URL 以及要分配的用户名作为输入。然后向 GitHub API 发送请求，将指定用户分配到该议题。

### 输入

| 输入 | 描述 |
|-------|-------------|
| Credentials | GitHub 认证信息 |
| Issue URL | 待分配 GitHub issue 的 URL |
| Assignee | 待分配到 issue 的用户名 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| Status | 指示 issue 是否成功分配的消息 |
| Error | 分配 issue 失败时的错误信息 |

### 可能的使用场景

根据团队成员的专业知识或工作负载自动将新 issue 分配给他们。

---

## Github 取消分配 Issue

### 功能说明

取消 GitHub issue 中用户的分配。

### 功能描述

该功能块允许用户从现有 issue 中移除特定 GitHub 用户的分配。

### 工作原理

该功能块接收 GitHub 凭据、issue 的 URL 以及待取消分配的用户名作为输入。然后向 GitHub API 发送请求，从 issue 中移除指定用户的分配。

### 输入

| 输入 | 描述 |
|-------|-------------|
| Credentials | GitHub 认证信息 |
| Issue URL | 待取消分配 GitHub issue 的 URL |
| Assignee | 待从 issue 中取消分配的用户名 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| Status | 表示问题是否成功取消分配的状态信息 |
| Error | 取消分配问题失败时的错误信息 |

### 可能的使用场景

自动取消分配已闲置一定时间的问题，或在团队成员间重新分配工作负载时使用。