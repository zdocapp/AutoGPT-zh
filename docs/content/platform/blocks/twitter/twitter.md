# Twitter API 集成模块

## Twitter 发布推文模块

### 功能概述

一个可在 Twitter 上创建推文的模块，支持多种可选附件和设置。

### 功能说明

该模块允许发布包含文本内容的推文，并支持媒体、投票、引用或深度链接等可选附件。

### 工作原理

通过 Twitter API (Tweepy) 创建具有指定内容和设置的推文，同时处理身份验证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_text | 推文的主要文本内容 |
| attachment | 可选的媒体、深度链接、投票、地点或引用附件 |
| for_super_followers_only | 推文是否仅限超级粉丝可见 |
| exclude_reply_user_ids | 要从回复线程中排除的用户 ID |
| in_reply_to_tweet_id | 正在回复的推文 ID |
| reply_settings | 可回复推文的用户范围 |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| tweet_id | 已创建推文的 ID |
| tweet_url | 查看推文的 URL |
| error | 发布失败时的错误信息 |

### 应用场景

自动化发布包含投票、媒体或引用等丰富内容的推文。

---

## Twitter 删除推文模块

### 功能概述

一个用于删除 Twitter 上指定推文的模块。

### 功能说明

此模块通过推文ID删除现有推文。

### 工作原理

它使用Twitter API（Tweepy）删除指定ID的推文，处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需作用域的Twitter API凭据 |
| tweet_id | 要删除的推文ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 删除是否成功 |
| error | 删除失败时的错误信息 |

### 可能的使用场景

自动清理过时或不相关的推文。

---

## Twitter搜索近期推文模块

### 功能定义

一个用于搜索Twitter上近期公开推文的模块。

### 功能说明

该模块根据指定条件搜索推文，并提供筛选和分页选项。

### 工作原理

它使用提供的参数查询Twitter API（Tweepy）搜索端点，并返回匹配的推文和元数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| query | 搜索查询字符串 |
| max_results | 每页最大结果数量 |
| pagination | 用于获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 |
| start_time | 搜索时间窗口开始时间 |
| end_time | 搜索时间窗口结束时间 |
| since_id | 返回此推文 ID 之后的结果 |
| until_id | 返回此推文 ID 之前的结果 |
| sort_order | 返回结果的排序方式 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| tweet_ids | 匹配的推文 ID 列表 |
| tweet_texts | 推文文本内容列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的推文数据 |
| included | 请求的附加数据 |
| meta | 分页和结果元数据 |
| error | 搜索失败时的错误信息 |

### 可能的使用场景

监控 Twitter 上特定话题或标签的提及情况。

---

## Twitter 获取引用推文块

### 功能概述

一个用于从 Twitter 检索引用推文（引用特定推文的推文）的功能块。

### 功能说明

该功能块获取引用指定推文 ID 的推文列表，支持分页和筛选选项。

### 工作原理

它使用 Twitter API (Tweepy) 获取指定推文 ID 的引用推文，处理身份验证并返回带有可选扩展的推文数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭据 |
| tweet_id | 要获取引用推文的推文 ID |
| max_results | 要返回的最大结果数（最多 100） |
| exclude | 要排除的推文类型 |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| ids | 引用推文ID列表 |
| texts | 引用推文文本内容列表 |
| next_token | 用于检索下一页的令牌 [更多信息](twitter.md#common-output)。 |
| data | 完整的推文数据 |
| included | 额外请求的数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

通过引用推文监控特定推文的互动和回应。

---

## Twitter 转推功能块

### 功能说明

一个用于在Twitter上转发现有条推文的功能块。

### 功能作用

该功能块使用推文ID创建指定推文的转发。

### 工作原理

通过Twitter API (Tweepy) 使用给定ID转发推文，处理身份验证和错误情况。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的Twitter API凭据 |
| tweet_id | 要转发的推文ID |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| success | 转发是否成功 |
| error | 转发失败时的错误信息 |

### 可能的使用场景

自动转发符合特定标准的内容。

---

## Twitter 取消转推功能块

### 功能说明

一个用于移除 Twitter 转推的模块。

### 功能说明

该模块移除指定推文的现有转推。

### 工作原理

使用 Twitter API (Tweepy) 通过给定的推文 ID 移除转推，处理身份验证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要移除转推的推文 ID |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| success | 转推移除是否成功 |
| error | 移除失败时的错误信息 |

### 可能的使用场景

基于特定条件自动清理转推。

---

## Twitter 获取转推者模块

### 模块简介

一个用于检索转发了特定推文的用户信息的模块。

### 功能说明

该模块获取转发了指定推文 ID 的用户列表，支持分页和筛选选项。

### 工作原理

使用 Twitter API (Tweepy) 获取给定推文 ID 的转推者信息，处理身份验证并返回带有可选扩展功能的用户数据。

### 输入参数

| Input | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要获取转发者的推文 ID |
| max_results | 每页最大结果数 (1-100) |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| Output | 描述 |
|--------|-------------|
| ids | 转发用户的 ID 列表 |
| names | 转发用户的名称列表 |
| usernames | 转发用户的用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的用户数据 |
| included | 请求的附加数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

通过转发模式监控参与度并分析用户行为。

---

## Twitter 获取用户提及区块

### 功能概述

用于获取提及特定 Twitter 用户的推文的模块。

### 功能说明

该模块通过用户 ID 获取提及该用户的推文。

### 工作原理

通过提供的用户 ID 查询 Twitter API（使用 Tweepy）来获取提及该用户的推文，处理分页和过滤。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| user_id | 要获取提及推文的用户 ID |
| max_results | 每页结果数量（5-100） |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| ids | 推文ID列表 |
| texts | 推文文本内容列表 |
| userIds | 提及目标用户的用户ID列表 |
| userNames | 提及目标用户的用户名列表 |
| next_token | 用于获取下一页的令牌 |
| data | 完整的推文数据 |
| included | 额外请求的数据 [更多信息](twitter.md#common-output) |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output) |
| error | 请求失败时的错误信息 |

### 可能的使用场景

监控特定账号的提及情况，用于社区管理。

---

## Twitter 获取主页时间线模块

### 功能概述

一个用于从用户主页时间线获取推文的模块。

### 功能说明

该模块返回认证用户及其关注账号最近发布的推文和转推集合。

### 工作原理

使用 Twitter API (Tweepy) 从主页时间线获取推文，处理分页并应用过滤器。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| max_results | 每页结果数量 (5-100) |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| ids | 推文 ID 列表 |
| texts | 推文文本内容列表 |
| userIds | 发布推文的用户 ID 列表 |
| userNames | 发布推文的用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的推文数据 |
| included | 请求的附加数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

监控和分析关注账号的内容。

---

## Twitter 获取用户推文块

### 功能说明

一个用于获取特定 Twitter 用户所发布推文的模块。

### 功能说明

该模块返回由单个用户（通过用户 ID 标识）撰写的推文。

### 工作原理

它使用 Twitter API（通过 Tweepy）从指定用户的时间线获取推文，处理分页和过滤。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| user_id | 要获取推文的用户 ID |
| max_results | 每页结果数量（5-100） |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input) |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input) |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input) |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input) |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input) |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input) |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| ids | 推文ID列表 |
| texts | 推文文本内容列表 |
| userIds | 推文作者的用户ID列表 |
| userNames | 推文作者的用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的推文数据 |
| included | 额外请求的数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误消息 |

### 可能的使用场景

分析特定Twitter账号的内容和活动模式。

---

## Twitter 获取推文块

### 功能概述

通过推文ID检索特定推文详细信息的块。

### 功能说明

该块通过推文ID获取指定单条推文的信息，包括推文内容、作者详情和可选的扩展数据。

### 工作原理

使用Twitter API（Tweepy）通过ID获取单条推文，处理身份验证并返回带有可选扩展的推文数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要获取的推文 ID |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input) |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input) |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input) |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input) |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input) |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input) |

### 输出

| 输出 | 描述 |
|--------|-------------|
| id | 推文 ID |
| text | 推文文本内容 |
| userId | 推文作者 ID |
| userName | 推文作者用户名 |
| data | 完整的推文数据 |
| included | 请求的附加数据 [更多信息](twitter.md#common-output) |
| meta | 推文元数据 [更多信息](twitter.md#common-output) |
| error | 请求失败时的错误信息 |

### 可能的使用场景

检索特定推文的详细信息用于分析或监控。

---

## Twitter 获取多条推文块

### 功能概述

通过 ID 检索多条推文信息的块。

### 功能说明

此模块通过推文ID批量获取多条推文信息（最多100条）。

### 工作原理

使用Twitter API（Tweepy）通过ID批量获取推文，处理身份验证并返回带有可选扩展字段的推文数据。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的Twitter API凭证 |
| tweet_ids | 要获取的推文ID列表（最多100条） |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| ids | 推文ID列表 |
| texts | 推文文本内容列表 |
| userIds | 推文作者ID列表 |
| userNames | 推文作者用户名列表 |
| data | 完整的推文数据数组 |
| included | 请求的附加数据 [更多信息](twitter.md#common-output)。 |
| meta | 推文元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

批量获取推文信息用于分析或归档目的。

---

## Twitter 点赞推文模块

### 功能说明

用于在 Twitter 上点赞推文的模块。

### 功能描述

该模块通过指定推文 ID 使用 Twitter API 创建点赞操作。

### 工作原理

使用 Twitter API (Tweepy) 通过给定 ID 点赞推文，处理身份验证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要点赞的推文 ID |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| success | 点赞是否成功 |
| error | 点赞失败时的错误信息 |

### 适用场景

自动点赞符合特定条件的推文。

---

## Twitter 获取点赞用户模块

### 功能说明

用于获取特定推文的点赞用户信息的模块。

### 功能描述

该模块获取指定推文 ID 的点赞用户列表，支持分页和筛选选项。

### 工作原理

使用 Twitter API (Tweepy) 获取给定推文 ID 的点赞用户信息，处理身份验证并返回带有可选扩展功能的用户数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要获取点赞用户的推文 ID |
| max_results | 返回的最大结果数量 (1-100) |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| id | 点赞用户的 ID 列表 |
| username | 点赞用户的用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的用户数据 |
| included | 请求的附加数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

通过跟踪推文点赞分析互动模式。

---

## Twitter 获取已点赞推文块

### 功能说明

一个用于检索特定 Twitter 用户点赞过的推文的块。

### 功能作用

该块获取指定用户 ID 点赞过的推文列表。

### 工作原理

它使用 Twitter API (Tweepy) 获取指定用户 ID 点赞的推文，处理分页和过滤。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| user_id | 要获取点赞推文的用户 ID |
| max_results | 每页最大结果数 (5-100) |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| ids | 点赞推文 ID 列表 |
| texts | 点赞推文文本内容列表 |
| userIds | 推文作者 ID 列表 |
| userNames | 推文作者用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的推文数据 |
| included | 额外请求的数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

通过点赞推文模式分析用户兴趣和偏好。

---

## Twitter 取消点赞推文块

### 功能说明

一个用于取消先前在 Twitter 上点赞的推文的块。

### 功能描述

该块使用推文 ID 移除指定推文的点赞。

### 工作原理

它使用 Twitter API (Tweepy) 通过给定的 ID 取消点赞推文，处理身份验证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要取消点赞的推文 ID |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| success | 取消点赞是否成功 |
| error | 取消点赞失败时的错误信息 |

### 可能的使用场景

基于特定条件自动清理点赞。

---

## Twitter 隐藏回复块

### 功能说明

一个用于隐藏对你推文的回复的块。

### 功能描述

该块隐藏指定的回复推文，使其不在主对话线程中可见。

### 工作原理

它使用 Twitter API (Tweepy) 通过给定的推文 ID 隐藏回复推文，处理身份验证和错误情况。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要隐藏的推文回复 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 隐藏是否成功 |
| error | 隐藏失败时的错误信息 |

### 可能的使用场景

通过隐藏不当或不想要的回复来管理对话。

---

## Twitter 取消隐藏回复块

### 功能说明

一个用于取消隐藏先前隐藏的推文回复的块。

### 功能作用

此块使隐藏的回复推文在对话线程中重新可见。

### 工作原理

它使用 Twitter API (Tweepy) 来取消隐藏具有给定推文 ID 的回复推文，处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| tweet_id | 要取消隐藏的推文回复 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 取消隐藏是否成功 |
| error | 取消隐藏失败时的错误信息 |

### 可能的使用场景

当不再需要审核时恢复先前隐藏的回复。

---

## Twitter 收藏推文块

### 功能说明

一个用于在 Twitter 上收藏指定推文的块。

### 功能作用

此模块通过推文ID为推文创建书签。

### 工作原理

它使用 Twitter API (Tweepy) 通过给定ID为推文添加书签，处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需作用域的 Twitter API 凭据 |
| tweet_id | 要添加书签的推文ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 书签是否添加成功 |
| error | 书签失败时的错误信息 |

### 可能的使用场景

保存推文以供后续参考和组织。

---

## Twitter 获取已收藏推文模块

### 功能说明

一个从 Twitter 获取用户已收藏推文的模块。

### 功能描述

此模块获取经过身份验证用户已收藏的推文列表。

### 工作原理

它使用 Twitter API (Tweepy) 获取已收藏推文，处理分页和可选的数据扩展。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| max_results | 每页最大结果数 (1-100) |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| id | 已收藏推文 ID 列表 |
| text | 已收藏推文文本内容列表 |
| userId | 推文作者 ID 列表 |
| userName | 推文作者用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的推文数据 |
| included | 请求的附加数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

检索和分析已保存的推文，用于内容策划或研究。

---

## Twitter 移除收藏推文块

### 功能说明

一个用于移除 Twitter 推文书签的功能块。

### 功能描述

该功能块通过指定推文 ID 移除已存在的 Twitter 书签。

### 实现原理

通过 Twitter API (Tweepy) 使用给定的推文 ID 移除书签，并处理身份验证和异常情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具备所需权限的 Twitter API 凭证 |
| tweet_id | 需要移除书签的推文 ID |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| success | 书签移除是否成功 |
| error | 移除失败时的错误信息 |

### 应用场景

通过移除过期或不再相关的已保存推文来管理书签。

---

## Twitter 取消屏蔽用户功能块

### 功能说明

一个用于解除 Twitter 平台上已屏蔽用户状态的功能块。

### 功能描述

该功能块通过指定用户 ID 移除对相应用户的屏蔽状态。

### 实现原理

通过 Twitter API (Tweepy) 使用给定的用户 ID 解除用户屏蔽，并处理身份验证和异常情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具备所需权限的 Twitter API 凭证 |
| target_user_id | 需要解除屏蔽的用户 ID |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| success | 解除屏蔽是否成功 |
| error | 解除屏蔽失败时的错误信息 |

### 可能的使用场景

当需要恢复访问权限时，撤销之前被屏蔽的用户。

---

## Twitter 获取被屏蔽用户 屏蔽

### 功能说明

一个用于检索已认证用户所屏蔽用户列表的模块。

### 功能作用

该模块获取被屏蔽用户的信息，支持分页和筛选选项。

### 工作原理

使用 Twitter API (Tweepy) 获取被屏蔽用户列表，处理身份验证并返回带有可选扩展字段的用户数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| max_results | 要返回的最大结果数量 (1-1000) |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的额外数据字段 |
| tweet_fields | 要包含的推文特定字段 |
| user_fields | 要包含的用户相关字段 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| user_ids | 被屏蔽用户ID列表 |
| usernames_ | 被屏蔽用户名列表 |
| included | 请求的额外数据 |
| meta | 分页和结果元数据 |
| next_token | 用于检索下一页的令牌 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

监控和管理被屏蔽用户，用于账户安全和内容审核。

---

## Twitter 屏蔽用户功能块

### 功能说明

一个用于在 Twitter 上屏蔽用户的功能块。

### 功能作用

该功能块使用用户 ID 屏蔽指定用户。

### 工作原理

通过 Twitter API (Tweepy) 使用给定用户 ID 屏蔽用户，处理身份验证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| target_user_id | 要屏蔽的用户 ID |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| success | 屏蔽是否成功 |
| error | 屏蔽失败时的错误信息 |

### 可能的使用场景

基于特定标准或行为自动屏蔽用户。

## Twitter 取消关注用户功能块

### 功能说明

一个用于取消关注 Twitter 用户的功能块。

### 功能作用

该功能块使用用户 ID 取消关注指定用户。

### 工作原理

通过 Twitter API (Tweepy) 使用给定用户 ID 取消关注用户，处理身份验证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| target_user_id | 要取消关注的用户 ID |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| success | 取消关注是否成功 |
| error | 取消关注失败时的错误信息 |

### 可能的使用场景

基于特定条件自动取消关注用户。

---

## Twitter 关注用户功能块

### 功能说明

用于关注 Twitter 用户的功能块。

### 功能作用

该功能块使用用户 ID 来关注指定的 Twitter 用户。

### 工作原理

通过 Twitter API (Tweepy) 使用给定的用户 ID 关注用户，处理身份验证和错误情况。如果目标用户设置了推文保护，此操作将发送关注请求。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| target_user_id | 要关注的用户 ID |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| success | 关注是否成功 |
| error | 关注失败时的错误信息 |

### 可能的使用场景

自动关注符合特定条件的用户。

---

## Twitter 获取粉丝列表功能块

### 功能说明

用于获取指定 Twitter 用户粉丝列表的功能块。

### 功能作用

该功能块获取关注指定用户 ID 的用户列表，支持分页和筛选选项。

### 工作原理

它使用 Twitter API (Tweepy) 获取指定用户 ID 的关注者，处理身份验证并返回带有可选扩展功能的用户数据。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| target_user_id | 要获取关注者的用户 ID |
| max_results | 每页最大结果数 (1-1000) |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| ids | 关注者用户 ID 列表 |
| usernames | 关注者用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的用户数据 |
| includes | 额外请求的数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

分析关注者模式和人口统计数据。

---

## Twitter 获取关注列表功能块

### 功能概述

一个用于检索指定 Twitter 用户所关注用户列表的功能块。

### 功能说明

此模块获取指定用户ID所关注的用户列表，支持分页和筛选选项。

### 工作原理

通过Twitter API（Tweepy）获取给定用户ID的关注列表，处理身份验证并返回带有可选扩展功能的用户数据。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的Twitter API凭证 |
| target_user_id | 需要获取关注列表的用户ID |
| max_results | 每页最大结果数（1-1000） |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 包含的附加数据字段 [更多信息](twitter.md#common-input) |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input) |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input) |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| ids | 关注用户ID列表 |
| usernames | 关注用户名列表 |
| next_token | 检索下一页的令牌 |
| data | 完整的用户数据 |
| includes | 请求的附加数据 [更多信息](twitter.md#common-output) |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output) |
| error | 请求失败时的错误信息 |

### 可能的使用场景

分析关注模式和网络连接关系。

---

## Twitter 取消静音用户模块

### 功能说明

用于解除 Twitter 上先前被禁言的用户。

### 功能描述

该模块通过用户 ID 解除对指定用户的禁言。若目标用户当前未被禁言，请求将成功执行且无实际操作。

### 实现原理

通过 Twitter API (Tweepy) 根据给定用户 ID 解除用户禁言，并处理身份验证及异常情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具备所需权限范围的 Twitter API 凭证 |
| target_user_id | 待解除禁言的用户 ID |

### 输出结果

| 输出项 | 描述 |
|--------|-------------|
| success | 解除禁言是否成功 |
| error | 解除禁言失败时的错误信息 |

### 应用场景

当需要恢复通信时撤销对用户的禁言操作。

---

## Twitter 获取禁言用户列表模块

### 功能说明

用于获取认证用户已禁言的用户列表。

### 功能描述

该模块获取被禁言用户列表，支持分页和筛选选项。

### 实现原理

通过 Twitter API (Tweepy) 获取被禁言用户数据，处理身份验证并返回带有可选扩展信息的用户数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| max_results | 每页最大结果数 (1-1000，默认 10) |
| pagination_token | 用于获取下一页/上一页的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| ids | 已屏蔽用户 ID 列表 |
| usernames | 已屏蔽用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 已屏蔽用户的完整用户数据 |
| includes | 额外请求的数据 |
| meta | 包含分页信息的元数据 |
| error | 请求失败时的错误消息 |

### 可能的使用场景

监控和管理已屏蔽用户列表以进行内容过滤。

---

## Twitter 屏蔽用户功能块

### 功能说明

一个在 Twitter 上屏蔽指定用户的功能块。

### 功能作用

该功能块使用用户 ID 屏蔽用户，以停止看到他们的推文。

### 工作原理

它使用 Twitter API (Tweepy) 通过给定的用户 ID 屏蔽用户，处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| target_user_id | 要禁言的用户 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 禁言是否成功 |
| error | 禁言失败时的错误信息 |

### 可能的使用场景

基于特定标准或行为自动禁言用户。

---

## Twitter 获取用户屏蔽状态

### 功能说明

通过用户 ID 或用户名获取单个 Twitter 用户信息的模块。

### 功能作用

该模块获取指定 Twitter 用户的详细信息，包括基本资料数据和可选的扩展信息。

### 工作原理

使用 Twitter API (Tweepy) 通过 ID 或用户名获取单个用户数据，处理身份验证并返回带有可选扩展功能的用户数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| identifier | 用户标识符（用户 ID 或用户名） |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| id | 用户 ID |
| username_ | Twitter 用户名 |
| name_ | 显示名称 |
| data | 完整的用户数据 |
| included | 额外请求的数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

检索详细的用户档案信息用于分析或验证。

---

## Twitter 获取用户块

### 功能说明

一个通过用户ID或用户名检索多个Twitter用户信息的模块。

### 功能描述

该模块可一次性获取最多100个用户的详细信息，包括基本档案数据和可选的扩展信息。

### 工作原理

使用Twitter API（Tweepy）批量获取通过ID或用户名标识的多个用户数据，处理身份验证并返回带有可选扩展功能的用户数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的Twitter API凭据 |
| identifier | 用户标识符列表（用户ID或用户名，最多100个） |
| expansions | 要包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| ids | 用户ID列表 |
| usernames_ | Twitter用户名列表 |
| names_ | 显示名称列表 |
| data | 完整的用户数据数组 |
| included | 额外请求的数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

批量检索用户档案信息用于分析或监控。

---

## Twitter 搜索空间区块

### 功能说明

一个通过指定搜索词搜索直播或预定Twitter空间的区块。

### 功能描述

该区块基于标题关键词搜索Twitter空间，提供按状态（直播中/已预定）筛选和分页功能。

### 工作原理

使用Twitter API（Tweepy）搜索符合查询参数的空间，处理身份验证并返回带有可选扩展的空间数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| query | 在 Space 标题中搜索的关键词 |
| max_results | 返回的最大结果数量 (1-100, 默认 10) |
| state | 要返回的 Space 类型 (live, scheduled, 或 all) |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input) |
| space_fields | 要包含的 Space 特定字段 [更多信息](twitter.md#common-input) |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input) |

### 输出

| 输出 | 描述 |
|--------|-------------|
| ids | Space ID 列表 |
| titles | Space 标题列表 |
| host_ids | 主持人 ID 列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的 Space 数据 |
| includes | 请求的附加数据 |
| meta | 包含分页信息的元数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

查找相关的 Twitter Spaces 以进行内容发现和互动。

---

## Twitter 获取 Spaces 块

### 功能概述

一个通过 Space ID 或创建者用户 ID 检索多个 Twitter Spaces 信息的块。

### 功能描述

该块使用 Space ID 或创建者用户 ID 获取最多 100 个 Space 的详细信息。

### 工作原理

它使用 Twitter API (Tweepy) 批量获取多个 Space 的数据，处理身份验证并返回带有可选扩展的 Space 数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需作用域的 Twitter API 凭据 |
| identifier | 选择通过 Space ID 或创建者用户 ID 进行查找 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| space_fields | 要包含的 Space 特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| ids | Space ID 列表 |
| titles | Space 标题列表 |
| data | 完整的 Space 数据数组 |
| includes | 请求的附加数据 |
| error | 请求失败时的错误消息 |

### 可能的使用场景

批量检索 Space 信息用于分析或监控。

---

## Twitter 通过 ID 获取 Space 块

### 功能概述

一个用于检索由 Space ID 指定的单个 Twitter Space 信息的块。

### 功能描述

此块获取单个 Space 的详细信息，包括主持人信息和其他元数据。

### 工作原理

它使用 Twitter API (Tweepy) 获取单个 Space ID 的 Space 数据，处理身份验证并返回带有可选扩展的 Space 数据。

### 输入参数

| 输入项 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| space_id | 要获取的 Space ID |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input) |
| space_fields | 要包含的 Space 特定字段 [更多信息](twitter.md#common-input) |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input) |

### 输出结果

| 输出项 | 描述 |
|--------|-------------|
| id | Space ID |
| title | Space 标题 |
| host_ids | 主持人 ID 列表 |
| data | 完整的 Space 数据 |
| includes | 请求的附加数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

获取特定 Space 的详细信息用于分析或展示。

---

## Twitter 获取 Space 购买者模块

### 功能概述

该模块用于检索购买了 Twitter Space 门票的用户列表。

### 功能说明

此模块获取购买了特定 Space 门票的参会用户信息。

### 工作原理

该模块使用 Twitter API (Tweepy) 获取 Space 的购买者信息，处理身份验证并返回带有可选扩展功能的用户数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| space_id | 要获取购买者的 Space ID |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input) |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input) |

### 输出

| 输出 | 描述 |
|--------|-------------|
| buyer_ids | 购买者用户 ID 列表 |
| usernames | 购买者用户名列表 |
| data | 完整的购买者用户数据 |
| includes | 额外请求的数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

分析付费 Spaces 的门票销售和参会者信息。

---

## Twitter 获取 Space 推文块

### 功能概述

一个用于检索特定 Twitter Space 中分享的推文的块。

### 功能说明

该块获取在 Space 会话期间分享的推文。

### 工作原理

它使用 Twitter API (Tweepy) 从 Space 获取推文，处理身份验证并返回带有可选扩展的推文数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| space_id | 要获取推文的 Space ID |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input) |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input) |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input) |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input) |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input) |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input) |

### 输出

| 输出 | 描述 |
|--------|-------------|
| tweet_ids | 推文 ID 列表 |
| texts | 推文文本列表 |
| data | 完整的推文数据 |
| includes | 额外请求的数据 |
| meta | 响应元数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

捕获和分析 Space 会话期间分享的内容。

---

## Twitter 获取列表块

### 功能说明

一个用于检索特定 Twitter 列表详细信息的块。

### 功能描述

该块通过列表 ID 获取 Twitter 列表的信息，包括基本列表数据和可选的扩展信息。

### 工作原理

它使用 Twitter API (Tweepy) 获取单个列表 ID 的列表数据，处理身份验证并返回带有可选扩展的列表数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需作用域的 Twitter API 凭据 |
| list_id | 要检索的 Twitter 列表 ID |
| expansions | 要包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| list_fields | 要包含的列表特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| id | 列表 ID |
| name | 列表名称 |
| owner_id | 列表所有者 ID |
| owner_username | 列表所有者用户名 |
| data | 完整的列表数据 |
| included | 额外请求的数据 |
| meta | 响应元数据 |
| error | 请求失败时的错误消息 |

### 可能的使用场景

检索特定 Twitter 列表的详细信息用于分析或显示。

---

## Twitter 获取拥有的列表块

### 功能说明

一个用于检索指定用户拥有的所有 Twitter 列表的块。

### 功能作用

此块获取由用户 ID 拥有的 Twitter 列表，支持分页和筛选选项。

### 工作原理

它使用 Twitter API (Tweepy) 获取指定用户 ID 拥有的列表，处理身份验证并返回带有可选扩展的列表数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需作用域的 Twitter API 凭据 |
| user_id | 要检索列表的用户 ID |
| max_results | 每页最大结果数 (1-100，默认 10) |
| pagination_token | 获取下一页的令牌 |
| expansions | 要包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| list_fields | 要包含的列表特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| list_ids | 拥有的列表 ID 列表 |
| list_names | 拥有的列表名称列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的列表数据数组 |
| included | 额外请求的数据 |
| meta | 包含分页信息的元数据 |
| error | 请求失败时的错误消息 |

### 可能的使用场景

分析拥有的列表以进行内容策划和受众管理。

---

## Twitter 移除列表成员块

### 功能说明

一个从认证用户拥有的指定 Twitter 列表中移除成员的块。

### 功能描述

此模块用于从 Twitter 列表中移除指定用户（该用户当前为列表成员）。

### 工作原理

它使用 Twitter API (Tweepy) 从指定列表中移除用户，并处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要移除成员的列表 ID |
| user_id | 要从列表中移除的用户 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 移除是否成功 |
| error | 移除失败时的错误信息 |

### 可能的使用场景

通过移除不再符合列表标准的用户来管理列表成员资格。

---

## Twitter 添加列表成员模块

### 功能概述

一个将成员添加到认证用户拥有的指定 Twitter 列表的模块。

### 功能描述

此模块将指定用户作为新成员添加到 Twitter 列表。

### 工作原理

它使用 Twitter API (Tweepy) 将用户添加到指定列表，并处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要添加成员的列表 ID |
| user_id | 要添加到列表的用户 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 添加是否成功 |
| error | 添加失败时的错误信息 |

### 可能的使用场景

通过添加符合列表条件的用户来增长列表成员数量。

---

## Twitter 获取列表成员块

### 功能概述

一个用于检索指定 Twitter 列表所有成员的块。

### 功能描述

该块获取给定列表成员的用户信息，支持分页和筛选选项。

### 工作原理

使用 Twitter API (Tweepy) 获取指定列表的成员数据，处理身份验证，并返回带有可选扩展字段的用户数据。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要获取成员的列表 ID |
| max_results | 每页最大结果数 (1-100，默认 10) |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 要包含的额外数据字段 |
| tweet_fields | 要包含的推文相关字段 |
| user_fields | 要包含的用户相关字段 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| ids | 成员用户ID列表 |
| usernames | 成员用户名列表 |
| next_token | 用于检索下一页的令牌 |
| data | 成员的完整用户数据 |
| included | 额外请求的数据 |
| meta | 分页和结果元数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

分析列表成员资格和成员资料。

---

## Twitter 获取列表成员资格块

### 功能概述

一个用于检索指定用户所属的所有列表的块。

### 功能描述

该块获取指定用户作为成员的列表信息，支持分页和筛选选项。

### 工作原理

使用 Twitter API (Tweepy) 获取给定用户ID的列表成员资格数据，处理身份验证并返回带有可选扩展的列表数据。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭据 |
| user_id | 要获取列表成员资格的用户ID |
| max_results | 每页最大结果数 (1-100，默认10) |
| pagination_token | 用于获取下一页结果的令牌 |
| expansions | 要包含的额外数据字段 |
| list_fields | 要包含的列表特定字段 |
| user_fields | 要包含的用户相关字段 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| list_ids | 列表ID集合 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的列表成员数据 |
| included | 额外请求的数据 |
| meta | 分页元数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

分析用户的列表成员关系，以了解其兴趣和社交联系。

---

## Twitter 获取列表推文块

### 功能概述

一个用于从指定Twitter列表中检索推文的模块。

### 功能说明

该模块获取给定列表内发布的推文，支持分页、筛选和扩展数据选项。

### 工作原理

通过Twitter API（Tweepy）从指定列表获取推文，处理身份验证并返回带有可选扩展功能的推文数据。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要获取推文的列表 ID |
| max_results | 每页最大结果数 (1-100，默认 10) |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 要包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 要包含的位置相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 要包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 要包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出

| 输出 | 描述 |
|--------|-------------|
| tweet_ids | 来自列表的推文 ID 列表 |
| texts | 推文文本内容列表 |
| next_token | 用于检索下一页的令牌 |
| data | 完整的推文数据数组 |
| included | 请求的附加数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 请求失败时的错误消息 |

### 可能的使用场景

监控和分析在精选 Twitter 列表中分享的推文。

---

## Twitter 删除列表块

### 功能说明

一个用于删除认证用户拥有的 Twitter 列表的区块。

### 功能说明

该区块使用列表 ID 删除指定的 Twitter 列表。

### 工作原理

它使用 Twitter API (Tweepy) 删除指定列表，处理认证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要删除的列表 ID |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| success | 删除是否成功 |
| error | 删除失败时的错误信息 |

### 使用场景

移除过时或不必要的 Twitter 列表。

---

## Twitter 更新列表区块

### 功能概述

一个用于更新认证用户拥有的 Twitter 列表的区块。

### 功能说明

该区块修改现有列表的名称和/或描述。

### 工作原理

它使用 Twitter API (Tweepy) 更新列表元数据，处理认证和错误情况。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要更新的列表 ID |
| name | 列表的新名称（可选） |
| description | 列表的新描述（可选） |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| success | 更新是否成功 |
| error | 更新失败时的错误信息 |

### 可能的使用场景

维护列表元数据以反映当前用途或组织方式。

---

## Twitter 创建列表块

### 功能说明

为认证用户创建新 Twitter 列表的功能块。

### 功能描述

该功能块使用指定的名称、描述和隐私设置创建新的 Twitter 列表。

### 工作原理

通过 Twitter API (Tweepy) 创建新列表，处理身份验证并返回列表详细信息。

### 输入参数

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| name | 新列表的名称 |
| description | 列表描述（可选） |
| private | 列表是否应为私有 |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| url | 已创建列表的 URL |
| list_id | 已创建列表的 ID |
| error | 创建失败时的错误信息 |

### 可能的使用场景

创建列表以围绕特定主题或兴趣组织 Twitter 用户。

---

## Twitter 取消固定列表块

### 功能说明

允许用户取消固定指定 Twitter 列表的功能块。

### 功能描述

该功能块从用户的固定列表中移除 Twitter 列表。

### 工作原理

使用 Twitter API (Tweepy) 通过列表 ID 取消固定列表，处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需作用域的 Twitter API 凭据 |
| list_id | 要取消固定的列表 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 取消固定是否成功 |
| error | 取消固定失败时的错误消息 |

### 可能的使用场景

通过移除不再优先的列表来管理已固定的列表。

---

## Twitter 固定列表块

### 功能概述

允许用户固定指定 Twitter 列表的功能块。

### 功能说明

此功能块将 Twitter 列表固定在用户列表的顶部显示。

### 工作原理

使用 Twitter API (Tweepy) 通过列表 ID 固定列表，处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需作用域的 Twitter API 凭据 |
| list_id | 要固定的列表 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 固定是否成功 |
| error | 固定失败时的错误消息 |

### 可能的使用场景

优先处理重要列表以便快速访问。

---

## Twitter 获取已固定列表块

### 功能概述

一个用于获取认证用户已固定的所有 Twitter 列表的模块。

### 功能说明

该模块获取用户已固定的列表集合，提供附加数据和筛选选项。

### 工作原理

使用 Twitter API (Tweepy) 获取已固定列表数据，处理认证并返回带有可选扩展项的列表信息。

### 输入参数

| 输入参数 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| expansions | 要包含的附加数据字段 [更多信息](twitter.md#common-input)。 |
| list_fields | 要包含的列表特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 要包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出结果

| 输出结果 | 描述 |
|--------|-------------|
| list_ids | 已固定列表 ID 列表 |
| list_names | 已固定列表名称列表 |
| data | 完整的列表数据 |
| included | 请求的附加数据 |
| meta | 响应元数据 |
| error | 请求失败时的错误信息 |

### 可能的使用场景

监控和管理已固定列表以实现组织化和快速访问。

---

## Twitter 取消关注列表模块

### 功能概述

一个用于取消关注认证用户当前正在关注的 Twitter 列表的模块。

### 功能说明

此模块使用列表 ID 取消关注指定的 Twitter 列表，将其从用户关注的列表中移除。

### 工作原理

它使用 Twitter API (Tweepy) 通过给定的列表 ID 取消关注列表，并处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要取消关注的列表 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 取消关注是否成功 |
| error | 取消关注失败时的错误消息 |

### 可能的使用场景

通过移除不再相关的列表来管理已关注的列表。

---

## Twitter 关注列表模块

### 功能概述

一个为认证用户关注 Twitter 列表的模块。

### 功能描述

此模块使用列表 ID 关注指定的 Twitter 列表，将其添加到用户关注的列表中。

### 工作原理

它使用 Twitter API (Tweepy) 通过给定的列表 ID 关注列表，并处理身份验证和错误情况。

### 输入

| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需权限范围的 Twitter API 凭据 |
| list_id | 要关注的列表 ID |

### 输出

| 输出 | 描述 |
|--------|-------------|
| success | 关注是否成功 |
| error | 关注失败时的错误消息 |

### 可能的用例

匹配用户兴趣或包含相关内容的下述列表。

---

## 通用输入

Twitter API 允许您在发起请求时选择想要获取的信息类型。以下是您可以请求的不同信息类别：

### expansions

关于被提及或关联的推文、图片和用户的额外信息

| 字段 | 描述 |
|-------|-------------|
| Poll_IDs | 获取推文中所有投票的相关信息，包括投票选项和结果 |
| Media_Keys | 获取推文附带的图片、视频或GIF的详细信息 |
| Author_User_ID | 获取推文作者的信息，例如个人资料详情 |
| Edit_History_Tweet_IDs | 显示推文是否及何时被编辑过，以及具体变更内容 |
| Mentioned_Usernames | 获取所有被@提及用户的个人资料信息 |
| Place_ID | 获取推文中标记位置的详细信息 |
| Reply_To_User_ID | 获取此推文回复对象的相关信息 |
| Referenced_Tweet_ID | 获取此推文引用或转发的任何推文的详细信息 |
| Referenced_Tweet_Author_ID | 获取被引用原始推文作者的个人资料信息 |

### media_fields

关于图片、视频和其他媒体文件的信息

| 字段 | 描述 |
|-------|-------------|
| Duration_in_Milliseconds | 视频或音频片段播放时长（毫秒） |
| Height | 图片或视频的像素高度 |
| Media_Key | 标识该特定媒体文件的唯一代码 |
| Preview_Image_URL | 图片预览缩略图的网络链接 |
| Media_Type | 媒体类型（照片、视频、GIF等） |
| Media_URL | 查看完整媒体的网络链接 |
| Width | 图片或视频的像素宽度 |
| Public_Metrics | 公开可见的数据指标（观看次数、播放次数等） |
| Non_Public_Metrics | 仅推文作者可见的私有数据指标 |
| Organic_Metrics | 自然互动数据指标（非推广内容） |
| Promoted_Metrics | 付费推广效果数据指标 |
| Alternative_Text | 为无障碍访问提供的媒体描述 |
| Media_Variants | 可用的不同尺寸/质量版本（如高清与标清视频） |

### place_fields

推文中提及位置的相关信息

| 字段 | 描述 |
|-------|-------------|
| Contained_Within_Places | 该地点所属的更大区域（如州内的城市） |
| Country | 完整的国家名称 |
| Country_Code | 国家的简短双字母代码（如 US 代表美国） |
| Full_Location_Name | 包含城市、州、国家等的完整名称 |
| Geographic_Coordinates | 地图上的精确位置（纬度和经度） |
| Place_ID | 标识此特定地点的唯一代码 |
| Place_Name | 地点的主要名称（如"时代广场"） |
| Place_Type | 地点类型（城市、企业、地标等） |

### poll_fields

推文中投票的相关信息

| 字段 | 描述 |
|-------|-------------|
| Duration_Minutes | 投票保持开放的时长（分钟） |
| End_DateTime | 投票结束的确切日期和时间 |
| Poll_ID | 标识此特定投票的唯一代码 |
| Poll_Options | 人们可以投票选择的不同选项 |
| Voting_Status | 投票是否仍在开放或已关闭 |

### tweet_fields

推文本身的相关信息

| 字段 | 描述 |
|-------|-------------|
| Tweet_Attachments | 推文中包含的所有媒体、链接或投票 |
| Author_ID | 标识推文作者的唯一代码 |
| Context_Annotations | 关于推文主题的额外信息 |
| Conversation_ID | 连接对话中所有回复的代码 |
| Creation_Time | 推文发布时间 |
| Edit_Controls | 推文是否可编辑及编辑时长限制 |
| Tweet_Entities | 推文中的特殊部分，如#话题标签、@提及和链接 |
| Geographic_Location | 推文发布的地理位置 |
| Tweet_ID | 该特定推文的唯一代码 |
| Reply_To_User_ID | 此推文回复的对象 |
| Language | 推文使用的语言 |
| Public_Metrics | 转发数、点赞数和回复数等统计数据 |
| Sensitive_Content_Flag | 推文可能包含敏感内容的警告标识 |
| Referenced_Tweets | 与此推文相关联的其他推文 |
| Reply_Settings | 允许回复此推文的用户范围 |
| Tweet_Source | 发布推文所使用的应用或网站 |
| Tweet_Text | 推文中的实际文字内容 |
| Withheld_Content | 推文是否在某些国家/地区被隐藏 |

### user_fields

关于Twitter用户的信息

| 字段 | 描述 |
|-------|-------------|
| Account_Creation_Date | 用户加入 Twitter 的时间 |
| User_Bio | 个人资料中的"关于我"文本 |
| User_Entities | 个人资料中的链接和@提及 |
| User_ID | 用户的唯一 Twitter 用户代码 |
| User_Location | 用户声明的所在地 |
| Latest_Tweet_ID | 用户最新推文的代码 |
| Display_Name | 用户的完整个人资料名称（非@用户名） |
| Pinned_Tweet_ID | 固定在个人资料顶部的推文代码 |
| Profile_Picture_URL | 个人资料图片的链接 |
| Is_Protected_Account | 用户的推文是否为私密状态 |
| Account_Statistics | 关注者数、正在关注数和推文数 |
| Profile_URL | 个人资料网页的链接 |
| Username | 用户在 Twitter 上使用的@用户名 |
| Is_Verified | 用户是否拥有验证标记 |
| Verification_Type | 用户拥有的验证类型 |
| Content_Withholding_Info | 用户内容是否在某些地区被隐藏 |

## 额外说明

- 使用扩展和字段的组合来构建精确查询。例如：
  - 要获取包含媒体详情的推文，需包含 `expansions=Media_Keys` 和相关的 `media_fields`。
  - 要在推文中获取用户数据，需添加 `expansions=Author_User_ID` 和适当的 `user_fields`。

- 在 `includes` 下返回的数据有助于使用 ID 将扩展的数据对象与其父实体进行交叉引用。

## 常见输出

Twitter API 在许多端点返回标准化的响应元素。以下是您将遇到的常见输出字段：

### data

响应中请求的主要数据

| 字段 | 描述 |
|-------|-------------|
| ID | 对象的唯一标识符 |
| Type | 对象类型（推文、用户等） |
| Properties | 对象特定字段，如推文的文本内容 |

### includes

主要数据中引用的额外扩展数据对象

| 字段 | 描述 |
|-------|-------------|
| Tweets | 被引用的完整推文对象 |
| Users | 作者/提及用户的个人资料数据 |
| Places | 地理位置标记内容的位置数据 |
| Media | 附加照片/视频的详细信息 |
| Polls | 嵌入式投票的相关信息 |

### meta

关于响应和分页的元数据

| 字段 | 描述 |
|-------|-------------|
| Result_Count | 返回的项目数量 |
| Next_Token | 获取下一页结果的令牌 |
| Previous_Token | 获取上一页的令牌 |
| Newest_ID | 结果中最新的ID |
| Oldest_ID | 结果中最旧的ID |
| Total_Tweet_Count | 匹配推文总数（搜索时） |

### errors

发生的任何错误的详细信息

| 字段 | 描述 |
|-------|-------------|
| Title | 简要错误描述 |
| Detail | 详细错误消息 |
| Type | 错误类别/分类 |
| Status | HTTP状态码 |

### 非分页响应

针对单对象查询：

- data：包含请求的对象
- includes：引用的对象
- errors：遇到的任何错误

### 分页响应

针对多对象查询：

- data：对象数组
- includes：引用的对象
- meta：分页详细信息
- errors：遇到的任何错误