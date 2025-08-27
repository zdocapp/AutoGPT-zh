# AutoGPT 模块概述

AutoGPT 采用模块化方法，使用各种"模块"来处理不同任务。这些模块是 AutoGPT 工作流的构建单元，允许用户通过组合简单、专业化的组件来创建复杂的自动化流程。

以下是所有可用模块的完整列表，按主要功能分类。点击任意模块名称可查看其详细文档。

## 基础操作

| 模块名称 | 描述 |
|------------|-------------|
| [存储值](basic.md#store-value) | 存储并转发一个值 |
| [打印到控制台](basic.md#print-to-console) | 输出文本到控制台用于调试 |
| [字典查找](basic.md#find-in-dictionary) | 在字典或列表中查找值 |
| [代理输入](basic.md#agent-input) | 在工作流中接受用户输入 |
| [代理输出](basic.md#agent-output) | 记录并格式化工作流结果 |
| [添加到字典](basic.md#add-to-dictionary) | 向字典添加新的键值对 |
| [添加到列表](basic.md#add-to-list) | 向列表添加新条目 |
| [注释](basic.md#note) | 在工作流中显示便签注释 |

## 数据处理

| 模块名称 | 描述 |
|------------|-------------|
| [读取 CSV](csv.md#read-csv) | 处理和提取 CSV 文件中的数据 |
| [数据采样](sampling.md#data-sampling) | 使用多种采样方法选择数据子集 |

## 文本处理

| 块名称 | 描述 |
|------------|-------------|
| [Match Text Pattern](text.md#match-text-pattern) | 检查文本是否匹配指定模式 |
| [Extract Text Information](text.md#extract-text-information) | 使用模式从文本中提取特定信息 |
| [Fill Text Template](text.md#fill-text-template) | 使用提供的值填充模板 |
| [Combine Texts](text.md#combine-texts) | 将多个文本输入合并为一个 |
| [Text Decoder](decoder_block.md#text-decoder) | 将编码文本转换为可读格式 |

## AI 与语言模型

| 块名称 | 描述 |
|------------|-------------|
| [AI Structured Response Generator](llm.md#ai-structured-response-generator) | 使用 LLM 生成结构化响应 |
| [AI Text Generator](llm.md#ai-text-generator) | 使用 LLM 生成文本响应 |
| [AI Text Summarizer](llm.md#ai-text-summarizer) | 使用 LLM 总结长文本 |
| [AI Conversation](llm.md#ai-conversation) | 使用 LLM 进行多轮对话 |
| [AI List Generator](llm.md#ai-list-generator) | 使用 LLM 基于提示创建列表 |

## Web 与 API 交互

| 块名称 | 描述 |
|------------|-------------|
| [Send Web Request](http.md#send-web-request) | 向指定网址发起 HTTP 请求 |
| [Read RSS Feed](rss.md#read-rss-feed) | 从 RSS 源检索并处理条目 |
| [Get Weather Information](search.md#get-weather-information) | 获取指定位置的实时天气数据 |
| [Google Maps Search](google_maps.md#google-maps-search) | 使用 Google Maps API 搜索本地商家 |

## 社交媒体与内容

| 块名称 | 描述 |
|------------|-------------|
| [Get Reddit Posts](reddit.md#get-reddit-posts) | 从指定 subreddit 获取帖子 |
| [Post Reddit Comment](reddit.md#post-reddit-comment) | 在 Reddit 上发布评论 |
| [Publish to Medium](medium.md#publish-to-medium) | 直接将内容发布到 Medium |
| [Read Discord Messages](discord.md#read-discord-messages) | 从 Discord 频道检索消息 |
| [Send Discord Message](discord.md#send-discord-message) | 向 Discord 频道发送消息 |

## 搜索与信息检索

| 块名称 | 描述 |
|------------|-------------|
| [Get Wikipedia Summary](search.md#get-wikipedia-summary) | 从维基百科获取主题摘要 |
| [Search The Web](search.md#search-the-web) | 执行网络搜索并返回结果 |
| [Extract Website Content](search.md#extract-website-content) | 从网站检索并提取内容 |

## 时间与日期

| 块名称 | 描述 |
|------------|-------------|
| [获取当前时间](time_blocks.md#get-current-time) | 提供当前时间 |
| [获取当前日期](time_blocks.md#get-current-date) | 提供当前日期 |
| [获取当前日期和时间](time_blocks.md#get-current-date-and-time) | 同时提供当前日期和时间 |
| [倒计时器](time_blocks.md#countdown-timer) | 作为倒计时器使用 |

## 数学与计算

| 块名称 | 描述 |
|------------|-------------|
| [计算器](maths.md#calculator) | 执行基本数学运算 |
| [计数项目](maths.md#count-items) | 统计集合中的项目数量 |

## 媒体生成

| 块名称 | 描述 |
|------------|-------------|
| [Ideogram 模型](ideogram.md#ideogram-model) | 基于文本提示生成图像 |
| [创建说话头像视频](talking_head.md#create-talking-avatar-video) | 创建带有说话头像的视频 |
| [Unreal 文本转语音](text_to_speech_block.md#unreal-text-to-speech) | 使用 Unreal Speech API 将文本转换为语音 |
| [AI 短视频创作器](ai_shortform_video_block.md#ai-shortform-video-creator) | 使用 AI 生成短视频 |
| [Replicate Flux 高级模型](replicate_flux_advanced.md#replicate-flux-advanced-model) | 使用 Replicate 的 Flux 模型创建图像 |
| [Flux Kontext](flux_kontext.md#flux-kontext) | 使用 Flux Kontext 进行基于文本的图像编辑 |

## 其他资源

| 块名称 | 描述 |
|------------|-------------|
| [Transcribe YouTube Video](youtube.md#transcribe-youtube-video) | 转录 YouTube 视频中的音频 |
| [Send Email](email_block.md#send-email) | 使用 SMTP 发送电子邮件 |
| [Condition Block](branching.md#condition-block) | 评估工作流分支的条件 |
| [Step Through Items](iteration.md#step-through-items) | 遍历列表或字典 |

## Google 服务

| 模块名称 | 描述 |
|------------|-------------|
| [Gmail Read](google/gmail.md#gmail-read) | 从 Gmail 账户检索并读取邮件 |
| [Gmail Get Thread](google/gmail.md#gmail-get-thread) | 返回 Gmail 会话中的所有消息 |
| [Gmail Reply](google/gmail.md#gmail-reply) | 发送保持在相同会话中的回复 |
| [Gmail Send](google/gmail.md#gmail-send) | 使用 Gmail 账户发送邮件 |
| [Gmail List Labels](google/gmail.md#gmail-list-labels) | 从 Gmail 账户检索所有标签 |
| [Gmail Add Label](google/gmail.md#gmail-add-label) | 为 Gmail 账户中的特定邮件添加标签 |
| [Gmail Remove Label](google/gmail.md#gmail-remove-label) | 从 Gmail 账户中的特定邮件移除标签 |
| [Google Sheets Read](google/sheet.md#google-sheets-read) | 从 Google Sheets 电子表格读取数据 |
| [Google Sheets Write](google/sheet.md#google-sheets-write) | 向 Google Sheets 电子表格写入数据 |
| [Google Maps Search](google_maps.md#google-maps-search) | 使用 Google Maps API 搜索本地商家 |

## GitHub集成

| 块名称 | 描述 |
|------------|-------------|
| [GitHub Comment](github/issues.md#github-comment) | 在 GitHub 问题或拉取请求上发布评论 |
| [GitHub Make Issue](github/issues.md#github-make-issue) | 在 GitHub 仓库中创建新问题 |
| [GitHub Read Issue](github/issues.md#github-read-issue) | 检索特定 GitHub 问题的信息 |
| [GitHub List Issues](github/issues.md#github-list-issues) | 从 GitHub 仓库检索问题列表 |
| [GitHub Add Label](github/issues.md#github-add-label) | 向 GitHub 问题或拉取请求添加标签 |
| [GitHub Remove Label](github/issues.md#github-remove-label) | 从 GitHub 问题或拉取请求移除标签 |
| [GitHub Assign Issue](github/issues.md#github-assign-issue) | 将用户分配给 GitHub 问题 |
| [GitHub List Tags](github/repo.md#github-list-tags) | 检索并列出指定 GitHub 仓库的所有标签 |
| [GitHub List Branches](github/repo.md#github-list-branches) | 检索并列出指定 GitHub 仓库的所有分支 |
| [GitHub List Discussions](github/repo.md#github-list-discussions) | 检索并列出指定 GitHub 仓库的最近讨论 |
| [GitHub Make Branch](github/repo.md#github-make-branch) | 在 GitHub 仓库中创建新分支 |
| [GitHub Delete Branch](github/repo.md#github-delete-branch) | 从 GitHub 仓库删除指定分支 |
| [GitHub List Pull Requests](github/pull_requests.md#github-list-pull-requests) | 从指定 GitHub 仓库检索拉取请求列表 |
| [GitHub Make Pull Request](github/pull_requests.md#github-make-pull-request) | 在指定 GitHub 仓库中创建新拉取请求 |
| [GitHub Read Pull Request](github/pull_requests.md#github-read-pull-request) | 检索特定 GitHub 拉取请求的详细信息 |
| [GitHub Assign PR Reviewer](github/pull_requests.md#github-assign-pr-reviewer) | 为特定 GitHub 拉取请求分配审阅者 |
| [GitHub Unassign PR Reviewer](github/pull_requests.md#github-unassign-pr-reviewer) | 从特定 GitHub 拉取请求移除分配的审阅者 |
| [GitHub List PR Reviewers](github/pull_requests.md#github-list-pr-reviewers) | 检索特定 GitHub 拉取请求的所有分配审阅者列表 |

## Twitter 集成

| 区块名称 | 描述 |
|------------|-------------|
| [Twitter 发布推文](twitter/twitter.md#twitter-post-tweet-block) | 在 Twitter 上创建包含文本内容和可选附件（包括媒体、投票、引用或深度链接）的推文 |
| [Twitter 删除推文](twitter/twitter.md#twitter-delete-tweet-block) | 使用推文 ID 删除指定的推文 |
| [Twitter 搜索近期推文](twitter/twitter.md#twitter-search-recent-tweets-block) | 搜索符合指定条件的推文，提供筛选和分页选项 |
| [Twitter 获取引用推文](twitter/twitter.md#twitter-get-quote-tweets-block) | 获取引用指定推文 ID 的推文，提供分页和筛选选项 |
| [Twitter 转推](twitter/twitter.md#twitter-retweet-block) | 使用推文 ID 创建指定推文的转推 |
| [Twitter 移除转推](twitter/twitter.md#twitter-remove-retweet-block) | 移除指定推文的现有转推 |
| [Twitter 获取转推用户](twitter/twitter.md#twitter-get-retweeters-block) | 获取转推了指定推文的用户列表，提供分页和筛选选项 |
| [Twitter 获取用户提及](twitter/twitter.md#twitter-get-user-mentions-block) | 获取提及特定用户的推文，使用用户 ID |
| [Twitter 获取主页时间线](twitter/twitter.md#twitter-get-home-timeline-block) | 获取认证用户和关注账户的近期推文和转推 |
| [Twitter 获取用户](twitter/twitter.md#twitter-get-user-block) | 获取单个 Twitter 用户的详细资料信息 |
| [Twitter 获取多个用户](twitter/twitter.md#twitter-get-users-block) | 获取多个 Twitter 用户的资料信息（最多 100 个） |
| [Twitter 搜索空间](twitter/twitter.md#twitter-search-spaces-block) | 搜索符合标题关键词的 Twitter 空间，提供状态筛选 |
| [Twitter 获取空间](twitter/twitter.md#twitter-get-spaces-block) | 通过空间 ID 或创建者 ID 获取多个 Twitter 空间的信息 |
| [Twitter 按 ID 获取空间](twitter/twitter.md#twitter-get-space-by-id-block) | 获取单个 Twitter 空间的详细信息 |
| [Twitter 获取空间推文](twitter/twitter.md#twitter-get-space-tweets-block) | 获取在 Twitter 空间会话期间分享的推文 |
| [Twitter 关注列表](twitter/twitter.md#twitter-follow-list-block) | 使用列表 ID 关注 Twitter 列表 |
| [Twitter 取消关注列表](twitter/twitter.md#twitter-unfollow-list-block) | 取消关注先前关注的 Twitter 列表 |
| [Twitter 获取列表](twitter/twitter.md#twitter-get-list-block) | 获取特定 Twitter 列表的详细信息 |
| [Twitter 获取拥有的列表](twitter/twitter.md#twitter-get-owned-lists-block) | 获取指定用户拥有的所有 Twitter 列表 |
| [Twitter 获取列表成员](twitter/twitter.md#twitter-get-list-members-block) | 获取指定 Twitter 列表成员的信息 |
| [Twitter 添加列表成员](twitter/twitter.md#twitter-add-list-member-block) | 将指定用户添加为 Twitter 列表的成员 |
| [Twitter 移除列表成员](twitter/twitter.md#twitter-remove-list-member-block) | 从 Twitter 列表中移除指定用户 |
| [Twitter 获取列表推文](twitter/twitter.md#twitter-get-list-tweets-block) | 获取在指定 Twitter 列表中发布的推文 |
| [Twitter 创建列表](twitter/twitter.md#twitter-create-list-block) | 使用指定名称和设置创建新的 Twitter 列表 |
| [Twitter 更新列表](twitter/twitter.md#twitter-update-list-block) | 更新现有 Twitter 列表的名称和/或描述 |
| [Twitter 删除列表](twitter/twitter.md#twitter-delete-list-block) | 删除指定的 Twitter 列表 |
| [Twitter 置顶列表](twitter/twitter.md#twitter-pin-list-block) | 将 Twitter 列表置顶显示在列表顶部 |
| [Twitter 取消置顶列表](twitter/twitter.md#twitter-unpin-list-block) | 从置顶列表中移除 Twitter 列表 |
| [Twitter 获取置顶列表](twitter/twitter.md#twitter-get-pinned-lists-block) | 获取当前所有置顶的 Twitter 列表 |
| Twitter 列表获取关注者 | 开发中... 获取指定 Twitter 列表的所有关注者 |
| Twitter 获取已关注列表 | 开发中... 获取用户关注的所有列表 |
| Twitter 获取私信事件 | 开发中... 检索用户的私信事件 |
| Twitter 发送私信 | 开发中... 向指定用户发送私信 |
| Twitter 创建私信会话 | 开发中... 创建新的私信会话 |

## Todoist 集成

| 块名称 | 描述 |
|------------|-------------|
| [Todoist Create Label](todoist.md#todoist-create-label) | 在 Todoist 中创建新标签 |
| [Todoist List Labels](todoist.md#todoist-list-labels) | 从 Todoist 检索所有个人标签 |
| [Todoist Get Label](todoist.md#todoist-get-label) | 通过 ID 检索特定标签 |
| [Todoist Create Task](todoist.md#todoist-create-task) | 在 Todoist 中创建新任务 |
| [Todoist Get Tasks](todoist.md#todoist-get-tasks) | 从 Todoist 检索活动任务 |
| [Todoist Update Task](todoist.md#todoist-update-task) | 更新现有任务 |
| [Todoist Close Task](todoist.md#todoist-close-task) | 完成/关闭任务 |
| [Todoist Reopen Task](todoist.md#todoist-reopen-task) | 重新打开已完成的任务 |
| [Todoist Delete Task](todoist.md#todoist-delete-task) | 永久删除任务 |
| [Todoist List Projects](todoist.md#todoist-list-projects) | 从 Todoist 检索所有项目 |
| [Todoist Create Project](todoist.md#todoist-create-project) | 在 Todoist 中创建新项目 |
| [Todoist Get Project](todoist.md#todoist-get-project) | 检索特定项目的详细信息 |
| [Todoist Update Project](todoist.md#todoist-update-project) | 更新现有项目 |
| [Todoist Delete Project](todoist.md#todoist-delete-project) | 删除项目及其内容 |
| [Todoist List Collaborators](todoist.md#todoist-list-collaborators) | 检索项目上的协作者 |
| [Todoist List Sections](todoist.md#todoist-list-sections) | 从 Todoist 检索分区 |
| [Todoist Get Section](todoist.md#todoist-get-section) | 检索特定分区的详细信息 |
| [Todoist Delete Section](todoist.md#todoist-delete-section) | 删除分区及其任务 |
| [Todoist Create Comment](todoist.md#todoist-create-comment) | 在任务或项目上创建新评论 |
| [Todoist Get Comments](todoist.md#todoist-get-comments) | 检索任务或项目的所有评论 |
| [Todoist Get Comment](todoist.md#todoist-get-comment) | 通过 ID 检索特定评论 |
| [Todoist Update Comment](todoist.md#todoist-update-comment) | 更新现有评论 |
| [Todoist Delete Comment](todoist.md#todoist-delete-comment) | 删除评论 |

这份详尽列表涵盖了 AutoGPT 中所有可用的功能模块。每个模块都设计用于执行特定任务，它们可以组合起来创建强大的自动化工作流。如需获取每个模块的详细信息，请点击其名称查看完整文档。