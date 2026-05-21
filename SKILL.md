---
name: kuaidao-live-video-editing
description: Use when turning Chinese livestreams, interviews, podcasts, courses, or long-form talking-head videos into story-complete vertical short videos for Kuaidao-style channels. Covers source ingest, StepFun/StepAudio ASR, topic/window discovery, story-spec gates, 5-10 minute story editing, Chinese subtitle cleanup, cover selection, picture-in-picture assets, HyperFrames packaging, versioning, QA, and teammate handoff. Triggers: 快刀, 直播剪辑, 长视频切短视频, 访谈切片, 课程切片, 书籍种草短视频, 加字幕, 做封面, 画中画, HyperFrames, 视频工作流, 短视频工作流.
---

# Kuaidao Live Video Editing

快刀短视频剪辑工作流 turns a long Chinese source video into a publishable short video with clean subtitles, strong story structure, versioned outputs, and team handoff files.

## Use This For

- 直播原片剪成 5-10 分钟短视频
- 访谈、播客、课程、分享、带货讲解的主题切片
- 从长视频里筛选多条候选短视频
- 需要中文字幕、花字、封面、画中画素材、版本管理和 QA 的视频
- 需要多人协作接力的视频工程

## Do Not Use This For

- 只需要简单截一刀的任务
- 没有转写也不需要字幕的纯视觉混剪
- 多机位自动切换本身，那应先用专门的 multicam workflow
- 上传发布平台自动化，本 skill 只到交付文件

## Core Rules

- 原片不复制、不覆盖、不重编码作为中间源，尽量用 symlink 和代理文件。
- 先转写再判断，不只靠画面抽帧定位内容。
- 开头 10 秒要抓人，随后必须补足冷启动上下文。
- 字幕是清理后的口语字幕，不是原始 ASR 逐字稿。
- 封面、字幕、工程、成片全部版本化，不覆盖用户已看过的版本。
- 自动化机械动作，保留故事、标题、情绪和剪点判断。
- 用户说“牛逼一点 / 高级一点 / 节奏好一点”不算需求；必须追到具体画面、剪点、观众反应和发布平台。

## Startup Mode Check

Before doing production work, inspect the project folder and classify the request:

1. New edit mode: no usable `project.yaml` or `cut-plan.md`, or the user is starting from a new source video.
2. Continue-edit mode: `project.yaml`, `cut-plan.md`, `edit-notes.md`, or version logs already exist and the user wants to continue the project.
3. Iteration mode: the user asks to revise a reviewed draft, such as subtitles, pacing, PIP placement, cover, music, or story structure.
4. Single-surface mode: the user only asks for subtitles, cover, QA, ASR, or packaging.

Do not restart the full workflow when the user is clearly iterating. Read existing notes first, then touch only the affected planning surface.

## Story Spec Gates

Read `references/story-spec-and-gates.md` before creating or changing a cut plan.

Minimum gates before rendering or final subtitles:

- purpose: what the video should make the viewer remember or do
- audience: concrete viewer and viewing context
- platform and target duration
- cold-viewer promise and core information
- source/transcript/material state
- must-keep and must-remove constraints
- visual/subtitle/cover expectations
- references and anti-examples when style matters

If a gate is missing and cannot be inferred from existing project files, ask only the missing high-impact question. Do not make up story goals or visual references silently.

## Iteration Discipline

Classify requested changes by impact:

- Light change: wording, subtitle cleanup, accent color, cover title text, small timing adjustment within about 0.5s. Confirm the exact replacement and update the relevant file.
- Medium change: add/drop/reorder a segment, replace PIP, move subtitles, change cover frame, change transition or audio treatment. Update `cut-plan.md`, `edit-notes.md`, and the next version log entry.
- Heavy change: core topic, audience, target platform, target duration, story arc, or the emotional peak changes. Produce an affected-scope summary first, then revise the cut plan after confirmation.

When timecoded feedback exists, preserve the reviewer wording and translate it into the smallest concrete file/version change that fixes the issue.

## Standard Outputs

For the exact project folder contract and delivery package, read `references/project-structure-and-outputs.md`.

```text
final-vNN-*.mp4
cover-vNN-*.jpg|png
subtitles-vNN-*.srt
edit-notes-vNN-*.md
qa-vNN-*.md
project.yaml
cut-plan.md
```

## Workflow

### 1. Ingest

Create a project folder, fill `templates/project.yaml`, and follow `references/project-structure-and-outputs.md`.

Run `ffprobe` to record source dimensions, duration, frame rate, and audio presence.

### 2. Transcribe

For Chinese video, use StepFun / StepAudio first unless the user explicitly asks for another provider.

Before ASR, check credentials in this order:

1. macOS Keychain service `STEPFUN_API_KEY`
2. environment variable `STEPFUN_API_KEY`
3. environment variable `STEP_API_KEY`

Never ask the user to paste the raw key into chat, project files, scripts, logs, or GitHub. If StepFun is unavailable, stop and ask before using local ASR such as Whisper.

Before ASR, create a glossary:

- people, gender pronouns, aliases
- book/product/course names
- locations and organization names
- likely ASR confusions

Outputs:

```text
notes/raw-transcript.json
notes/raw-transcript.srt
notes/glossary.md
notes/asr-issues.md
```

### 3. Find Story Windows

Search by keywords and read semantically. Do not trust prop visibility alone.

Create `notes/candidate-windows.md` with:

- source start/end
- topic
- why it matters
- missing context
- emotional/commercial value
- keep/drop recommendation

For requests like “挑 4 条很强的短视频”, start with a candidate pool and selection criteria. Do not pretend the exact clips are known before transcript coverage is sufficient.

### 4. Build The Cut Plan

Use `templates/cut-plan.md` and the gates in `references/story-spec-and-gates.md`.

For 5-10 minute edits, default structure:

1. Hook: strongest claim, conflict, or vivid scene
2. Context repair: who/what this is and why it matters
3. Stakes: difficulty, contradiction, or real-life pressure
4. Turn: moment that changes interpretation
5. Evidence: quote, story, product, original text, or scene
6. Emotional peak: let emotion breathe
7. Restrained close: useful reason to act, not hard sell

Never leave a metaphor without setup, a quote without enough context, or a tearful moment without runway.

### 5. Clean Subtitles

Read `references/subtitle-rules.md`. Use `scripts/subtitle_qa.py` before final render.

Default Chinese subtitle house style:

- correct names, titles, places, pronouns, and domain terms
- remove filler words and stutters that do not carry meaning
- use half-width spaces instead of ordinary Chinese punctuation
- no sentence-final punctuation in final subtitles
- keep visible lines around 16 Chinese characters when possible

### 6. Package Visuals

Read `references/cover-and-overlays.md`.

Use HyperFrames when the video needs HTML/CSS captions, picture-in-picture assets, quote cards, or timed motion graphics. Prefer one final encode.

Visual priorities:

- speaker face and emotion first
- subtitle readability second
- PIP and quote cards third
- sales or CTA last

### 7. Voiceover And Audio

For TTS or voiceover, use StepFun / StepAudio first. Default model: `stepaudio-2.5-tts`. For warm Chinese cultural narration or short-video voiceover, prefer voice `ruyananshi` with speed around `1.12`, then adjust timing with ffmpeg only if needed.

Do not silently fall back to macOS `say`, local TTS, or mini models. If StepFun cannot be used, stop and ask for approval before fallback.

### 8. Version And QA

Read `references/versioning-and-handoff.md`. Use `templates/qa.md`.

Minimum QA:

- `ffprobe`: 1080x1920, 30fps unless source requires otherwise, audio present
- HyperFrames `lint`, `validate`, and `inspect` when applicable
- subtitle QA script and manual scan
- screenshots at opening, context repair, PIP moments, emotional peak, and close
- audio spot check if inserted assets include original sound

### 9. Handoff

Update:

```text
edit-notes.md
versions/version-log.md
qa-vNN-*.md
media/assets/asset-log.md
```

Reviewer feedback should be timecoded. Use `templates/review.md`.

## References

- `references/story-spec-and-gates.md`: director-style gates, question discipline, and iteration impact rules.
- `references/subtitle-rules.md`: subtitle cleanup and QA rules.
- `references/story-editing.md`: story structure and segment decision rules.
- `references/project-structure-and-outputs.md`: exact folder layout, file naming, and delivery package.
- `references/cover-and-overlays.md`: cover, PIP, quote card, and HyperFrames guidance.
- `references/versioning-and-handoff.md`: file layout, versioning, and teammate review.
- `references/liukaixin-case-study.md`: real example distilled from the Liu Kaixin livestream edit.
- `templates/`: copyable project, cut plan, review, QA, version log, and asset log templates.
