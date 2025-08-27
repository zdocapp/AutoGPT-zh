# 发布到 Medium

## 功能概述

发布到 Medium 模块是一个工具，能够在自动化工作流中直接将内容发布到 Medium 平台。

## 功能说明

该模块接收完全格式化的博客文章及相关元数据，并通过平台的 API 将其发布到 Medium。它处理发布过程的所有方面，包括设置标题、内容、标签和其他文章特定细节。

## 工作原理

该模块使用提供的 Medium API 密钥和作者 ID 与 Medium 平台进行身份验证。然后构建包含所有文章详情的 API 请求，并将其发送到 Medium 的服务器。文章发布后，该模块会检索并返回有关新创建文章的相关信息，例如其唯一 ID 和公共 URL。

## 输入参数

| 输入 | 描述 |
|-------|-------------|
| Author ID | Medium 作者账户的唯一标识符 |
| Title | Medium 文章的标题 |
| Content | 文章正文（HTML 或 Markdown 格式） |
| Content Format | 指定内容格式为 'html' 或 'markdown' |
| Tags | 用于分类文章的主题标签（最多5个，逗号分隔） |
| Canonical URL | 若内容首发于其他平台，此处填写原始 URL |
| Publish Status | 设置文章可见性：'public'（公开）、'draft'（草稿）或 'unlisted'（未列出） |
| License | 文章版权许可（默认：'all-rights-reserved'） |
| Notify Followers | 布尔标志，是否通知作者关注者有新文章发布 |
| API Key | 用于身份验证的 Medium API 密钥 |

## 输出结果

| 输出 | 描述 |
|--------|-------------|
| Post ID | Medium 为已发布文章分配的唯一标识符 |
| Post URL | 可查看文章的公开网址 |
| Published At | 文章发布的时间戳 |
| Error | 发布过程失败时返回的错误信息 |

## 可能的使用场景

数字营销团队可将此模块集成至其内容管理系统，以简化跨平台发布策略。在主系统中创建并审核博客文章后，他们可利用此模块自动将内容发布至Medium平台，确保在多渠道实现内容同步且及时的自动化分发，无需人工干预。