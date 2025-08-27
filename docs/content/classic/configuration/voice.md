# 文本转语音

输入此命令以使用 TTS（文本转语音）功能为 AutoGPT 提供语音支持

```shell
./autogpt.sh --speak
```

Eleven Labs 提供语音技术，包括语音设计、语音合成和预制语音，AutoGPT 可使用这些功能进行语音输出。

1. 访问 [ElevenLabs](https://beta.elevenlabs.io/)，如无账户请先注册
2. 选择并设置 *Starter* 套餐
3. 点击右上角图标，找到 *Profile* 以获取您的 API 密钥

在 `.env` 文件中设置：

- `ELEVENLABS_API_KEY`
- `ELEVENLABS_VOICE_1_ID`（示例：_"premade/Adam"_）

### 可用语音列表

!!! note
    您可以使用名称或语音 ID 来配置语音

| 名称   | 语音 ID |
| ------ | -------- |
| Rachel | `21m00Tcm4TlvDq8ikWAM` |
| Domi   | `AZnzlk1XvdvUeBnXmlld` |
| Bella  | `EXAVITQu4vr4xnSDxMaL` |
| Antoni | `ErXwobaYiN019PkySvjV` |
| Elli   | `MF3mGyEYCl7XYWbV9V6O` |
| Josh   | `TxGEqnHWrfWFTfGW9XjX` |
| Arnold | `VR6AewLTigWG4xSOukaG` |
| Adam   | `pNInz6obpgDQGcFmaJgB` |
| Sam    | `yoZ06aMxZJJ28mfd3POQ` |