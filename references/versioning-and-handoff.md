# Versioning And Handoff

For the full project folder and output contract, read `project-structure-and-outputs.md`.

## Folder Contract

```text
project/
  media/
    raw/
    proxy/
    assets/
  notes/
    raw-transcript.*
    candidate-windows.md
    glossary.md
    asr-issues.md
  frames/
    cover-options/
    qa-checks/
  versions/
    subtitles/
    covers/
    index/
    edit-notes/
  renders/
    versions/
  project.yaml
  cut-plan.md
  subtitles.srt
  edit-notes.md
```

## Version Naming

```text
v01-rough
v02-story
v03-content-subtitle-sync
v04-subtitle-house-style
v05-pip-assets
v06-cover-title
```

Every version should answer:

- What changed?
- Which feedback did it address?
- What did not change and why?
- Which file is the current deliverable?

## Review Format

Reviewer feedback should be timecoded:

```text
00:07 字幕：人名应为“和菜头”
01:20 结构：这里缺人物履历
07:25 画面：不要全屏字卡  让哭的画面保留
10:08 语气：不要太像硬卖
```

The operator converts this into:

- subtitle version if text-only
- composition version if layout/PIP/cover/cut points change
- render version if output changes

## Roles

Producer:

- Defines goal, platform, target duration, audience, and must-keep/must-remove.

AI/video operator:

- Runs ASR, creates cut plan, cleans subtitles, builds HyperFrames composition, renders versions, maintains logs.

Reviewer:

- Watches drafts and gives timecoded feedback.

Designer:

- Provides cover references and approves final cover treatment.
