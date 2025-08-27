## Replicate Flux 高级模型

### 功能说明

Replicate Flux 高级模型模块是一款基于人工智能的图像生成工具，能够根据文本提示和各种可自定义设置创建图像。

### 功能描述

该模块使用 Replicate 提供的先进 AI 模型（特别是 Flux 系列模型）生成高质量图像。用户可输入文本描述并调整各种参数来微调图像生成过程。

### 工作原理

该模块接收文本提示和多个自定义选项作为输入，随后将这些信息发送至 Replicate 平台上选定的 Flux 模型。AI 模型处理输入数据并根据提供的规格生成图像，最终模块会返回生成图像的 URL。

### 输入

| 输入 | 描述 |
|-------|-------------|
| API Key | 用于身份验证的 Replicate API 密钥 |
| Prompt | 所需生成图像的文本描述（例如："日落时分的未来主义城市景观"） |
| Image Generation Model | 从 Flux Schnell、Flux Pro 或 Flux Pro 1.1 中选择 |
| Seed | 用于可重复图像生成的可选数字 |
| Steps | 图像生成过程中的扩散步骤数量 |
| Guidance | 控制图像与文本提示的贴合程度 |
| Interval | 影响可能输出的多样性 |
| Aspect Ratio | 生成图像的宽高比 |
| Output Format | 在 WEBP、JPG 或 PNG 文件格式之间选择 |
| Output Quality | JPG 和 WEBP 格式的图像质量设置（0-100） |
| Safety Tolerance | 内容安全设置，范围从 1（最严格）到 5（最宽松） |

### 输出结果

| 输出 | 描述 |
|--------|-------------|
| Result | 生成图像的 URL 链接 |
| Error | 图像生成过程失败时的错误信息 |

### 可能的使用场景

平面设计师可以使用此模块快速生成科幻游戏的概念艺术。他们可能会输入类似"遥远星球上的未来主义太空港，天空中有多个卫星"的提示，并调整设置以获得所需的风格和质量。生成的图像随后可作为灵感来源或进一步设计工作的起点。

- API Key：用于身份验证的 Replicate API 密钥
- Prompt：所需生成图像的文本描述（例如："日落时分的未来城市景观"）
- Image Generation Model：从 Flux Schnell、Flux Pro 或 Flux Pro 1.1 中选择
- Seed：用于可重复图像生成的可选数字
- Steps：图像生成过程中的扩散步骤数量
- Guidance：控制图像与文本提示的贴合程度
- Interval：影响可能输出的多样性
- Aspect Ratio：生成图像的宽高比
- Output Format：在 WEBP、JPG 或 PNG 文件格式之间选择
- Output Quality：JPG 和 WEBP 格式的图像质量设置（0-100）
- Safety Tolerance：内容安全设置，范围从 1（最严格）到 5（最宽松）

### 输出结果

- Result：生成图像的 URL 链接
- Error：图像生成过程失败时的错误消息

### 可能的使用场景

平面设计师可以使用此模块快速生成科幻游戏的概念图。他们可以输入类似"遥远星球上的未来太空港，天空中有多个卫星"的提示词，并通过调整设置来获得所需的风格和质量。生成的图像可作为灵感来源或进一步设计工作的起点。