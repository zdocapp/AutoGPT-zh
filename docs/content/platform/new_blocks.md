# 为 AutoGPT Agent Server 贡献：创建与测试模块

本指南将以 WikipediaSummaryBlock 为例，引导您完成为 AutoGPT Agent Server 创建和测试新模块的全过程。

## 理解模块与测试

模块是可复用的组件，可以连接起来形成表示智能体行为的图。每个模块都有输入、输出和特定功能。充分的测试对于确保模块正确且一致地工作至关重要。

## 创建与测试新模块

请遵循以下步骤来创建和测试一个新模块：

1. **创建一个新的 Python 文件**用于你的块，放在 `autogpt_platform/backend/backend/blocks` 目录中。使用描述性名称并采用蛇形命名法。例如：`get_wikipedia_summary.py`。

2. **导入必要的模块并创建一个继承自 `Block` 的类**。确保包含你的块所需的所有导入。

    每个块应包含以下内容：

    ```python
    from backend.data.block import Block, BlockSchema, BlockOutput
    ```

    Wikipedia 摘要块的示例：

    ```python
    from backend.data.block import Block, BlockSchema, BlockOutput
    from backend.utils.get_request import GetRequest
    import requests

    class WikipediaSummaryBlock(Block, GetRequest):
        # 块的实现将放在这里
    ```

3. **使用 `BlockSchema` 定义输入和输出模式**。这些模式指定了块期望接收（输入）和产生（输出）的数据结构。

   - 输入模式定义了块将处理的数据结构。模式中的每个字段代表一个必需的输入数据片段。
   - 输出模式定义了块在处理后将返回的数据结构。模式中的每个字段代表一个输出数据片段。

    示例：

    ```python
    class Input(BlockSchema):
        topic: str  # 要获取 Wikipedia 摘要的主题

    class Output(BlockSchema):
        summary: str  # 来自 Wikipedia 的主题摘要
        error: str  # 如果请求失败时的任何错误消息，错误字段需要命名为 `error`
    ```

4. **实现 `__init__` 方法，包括测试数据和模拟：**

    !!! important
         为每个新块的 `id` 使用 UUID 生成器（例如 https://www.uuidgenerator.net/），*不要*自己编造。或者，你可以运行这段 Python 代码来生成 uuid：`print(__import__('uuid').uuid4())`

    ```python
    def __init__(self):
        super().__init__(
            # 块的唯一 ID，跨用户用于模板
            # 如果你是 AI，请保持原样或更改为 "generate-proper-uuid"
            id="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            input_schema=WikipediaSummaryBlock.Input,  # 分配输入模式
            output_schema=WikipediaSummaryBlock.Output,  # 分配输出模式

                # 提供用于测试块的示例输入、输出和测试模拟

            test_input={"topic": "Artificial Intelligence"},
            test_output=("summary", "summary content"),
            test_mock={"get_request": lambda url, json: {"extract": "summary content"}},
        )
    ```

    - `id`：块的唯一标识符。

    - `input_schema` 和 `output_schema`：定义输入和输出数据的结构。

    让我们分解测试组件：

    - `test_input`：这是用于测试块的示例输入。它应该是根据你的输入模式的有效输入。

    - `test_output`：这是使用 `test_input` 运行块时的预期输出。它应该与你的输出模式匹配。对于非确定性输出或当你只想断言类型时，可以使用 Python 类型而不是特定值。在此示例中，`("summary", str)` 断言输出键为 "summary" 且其值为字符串。

    - `test_mock`：这对于进行网络调用的块至关重要。它提供了一个模拟函数，在测试期间替换实际的网络调用。

     在这种情况下，我们模拟 `get_request` 方法始终返回一个带有 'extract' 键的字典，模拟成功的 API 响应。这使我们能够测试块的逻辑而无需进行实际的网络请求，这些请求可能缓慢、不可靠或受到速率限制。

5. **实现带有错误处理的 `run` 方法**。这应包含块的主要逻辑：

   ```python
   def run(self, input_data: Input, **kwargs) -> BlockOutput:
       try:
           topic = input_data.topic
           url = f"https://en.wikipedia.org/api/rest_v1/page/summary/{topic}"

           response = self.get_request(url, json=True)
           yield "summary", response['extract']

       except requests.exceptions.HTTPError as http_err:
           raise RuntimeError(f"HTTP error occurred: {http_err}")
   ```

   - **Try 块**：包含获取和处理 Wikipedia 摘要的主要逻辑。
   - **API 请求**：向 Wikipedia API 发送 GET 请求。
   - **错误处理**：处理在 API 请求和数据处理过程中可能发生的各种异常。我们不需要捕获所有异常，只捕获我们预期并能处理的异常。未捕获的异常将自动作为 `error` 在输出中产生。任何引发异常（或产生 `error` 输出）的块将被标记为失败。优先选择引发异常而不是产生 `error`，因为它会立即停止执行。
   - **Yield**：使用 `yield` 输出结果。优先一次输出一个结果对象。如果你调用一个返回列表的函数，可以分别产生列表中的每个项目。你也可以将整个列表作为一个单独的结果对象产生，但两种方式都要做，而不是只产生列表。例如：如果你正在编写一个输出电子邮件的块，你会将每个电子邮件作为单独的结果对象产生，但也可以将整个列表作为一个额外的单一结果对象产生。产生名为 `error` 的输出将立即中断执行并将块执行标记为失败。
   - **kwargs**：`kwargs` 参数用于向块传递额外的参数。在上面的示例中未使用，但它对块可用。你也可以在 run 方法中使用内联签名参数，如 `def run(self, input_data: Input, *, user_id: str, **kwargs) -> BlockOutput:`。
       可用的 kwargs 包括：
       - `user_id`：运行块的用户 ID。
       - `graph_id`：正在执行块的代理 ID。对于代理的每个版本都是相同的
       - `graph_exec_id`：代理执行的 ID。每次代理有新的 "运行" 时都会改变
       - `node_exec_id`：节点执行的 ID。每次执行节点时都会改变
       - `node_id`：正在执行的节点的 ID。它在图的每个版本中都会改变，但不是每次执行节点时都改变。

### 字段类型

#### oneOf 字段

`oneOf` 允许您指定字段必须是多个可能选项中的恰好一个。当您希望区块接受互斥的不同类型输入时，这非常有用。

示例：

```python
attachment: Union[Media, DeepLink, Poll, Place, Quote] = SchemaField(
    discriminator='discriminator',
    description="Attach either media, deep link, poll, place or quote - only one can be used"
)
```

`discriminator` 参数告诉 AutoGPT 在输入中查看哪个字段以确定其类型。

在每个模型中，您需要定义鉴别器值：

```python
class Media(BaseModel):
    discriminator: Literal['media']
    media_ids: List[str]

class DeepLink(BaseModel):
    discriminator: Literal['deep_link']
    direct_message_deep_link: str
```

#### OptionalOneOf 字段

`OptionalOneOf` 类似于 `oneOf`，但允许字段为可选（None）。这意味着字段可以是指定类型之一，也可以是 None。

示例：

```python
attachment: Union[Media, DeepLink, Poll, Place, Quote] | None = SchemaField(
    discriminator='discriminator',
    description="Optional attachment - can be media, deep link, poll, place, quote or None"
)
```

关键区别在于 `| None`，它使整个字段变为可选。

### 带认证的区块

我们的系统支持 API 密钥和 OAuth2 授权流程的认证卸载。
添加带有 API 密钥认证的区块非常简单，为我们已支持 OAuth2 的服务添加区块也同样简单。

实现区块本身相对简单。除了上述说明外，您还需要向 `Input` 模型和 `run` 方法添加一个 `credentials` 参数：

```python
from backend.data.model import (
    APIKeyCredentials,
    OAuth2Credentials,
    Credentials,
)

from backend.data.block import Block, BlockOutput, BlockSchema
from backend.data.model import CredentialsField
from backend.integrations.providers import ProviderName


# API Key auth:
class BlockWithAPIKeyAuth(Block):
    class Input(BlockSchema):
        # Note that the type hint below is require or you will get a type error.
        # The first argument is the provider name, the second is the credential type.
        credentials: CredentialsMetaInput[
            Literal[ProviderName.GITHUB], Literal["api_key"]
        ] = CredentialsField(
            description="The GitHub integration can be used with "
            "any API key with sufficient permissions for the blocks it is used on.",
        )

    # ...

    def run(
        self,
        input_data: Input,
        *,
        credentials: APIKeyCredentials,
        **kwargs,
    ) -> BlockOutput:
        ...

# OAuth:
class BlockWithOAuth(Block):
    class Input(BlockSchema):
        # Note that the type hint below is require or you will get a type error.
        # The first argument is the provider name, the second is the credential type.
        credentials: CredentialsMetaInput[
            Literal[ProviderName.GITHUB], Literal["oauth2"]
        ] = CredentialsField(
            required_scopes={"repo"},
            description="The GitHub integration can be used with OAuth.",
        )

    # ...

    def run(
        self,
        input_data: Input,
        *,
        credentials: OAuth2Credentials,
        **kwargs,
    ) -> BlockOutput:
        ...

# API Key auth + OAuth:
class BlockWithAPIKeyAndOAuth(Block):
    class Input(BlockSchema):
        # Note that the type hint below is require or you will get a type error.
        # The first argument is the provider name, the second is the credential type.
        credentials: CredentialsMetaInput[
            Literal[ProviderName.GITHUB], Literal["api_key", "oauth2"]
        ] = CredentialsField(
            required_scopes={"repo"},
            description="The GitHub integration can be used with OAuth, "
            "or any API key with sufficient permissions for the blocks it is used on.",
        )

    # ...

    def run(
        self,
        input_data: Input,
        *,
        credentials: Credentials,
        **kwargs,
    ) -> BlockOutput:
        ...
```

凭证将由后端执行器自动注入。

`APIKeyCredentials` 和 `OAuth2Credentials` 模型定义在[此处](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/autogpt_libs/autogpt_libs/supabase_integration_credentials_store/types.py)。
要在例如 API 请求中使用它们，您可以直接访问令牌：

```python
# credentials: APIKeyCredentials
response = requests.post(
    url,
    headers={
        "Authorization": f"Bearer {credentials.api_key.get_secret_value()})",
    },
)

# credentials: OAuth2Credentials
response = requests.post(
    url,
    headers={
        "Authorization": f"Bearer {credentials.access_token.get_secret_value()})",
    },
)
```

或使用快捷方式 `credentials.auth_header()`：

```python
# credentials: APIKeyCredentials | OAuth2Credentials
response = requests.post(
    url,
    headers={"Authorization": credentials.auth_header()},
)
```

`ProviderName` 枚举是我们系统中存在哪些提供者的单一事实来源。
自然地，要为新的提供者添加认证块，您也需要在此处添加它。

<details>
<summary><code>ProviderName</code> 定义</summary>

```python title="backend/integrations/providers.py"
--8<-- "autogpt_platform/backend/backend/integrations/providers.py:ProviderName"
```

</details>

#### 多个凭据输入

支持多个凭据输入，但需满足以下条件：

- 每个凭据输入字段的名称必须以 `_credentials` 结尾。
- 凭据输入字段的名称必须与块 `run(..)` 方法上相应参数的名称匹配。
- 如果需要多个凭据参数，`test_credentials` 是一个 `dict[str, Credentials]`，其中每个必需的凭据输入都以参数名作为键，并使用合适的测试凭据作为值。

#### 添加 OAuth2 服务集成

要添加对新的 OAuth2 认证服务的支持，您需要添加一个 `OAuthHandler`。
我们所有现有的处理程序和基类都可以在[此处][OAuth2 handlers]找到。

每个处理程序都必须实现 [`BaseOAuthHandler`] 接口的以下部分：

```python title="backend/integrations/oauth/base.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler1"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler2"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler3"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler4"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler5"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler6"
```

如您所见，这是按照标准 OAuth2 流程建模的。

除了实现 `OAuthHandler` 本身之外，将处理程序添加到系统中还需要另外两件事：

- 将处理程序类添加到 [`integrations/oauth/__init__.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/__init__.py) 中的 `HANDLERS_BY_NAME`

```python title="backend/integrations/oauth/__init__.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/__init__.py:HANDLERS_BY_NAMEExample"
```

- 将 `{provider}_client_id` 和 `{provider}_client_secret` 添加到 [`util/settings.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/util/settings.py) 中应用程序的 `Secrets` 中

```python title="backend/util/settings.py"
--8<-- "autogpt_platform/backend/backend/util/settings.py:OAuthServerCredentialsExample"
```

[OAuth2 handlers]: https://github.com/Significant-Gravitas/AutoGPT/tree/master/autogpt_platform/backend/backend/integrations/oauth

[`BaseOAuthHandler`]: https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/base.py

#### 添加到前端

您需要将提供程序（api 或 oauth）添加到 [`frontend/src/components/integrations/credentials-input.tsx`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/frontend/src/components/integrations/credentials-input.tsx) 中的 `CredentialsInput` 组件。

```ts title="frontend/src/components/integrations/credentials-input.tsx"
--8<-- "autogpt_platform/frontend/src/components/integrations/credentials-input.tsx:ProviderIconsEmbed"
```

您还需要将提供程序添加到 [`frontend/src/components/integrations/helper.ts`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/frontend/src/components/integrations/helper.ts) 中的凭据提供程序列表中。

```ts title="frontend/src/components/integrations/helper.ts"
--8<-- "autogpt_platform/frontend/src/components/integrations/helper.ts:CredentialsProviderNames"
```

最后，您需要将提供者添加到 [`frontend/src/lib/autogpt-server-api/types.ts`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/frontend/src/lib/autogpt-server-api/types.ts) 中的 `CredentialsType` 枚举。

```ts title="frontend/src/lib/autogpt-server-api/types.ts"
--8<-- "autogpt_platform/frontend/src/lib/autogpt-server-api/types.ts:BlockIOCredentialsSubSchema"
```

#### 示例：GitHub 集成

- 支持 API 密钥 + OAuth2 的 GitHub 模块：[`blocks/github`](https://github.com/Significant-Gravitas/AutoGPT/tree/master/autogpt_platform/backend/backend/blocks/github/)

```python title="backend/blocks/github/issues.py"
--8<-- "autogpt_platform/backend/backend/blocks/github/issues.py:GithubCommentBlockExample"
```

- GitHub OAuth2 处理器：[`integrations/oauth/github.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/github.py)

```python title="backend/integrations/oauth/github.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/github.py:GithubOAuthHandlerExample"
```

#### 示例：Google 集成

- Google OAuth2 处理器：[`integrations/oauth/google.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/google.py)

```python title="backend/integrations/oauth/google.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/google.py:GoogleOAuthHandlerExample"
```

您可以看到 Google 定义了一个 `DEFAULT_SCOPES` 变量，这用于设置无论用户请求什么都会请求的作用域。

```python title="backend/blocks/google/_auth.py"
--8<-- "autogpt_platform/backend/backend/blocks/google/_auth.py:GoogleOAuthIsConfigured"
```

您还可以看到 `GOOGLE_OAUTH_IS_CONFIGURED` 用于在未配置 OAuth 时禁用需要 OAuth 的模块。这在每个模块的 `__init__` 方法中。这是因为 Google 模块没有 API 密钥回退，因此我们需要确保在允许用户使用这些模块之前已配置 OAuth。

### Webhook 触发的模块

Webhook 触发的模块允许您的智能体实时响应外部事件。
这些模块由来自第三方服务的传入 Webhook 触发，
而非手动执行。

创建和运行 Webhook 触发的模块涉及三个主要组件：

- 模块本身，用于指定：
    - 用户选择资源和订阅事件的输入项
    - 带有管理 Webhook 所需权限范围的 `credentials` 输入
    - 将 Webhook 有效载荷转换为 Webhook 模块输出的逻辑
- 对应 Webhook 服务提供商的 `WebhooksManager`，负责处理：
    - 向提供商（取消）注册 Webhook
    - 解析和验证传入的 Webhook 有效载荷
- 对应服务提供商的凭证系统，可能包含 `OAuthHandler`

底层还有更多机制在运作，例如存储和检索 Webhook 及其与节点的链接，
但添加 Webhook 触发的模块时，您无需对系统的这些部分进行修改。

#### 创建 Webhook 触发的模块

要创建 Webhook 触发的模块，请在基本模块创建流程的基础上遵循以下额外步骤：

1. **在块的 `__init__` 方法中定义 `webhook_config`**

    <details>
    <summary>示例：<code>GitHubPullRequestTriggerBlock</code></summary>

    ```python title="backend/blocks/github/triggers.py"
    --8<-- "autogpt_platform/backend/backend/blocks/github/triggers.py:example-webhook_config"
    ```
    </details>

    <details>
    <summary><code>BlockWebhookConfig</code> 定义</summary>

    ```python title="backend/data/block.py"
    --8<-- "autogpt_platform/backend/backend/data/block.py:BlockWebhookConfig"
    ```
    </details>

2. **在块的输入模式中定义事件过滤器输入**
    这允许用户选择在其代理中触发块的具体事件类型。

    <details>
    <summary>示例：<code>GitHubPullRequestTriggerBlock</code></summary>

    ```python title="backend/blocks/github/triggers.py"
    --8<-- "autogpt_platform/backend/backend/blocks/github/triggers.py:example-event-filter"
    ```
    </details>

    - 输入字段的名称（本例中为 `events`）必须与 `webhook_config.event_filter_input` 匹配。
    - 事件过滤器本身必须是一个仅包含布尔字段的 Pydantic 模型。

4. **在块的输入模式中包含有效载荷字段**

    <details>
    <summary>示例：<code>GitHubTriggerBase</code></summary>

    ```python title="backend/blocks/github/triggers.py"
    --8<-- "autogpt_platform/backend/backend/blocks/github/triggers.py:example-payload-field"
    ```
    </details>

5. **在块的输入模式中定义 `credentials` 输入**
    - 其权限范围必须足以通过提供商的 API 管理用户的 webhook
    - 更多详细信息请参阅[带身份验证的块](#blocks-with-authentication)

6. **处理 webhook 有效载荷**并在块的 `run` 方法中输出其相关部分。

    <details>
    <summary>示例：<code>GitHubPullRequestTriggerBlock</code></summary>

    ```python
    def run(self, input_data: Input, **kwargs) -> BlockOutput:
        yield "payload", input_data.payload
        yield "sender", input_data.payload["sender"]
        yield "event", input_data.payload["action"]
        yield "number", input_data.payload["number"]
        yield "pull_request", input_data.payload["pull_request"]
    ```

    请注意，如果凭据在块运行时未被使用（如示例中所示），可以省略 `credentials` 参数。
    </details>

#### 添加 Webhooks 管理器

要添加对新 webhook 提供商的支持，您需要创建一个实现 `BaseWebhooksManager` 接口的 WebhooksManager：

```python title="backend/integrations/webhooks/_base.py"
--8<-- "autogpt_platform/backend/backend/integrations/webhooks/_base.py:BaseWebhooksManager1"

--8<-- "autogpt_platform/backend/backend/integrations/webhooks/_base.py:BaseWebhooksManager2"
--8<-- "autogpt_platform/backend/backend/integrations/webhooks/_base.py:BaseWebhooksManager3"
--8<-- "autogpt_platform/backend/backend/integrations/webhooks/_base.py:BaseWebhooksManager4"
--8<-- "autogpt_platform/backend/backend/integrations/webhooks/_base.py:BaseWebhooksManager5"
```

并在 `load_webhook_managers` 中添加对您的 `WebhooksManager` 类的引用：

```python title="backend/integrations/webhooks/__init__.py"
--8<-- "autogpt_platform/backend/backend/integrations/webhooks/__init__.py:load_webhook_managers"
```

#### 示例：GitHub Webhook 集成

<details>
<summary>
GitHub Webhook 触发器：<a href="https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/blocks/github/triggers.py"><code>blocks/github/triggers.py</code></a>
</summary>

```python title="backend/blocks/github/triggers.py"
--8<-- "autogpt_platform/backend/backend/blocks/github/triggers.py:GithubTriggerExample"
```

</details>

<details>
<summary>
GitHub Webhooks 管理器：<a href="https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/webhooks/github.py"><code>integrations/webhooks/github.py</code></a>
</summary>

```python title="backend/integrations/webhooks/github.py"
--8<-- "autogpt_platform/backend/backend/integrations/webhooks/github.py:GithubWebhooksManager"
```

</details>

## 关键要点

- **唯一标识符**：在 **init** 方法中为您的块指定一个唯一 ID
- **输入和输出模式**：定义清晰的输入和输出模式
- **错误处理**：在 `run` 方法中实现错误处理
- **输出结果**：在 `run` 方法中使用 `yield` 输出结果
- **测试**：在 **init** 方法中提供测试输入和输出以进行自动测试

## 理解测试流程

块的测试由 `test_block.py` 处理，该文件执行以下操作：

1. 它使用提供的 `test_input` 调用代码块。
   如果代码块包含 `credentials` 字段，则同时传入 `test_credentials`。
2. 如果提供了 `test_mock`，它会临时用模拟函数替换指定的方法。
3. 然后断言输出与 `test_output` 匹配。

对于 WikipediaSummaryBlock：

- 测试将使用主题 "Artificial Intelligence" 调用该代码块
- 它将使用模拟函数而非进行实际的 API 调用，该函数返回 `{"extract": "summary content"}`
- 然后检查输出键是否为 "summary" 及其值是否为字符串

这种方法使我们能够全面测试代码块的逻辑，而无需依赖外部服务，同时还能适应非确定性输出。

## SSRF 防护的安全最佳实践

创建处理外部 URL 输入或进行网络请求的代码块时，使用平台内置的 SSRF 保护机制至关重要。`backend.util.request` 模块提供了一个安全的 `Requests` 包装器类，应用于所有 HTTP 请求。

### 使用安全请求包装器

```python
from backend.util.request import requests

class MyNetworkBlock(Block):
    def run(self, input_data: Input, **kwargs) -> BlockOutput:
        try:
            # The requests wrapper automatically validates URLs and blocks dangerous requests
            response = requests.get(input_data.url)
            yield "result", response.text
        except ValueError as e:
            # URL validation failed
            raise RuntimeError(f"Invalid URL provided: {e}")
        except requests.exceptions.RequestException as e:
            # Request failed
            raise RuntimeError(f"Request failed: {e}")
```

`Requests` 包装器提供以下安全功能：

1. **URL 验证**:
    - 阻止对私有 IP 范围（RFC 1918）的请求
    - 验证 URL 格式和协议
    - 解析 DNS 并检查 IP 地址
    - 支持白名单可信来源

2. **安全默认设置**:
    - 默认禁用重定向
    - 对非 200 状态码抛出异常
    - 支持自定义标头和验证器

3. **受保护的 IP 范围**:
   包装器拒绝访问以下网络的请求：

    ```python title="backend/util/request.py"
    --8<-- "autogpt_platform/backend/backend/util/request.py:BLOCKED_IP_NETWORKS"
    ```

### 自定义请求配置

如需自定义请求行为：

```python
from backend.util.request import Requests

# Create a custom requests instance with specific trusted origins
custom_requests = Requests(
    trusted_origins=["api.trusted-service.com"],
    raise_for_status=True,
    extra_headers={"User-Agent": "MyBlock/1.0"}
)
```

## 有效阻断测试技巧

1. **提供真实的测试输入**：确保测试输入涵盖典型使用场景。

2. **定义合适的测试输出**：

    - 对于确定性输出，使用具体的期望值。
    - 对于非确定性输出或仅需关注类型的情况，使用 Python 类型（如 `str`、`int`、`dict`）。
    - 可以混合使用具体值和类型，例如 `("key1", str), ("key2", 42)`。

3. **对网络调用使用 test_mock**：这可以防止因网络问题或 API 变更导致测试失败。

4. **考虑省略无外部依赖的测试模拟**：如果代码块不进行网络调用或使用外部资源，可能不需要模拟。

5. **考虑边界情况**：在 `run` 方法中包含对潜在错误条件的测试。

6. **修改代码块行为时更新测试**：如果修改了代码块，请确保相应更新测试。

通过遵循这些步骤，您可以创建扩展 AutoGPT Agent Server 功能的新代码块。

## 我们希望看到的代码块

以下是我们希望在 AutoGPT Agent Server 中实现的代码块列表。如果您有兴趣贡献，请随意选择其中一个代码块或选择您自己的创意。

如果您想实现其中一个代码块，请提交拉取请求，我们将启动审核流程。

### 消费者服务/平台

- Google sheets - [~~读取/追加~~](https://github.com/Significant-Gravitas/AutoGPT/pull/8236)
- 电子邮件 - 通过 [~~Gmail~~](https://github.com/Significant-Gravitas/AutoGPT/pull/8236)、Outlook、Yahoo、Proton 等读取/发送
- 日历 - 通过 Google Calendar、Outlook Calendar 等读取/写入
- Home Assistant - 调用服务、获取状态
- Dominos - 订购披萨、跟踪订单
- Uber - 预订乘车、跟踪行程
- Notion - 创建/读取页面、创建/追加/读取数据库
- Google drive - 读取/写入/覆盖文件/文件夹

### 社交媒体

- Twitter - 发布推文、回复、获取回复、获取评论、获取粉丝、获取关注、获取推文、获取提及
- Instagram - 发布帖子、回复、获取评论、获取粉丝、获取关注、获取帖子、获取提及、获取热门帖子
- TikTok - 发布视频、回复、获取评论、获取粉丝、获取关注、获取视频、获取提及、获取热门视频
- LinkedIn - 发布帖子、回复、获取评论、获取粉丝、获取关注、获取帖子、获取提及、获取热门帖子
- YouTube - 转录视频/短视频、发布视频/短视频、读取/回复/回应评论、更新缩略图、更新描述、更新标签、更新标题、获取观看次数、获取点赞、获取点踩、获取订阅者、获取评论、获取分享、获取观看时长、获取收入、获取热门视频、获取热门视频、获取热门频道
- Reddit - 发布帖子、回复、获取评论、获取粉丝、获取关注、获取帖子、获取提及、获取热门帖子
- Treatwell（及相关平台）- 预订、取消、评价、获取推荐
- Substack - 阅读/订阅/取消订阅、发布/回复、获取推荐
- Discord - 读取/发布/回复、管理操作
- GoodReads - 阅读/发布/回复、获取推荐

### 电子商务

- Airbnb - 预订、取消、评价、获取推荐
- Amazon - 下单、跟踪订单、退货、评价、获取推荐
- eBay - 下单、跟踪订单、退货、评价、获取推荐
- Upwork - 发布工作、雇佣自由职业者、评价自由职业者、解雇自由职业者

### 商业工具

- External Agents - 调用类似 AutoGPT 的其他智能体
- Trello - 创建/读取/更新/删除卡片、列表、看板
- Jira - 创建/读取/更新/删除问题、项目、看板
- Linear - 创建/读取/更新/删除问题、项目、看板
- Excel - 读取/写入/更新/删除行、列、工作表
- Slack - 读取/发布/回复消息，创建频道，邀请用户
- ERPNext - 创建/读取/更新/删除发票、订单、客户、产品
- Salesforce - 创建/读取/更新/删除潜在客户、商机、账户
- HubSpot - 创建/读取/更新/删除联系人、交易、公司
- Zendesk - 创建/读取/更新/删除工单、用户、组织
- Odoo - 创建/读取/更新/删除销售订单、发票、客户
- Shopify - 创建/读取/更新/删除产品、订单、客户
- WooCommerce - 创建/读取/更新/删除产品、订单、客户
- Squarespace - 创建/读取/更新/删除页面、产品、订单

## 我们希望看到的智能体模板

### 数据/信息

- 通过 Apple News 或其他大型媒体（如 BBC、TechCrunch、hackernews 等）汇总今日、本周、本月的重要新闻
- 创建、阅读并总结 Substack 新闻简报或任何类型的新闻简报（博客作者视角 vs 博客读者视角）
- 获取/阅读/总结当日、本周、本月在 Twitter、Instagram、TikTok（泛指社交媒体账号）上最热门的帖子
- 获取/阅读提及 AI Agents 的 LinkedIn 帖子或个人资料
- 阅读/总结 Discord 内容（可能因需要访问权限而无法实现）
- 从 GoodReads 或 Amazon Books 等平台获取指定月份、年份等时段内最受关注的书籍
- 获取所有流媒体平台上特定节目的播出日期
  - 推荐/获取所有流媒体平台在指定月份、年份等时段内最受欢迎的节目
- 对 xlsx 数据集进行数据分析
  - 通过 Excel 或 Google Sheets 采集数据 > 随机抽样数据（抽样模块可提取前 X 条、后 X 条或随机抽取等）> 将数据传递给 LLM 模块生成分析完整数据的脚本 > Python 模块运行脚本 > 出错时通过 LLM 修复模块循环处理 > 创建图表/可视化（可能在代码块中实现）> 将图像显示为输出（可能需要前端调整以展示）
- TikTok 视频搜索与下载

### 市场营销

- 作品集网站设计与优化