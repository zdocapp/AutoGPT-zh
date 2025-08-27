# 查找 D-ID 可用语音

1. **ElevenLabs**
   - 从语音列表中选择任意语音：https://api.elevenlabs.io/v1/voices
   - 复制 voice_id
   - 在 CreateTalkingAvatarClip 块的 voice_id 字段中将其作为字符串使用

2. **Microsoft Azure 语音**
    - 从语音库中选择任意语音：https://speech.microsoft.com/portal/voicegallery
    - 点击右侧的"示例代码"标签页
    - 复制语音名称，例如：config.SpeechSynthesisVoiceName ="en-GB-AbbiNeural"
    - 在 CreateTalkingAvatarClip 块的 voice_id 字段中使用此字符串 en-GB-AbbiNeural

3. **Amazon Polly 语音**
    - 从语音列表中选择任意语音：https://docs.aws.amazon.com/polly/latest/dg/available-voices.html
    - 复制语音名称/ID
    - 在 CreateTalkingAvatarClip 块的 voice_id 字段中将其作为字符串使用