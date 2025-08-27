## 🔍 Google API 密钥配置

!!! note
    此部分为可选配置。当搜索尝试返回错误 429 时，请使用官方 Google API。要使用 `google` 命令，您需要在环境变量中设置 Google API 密钥，或通过配置将其传递给 [`WebSearchComponent`](../../forge/components/built-in-components.md)。

创建您的项目：

1. 前往 [Google Cloud Console](https://console.cloud.google.com/)
1. 如果您还没有账户，请创建一个并登录
1. 通过点击页面顶部的*选择项目*下拉菜单并点击*新建项目*来创建新项目
1. 为其命名并点击*创建*
1. 设置自定义搜索 API 并添加到您的 .env 文件中：
    1. 前往 [APIs & Services 仪表板](https://console.cloud.google.com/apis/dashboard)
    1. 点击*启用 API 和服务*
    1. 搜索*Custom Search API*并点击它
    1. 点击*启用*
    1. 前往[凭据](https://console.cloud.google.com/apis/credentials)页面
    1. 点击*创建凭据*
    1. 选择*API 密钥*
    1. 复制 API 密钥
    1. 将其设置为 `.env` 文件中的 `GOOGLE_API_KEY`
1. 在您的项目上[启用](https://console.developers.google.com/apis/api/customsearch.googleapis.com)
    Custom Search API（可能需要等待几分钟才能生效）
    设置自定义搜索引擎并添加到您的 .env 文件中：
    1. 前往[自定义搜索引擎](https://cse.google.com/cse/all)页面
    1. 点击*添加*
    1. 按照提示设置您的搜索引擎
        您可以选择搜索整个网络或特定网站
    1. 创建搜索引擎后，点击*控制面板*
    1. 点击*基础*
    1. 复制*搜索引擎 ID*
    1. 将其设置为 `.env` 文件中的 `CUSTOM_SEARCH_ENGINE_ID`

请注意，您的免费每日自定义搜索配额仅允许最多100次搜索。如需提高此限制，您需要为项目分配一个结算账户，以便享受每日最多10,000次搜索的服务。