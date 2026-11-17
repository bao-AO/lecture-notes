# 课堂录音笔记 / Lecture Notes

把一整节课的录音，变成一份能直接拿来复习的流畅知识笔记。

Turn a full lecture recording into a clear, study-ready knowledge notebook.

## 它能做什么 / What it does

把课堂录音转成结构化 Markdown 学习笔记：转写文字 → 提炼老师强调的重点 → 按课堂逻辑重建知识脉络 → 联网检索补充背景资料 → 输出连贯、可自学的笔记。不是零散的转写堆砌，而是一份拿来就能复习的知识讲义。

Transcribes class audio, extracts the teacher's emphasized points, rebuilds the lesson's logical flow, enriches with web-sourced background material, and outputs a coherent Markdown notebook you can actually study from.

### 核心能力 / Key features

- **音频转写** — 支持 mp3、m4a、wav、mp4 等格式，最长 5 小时 / Audio transcription — mp3, m4a, wav, mp4 and more, up to 5 hours
- **重点提炼** — 识别老师强调和反复出现的考点，按类别标注 / Key-point extraction, flagged and categorized
- **知识脉络** — 带衔接的连贯讲解，还原课堂推进逻辑 / Knowledge flow with connecting explanations
- **术语校正** — 对听错的专业名词联网查证 / Term correction via web search
- **联网补充** — 检索权威资料，逐条标注来源 / Enrichment with cited authoritative sources
- **存疑标注** — 听不清、板书缺失、说法冲突处明确标注 / Explicit uncertainty flags

适用场景：大学生、研究生和培训学员复习课堂、讲座、公开课。
Best for: undergrads, grad students, and learners reviewing lectures, talks, and open courses.

## 安装 / Installation

方式一：克隆到 WorkBuddy 用户技能目录（跨项目可用）
Clone into the WorkBuddy user skills directory (available across all projects):

```bash
git clone https://github.com/<你的用户名>/lecture-notes.git ~/.workbuddy/skills/lecture-notes
```

方式二：下载 ZIP，解压到 `~/.workbuddy/skills/lecture-notes/`
Or download the ZIP and extract it to `~/.workbuddy/skills/lecture-notes/`.

Windows 用户目录通常是 `C:\Users\<你的用户名>\.workbuddy\skills\lecture-notes\`。
On Windows the path is typically `C:\Users\<your-name>\.workbuddy\skills\lecture-notes\`.

## 使用 / Usage

把录音文件拖进 WorkBuddy 对话，说一句「把这段录音整理成笔记」即可。也支持音频 URL。

Drop an audio file into a WorkBuddy conversation and say "把这段录音整理成笔记" (turn this recording into notes). Audio URLs are also accepted.

笔记会保存为工作区下的 `.md` 文件并自动打开预览。
The note is saved as a `.md` file in the workspace and opened for preview.

## 目录结构 / Structure

```
lecture-notes/
├── SKILL.md                        工作流与触发条件 / Workflow and triggers
└── references/
    ├── keypoint_detection.md       重点识别与脉络重建 / Key-point and flow rules
    ├── enrichment_search.md        检索与术语校正 / Search and term correction
    └── output_template.md          笔记模板与写法示例 / Note template and examples
```

## 要求 / Requirements

- WorkBuddy（使用内置 AudioTranscribe 与 WebSearch 工具）
- 无需额外安装依赖 / No extra dependencies

## 许可 / License

MIT
