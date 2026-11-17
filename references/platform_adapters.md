# 平台适配与工具落地

这份文档解决一个问题：同一个流程，在不同 Agent 环境里分别用什么工具落地。流程和产物标准不变，变的只是调用方式。

## 一、需要的四项能力

不管在哪个平台，跑通这套流程只需要四项能力：

1. **读文件**：能拿到本地音频文件，或能访问音频 URL。
2. **音频转写**：能把录音转成文本，最好带说话人或时间信息。
3. **联网检索**：能搜索术语、背景和延伸资料。
4. **写文件并交付**：能把笔记写成 Markdown 并交给用户。

四项里有缺项时，先向用户说明缺哪一项、会影响哪个环节，不要跳过环节假装完成。

## 二、落地对照

| 能力 | WorkBuddy | Claude Code / Codex CLI / Gemini CLI | IDE 类 Agent（Cursor 等） | 纯脚本环境 |
| --- | --- | --- | --- | --- |
| 读文件 | 拖进对话，或给本地路径、URL | 给工作目录内的路径 | 打开工作区文件 | 绝对路径 |
| 音频转写 | AudioTranscribe（先检索工具 schema 再调用） | 自建 Whisper 脚本，或调用内部语音识别接口 | 同左 | ffmpeg + faster-whisper |
| 联网检索 | WebSearch | 内置 web 搜索，或用 curl 调搜索 API | 内置搜索 | curl 调搜索 API |
| 写文件与交付 | 写入工作区并以预览打开 | 写入工作目录并给出路径 | 写入工作区并打开 | 直接落盘 |

平台若已内置转写，优先用内置的：省去部署，也更省时间。只有内置能力缺失、或需要离线处理时，才走本地 Whisper 路线。

## 三、没有 skills 机制时怎么用

- **支持自定义指令的产品**：把 `SKILL.md` 全文粘进系统提示或项目规则文件，`references/` 三份文档作为附加资料一并提供。
- **支持 slash command 的产品**：把 SKILL.md 包装成一条命令，命令体只写触发条件，正文引用 SKILL.md。
- **什么都没有**：把 SKILL.md 当一份提示词模板，按阶段顺序执行即可，产物标准不受影响。

## 四、自建转写的最小方案

环境没有内置转写时，最省事的组合是 ffmpeg 抽音轨加 faster-whisper：

```bash
# 1. 抽成 16kHz 单声道 wav，转写模型对采样率敏感
ffmpeg -i lecture.mp3 -ar 16000 -ac 1 lecture.wav

# 2. 安装并转写（首次运行会下载模型）
pip install faster-whisper

python - <<'PY'
from faster_whisper import WhisperModel
model = WhisperModel("medium", device="cpu", compute_type="int8")
segments, info = model.transcribe("lecture.wav", language="zh", vad_filter=True)
with open("lecture.txt", "w", encoding="utf-8") as f:
    for seg in segments:
        f.write(f"[{seg.start:.1f}-{seg.end:.1f}] {seg.text.strip()}\n")
print("done")
PY
```

- 模型按算力选：CPU 用 `small` 或 `medium`，有 GPU 可用 `large-v3` 提升专有名词准确率。
- 长音频先按 20–30 分钟切段，逐段转写再合并，避免内存占用过高和中断重跑。
- 需要区分说话人时，配合说话人分离工具（如 pyannote）或 Whisper 的 speaker diarization 分支，否则在笔记存疑区注明「说话人依据语境推断」。

## 五、隐私与合规

- 课堂录音通常包含老师和同学的声音，属于他人信息。上传到第三方语音识别或大模型服务前，先确认是否已获授权、是否符合学校或公司规定。
- 能本地处理就本地处理；必须上云时，优先选择承诺不留存、不训练的接口。
- 笔记里不要写入与教学内容无关的个人信息（姓名、联系方式、病情、家庭情况等）；确需保留的提问场景，用「一位同学提问」这类匿名写法。

## 六、不随平台改变的部分

以下三项是产物标准，换平台也必须遵守，与工具无关：

- 七节结构：概览、知识脉络、重点清单、知识点详解、补充与延伸、存疑与待确认、术语表。
- 三类内容标注：课堂原话、整理后的解释、联网补充，三者不得混淆。
- 存疑区必写：听不清的、板书缺失的、课堂与检索冲突的、术语未确认的。
