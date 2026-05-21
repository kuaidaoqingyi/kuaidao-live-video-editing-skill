# Kuaidao Live Video Editing Skill

快刀短视频剪辑工作流：把中文直播、访谈、播客、课程、带货讲解等长视频，剪成一条上下文完整、字幕干净、封面和包装可交付的竖版短视频。

当前版本：`0.3.0`

## 解决什么问题

这个仓库沉淀的是快刀在真实直播剪辑里磨出来的一套流程，不是单个剪辑脚本。

它适合处理这类任务：

- 从 1-4 小时直播原片里定位一个主题段落
- 从长视频里先筛出多条候选短视频，再决定剪哪几条
- 把原始口播剪成 5-10 分钟左右的完整短视频
- 加中文字幕、花字、封面、画中画素材和结尾信息
- 保留每一版草稿，避免覆盖后无法回退
- 让同事或 Agent 能按统一工程结构接手继续做

## 已实现功能

### 0. 导演型规格门禁

做视频前先把关键问题钉住：给谁看、发在哪、时长多少、冷启动观众为什么要继续看、哪些内容必须保留、哪些表达绝对不要。

详见：`references/story-spec-and-gates.md`

### 1. ASR / TTS 默认走 StepFun

做字幕和配音前必须先确认 StepFun / StepAudio 路径可用。

默认凭证读取顺序：

```text
macOS Keychain service STEPFUN_API_KEY
env STEPFUN_API_KEY
env STEP_API_KEY
```

默认 ASR 参考：

```text
https://platform.stepfun.com/docs/zh/guides/models/stepaudio-2.5-asr
```

默认 TTS：`stepaudio-2.5-tts`。中文文化叙事或温暖短视频旁白优先 voice `ruyananshi`，speed 约 `1.12`。

不要把 `.env`、API key、access token 提交到 GitHub。不要在聊天、脚本、日志或文档里打印原始密钥。

### 2. 长视频工程初始化

- 约定标准工程目录
- 原片不复制、不覆盖，优先用 symlink 或代理文件
- 用 `project.yaml` 记录源文件、目标平台、ASR、TTS、规划状态、版本、QA 状态
- 区分 Skill 仓库和具体视频工程文件夹

详见：`references/project-structure-and-outputs.md`

### 3. ASR 和内容定位

- 要求先建 glossary：人名、书名、地点、品牌、性别代词、常见 ASR 错误
- 支持按项目配置中文 ASR 供应商，但默认 StepFun / StepAudio
- 输出 `raw-transcript`、`candidate-windows.md`、`asr-issues.md`
- 明确不能只靠抽帧或道具出现来判断内容段落

### 4. 候选池和故事剪辑

- 适配 “从长直播里挑几条强短视频” 的候选池模式
- 适配 5-10 分钟短视频结构
- 强调前 10 秒留人，但随后必须补上下文
- 保护人物介绍、情绪酝酿、引用完整性和收尾姿态
- 提供 `templates/cut-plan.md` 做可执行剪辑规格

详见：`references/story-editing.md`

### 5. 中文字幕规范

- 清理口水词、重复起句和 ASR 噪声
- 纠正人名、书名、地点、概念和性别代词
- 最终字幕默认不用普通中文标点，使用半角空格
- 每行尽量控制在 16 个中文字左右
- 提供字幕 QA 脚本

脚本：

```bash
python3 scripts/subtitle_qa.py path/to/subtitles.srt --max-chars 16
```

带错词表：

```bash
python3 scripts/subtitle_qa.py subtitles.srt \
  --bad-term 何菜头=和菜头 \
  --bad-term 段瑞=段睿
```

详见：`references/subtitle-rules.md`

### 6. 封面、花字和画中画

- 封面优先使用明亮的人物画面
- 默认两行大标题
- 图片、书封、截图、视频素材用画中画方式插入
- 情绪段落优先保留脸和眼神，不用全屏字卡遮挡
- HyperFrames 用于统一做字幕、PIP、花字和最终包装

详见：`references/cover-and-overlays.md`

### 7. 版本、返修和交付管理

- 每个用户看过的版本都用 `vNN-slug` 保留
- `final.mp4` 只作为快捷 copy，不作为正式归档
- 每个交付版本都应有视频、字幕、封面、编辑说明和 QA 文件
- 返修按轻度 / 中度 / 重度分级，避免为了一个字幕问题重跑整套流程
- 提供 `templates/review.md`、`templates/qa.md`、`templates/version-log.md`

详见：`references/versioning-and-handoff.md`

### 8. 真实案例沉淀

- `references/liukaixin-case-study.md` 记录了刘开心书籍直播切片项目中的经验
- 包括开头突兀、人物背景缺失、字幕规范、封面风格、情绪段落和 PIP 插入等真实迭代点

## 仓库结构

```text
.
├── SKILL.md
├── VERSION
├── CHANGELOG.md
├── README.md
├── references/
│   ├── cover-and-overlays.md
│   ├── liukaixin-case-study.md
│   ├── project-structure-and-outputs.md
│   ├── story-editing.md
│   ├── story-spec-and-gates.md
│   ├── subtitle-rules.md
│   └── versioning-and-handoff.md
├── scripts/
│   └── subtitle_qa.py
└── templates/
    ├── asset-log.md
    ├── cut-plan.md
    ├── project.yaml
    ├── qa.md
    ├── review.md
    └── version-log.md
```

## 如何使用

### 方式一：作为 Codex/Claude Skill 使用

把整个仓库复制或克隆到你的 skills 目录，例如：

```bash
git clone https://github.com/kuaidaoqingyi/kuaidao-live-video-editing-skill.git \
  ~/.codex/skills/kuaidao-live-video-editing
```

之后可以用自然语言触发：

```text
帮我把这场直播剪成一条 8-10 分钟短视频
```

或：

```text
按 kuaidao-live-video-editing 这个 skill 处理这条访谈切片
```

### 方式二：作为团队工作流模板使用

新项目先复制模板：

```text
templates/project.yaml
templates/cut-plan.md
templates/review.md
templates/qa.md
templates/version-log.md
templates/asset-log.md
```

然后按 `references/project-structure-and-outputs.md` 建立视频工程目录。

## 标准交付包

每条成片至少交付：

```text
final-vNN-slug.mp4
cover-vNN-slug.jpg
subtitles-vNN-slug.srt
edit-notes-vNN-slug.md
qa-vNN-slug.md
project.yaml
```

如果需要其他同事继续编辑，还应交付：

```text
cut-plan.md
notes/glossary.md
notes/candidate-windows.md
media/assets/asset-log.md
versions/version-log.md
versions/index/index-vNN-slug.html
```

## 版本管理

本仓库使用语义化版本：

- `MAJOR`：工作流结构或文件合同发生破坏性变化
- `MINOR`：新增能力、模板、reference 或脚本
- `PATCH`：修正文案、补充说明、小脚本 bugfix

当前版本记录在：

```text
VERSION
CHANGELOG.md
```

每次沉淀新规则时：

1. 修改 `SKILL.md`、`references/`、`templates/` 或 `scripts/`
2. 更新 `CHANGELOG.md`
3. 更新 `VERSION`
4. 提交并打 tag，例如 `v0.3.0`
5. 推送 main 和 tag 到 GitHub

## 不要提交什么

不要把以下内容提交到这个 Skill 仓库：

- 原始视频
- 渲染成片
- 用户照片、书图、B-roll 等素材
- API key 或任何密钥
- 某个具体项目的完整工程目录

具体项目应该单独放在工作目录里，只把可复用规则回流到本仓库。
