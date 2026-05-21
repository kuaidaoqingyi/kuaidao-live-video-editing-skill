# Story Spec And Gates

This document borrows the useful discipline of a video-spec workflow, but keeps Kuaidao focused on real long-video editing: source video, transcript, cut plan, subtitles, visual packaging, QA, and handoff.

## Principle

Do not let vague taste words enter the cut plan.

Reject answers like:

- 高级一点
- 牛逼一点
- 节奏好一点
- 看着办
- 适合所有人
- 先剪出来再说

Translate them into concrete decisions:

- which viewer is this for
- what the opening 10 seconds promises
- what clip earns attention
- what context must be repaired
- where subtitles, PIP, cover text, and silence should go
- what the viewer should feel or do after the scene

## Gate 1: Basic Brief

A cut plan is not ready until these are explicit or inferable from existing files:

| Field | Acceptable answer | Not acceptable |
| --- | --- | --- |
| Purpose | Let a specific viewer remember/do one thing | 想火 / 做内容沉淀 |
| Audience | Concrete role + viewing context | 年轻人 / 所有人 |
| Platform | Douyin, video account, Xiaohongshu, Bilibili, internal review, etc. | 多平台都发 without a primary |
| Target duration | A range or exact goal, such as 60-90s or 5-10min | 看素材定 |
| Core information | One sentence the viewer should retain | Several unrelated takeaways |
| Cold-viewer promise | Why a viewer who missed the livestream should care | 熟人自然懂 |

If only one or two fields are missing, ask only those. If most fields are missing, start by asking the user to describe the video goal and the intended viewer.

## Gate 2: Source And Material State

Before selecting clips, record:

- original source path and duration
- transcript coverage and ASR provider
- glossary status
- whether candidate windows cover the full source or only a partial range
- required people, books, screenshots, B-roll, photos, or product assets
- known rights or usage constraints for inserted materials

If transcript coverage is partial, state the coverage boundary in `candidate-windows.md` and do not claim full-video selection is complete.

## Gate 3: Candidate Pool

For requests like “剪 4 条很强的短视频”, create a candidate pool before promising final clips.

Each candidate needs:

- source start/end
- topic
- hook line or scene
- missing context
- emotional or practical payoff
- platform fit
- risks, such as weak opening, missing setup, or hard-to-subtitle sections
- recommendation: keep / maybe / drop

A strong candidate must be understandable without the original livestream and must contain either a clear claim, vivid scene, emotional beat, proof, conflict, or practical payoff.

## Gate 4: Story Arc

For a selected edit, every retained segment must serve one role:

- hook
- context repair
- stakes
- turn
- evidence
- emotional peak
- close
- bridge, only when it genuinely carries transition context

Bridge segments should be rare. If a bridge has no information load, cut it.

## Gate 5: Visual And Subtitle Plan

Before rendering, each output segment should specify:

- subtitle treatment: ordinary subtitle, keyword highlight, quote card, or no subtitle
- PIP/overlay treatment and where it appears
- face-protection rule when emotion matters
- cover candidate frame or cover title direction if relevant
- silence, music, original audio, or SFX note
- QA screenshot point if this segment is visually risky

For emotional sections, protect eyes and mouth. Do not cover a tearful or paused face with full-screen text unless the user explicitly asks.

## Gate 6: References And Anti-Examples

Ask for references when style matters. If no reference exists, write style defaults from the project and mark the absence.

At minimum, record:

- one positive reference or local project pattern
- at least one anti-example, such as hard sell, template-feel captions, cluttered PIP, or full-screen text covering faces
- whether the cover should feel person-first, object-first, quote-first, or title-first

## Iteration Impact Rules

Light iteration:

- subtitle wording
- typo/name correction
- cover title text
- color or density tweak
- tiny timing adjustment

Update the smallest file and version note.

Medium iteration:

- add/drop/reorder a segment
- replace PIP or B-roll
- move subtitle area
- change cover frame
- change music, silence, SFX, or original audio mix

Update `cut-plan.md`, `edit-notes.md`, and the next version-log entry.

Heavy iteration:

- core topic changes
- target viewer changes
- platform or duration changes
- emotional peak changes
- story arc changes

First summarize the affected scope: changed promise, changed segments, expected duration impact, assets affected, and QA that must be rerun.

## Cut Plan Readiness Checklist

Before render or final subtitle cleanup:

```text
[ ] purpose, audience, platform, and target duration are explicit
[ ] source path and transcript coverage are recorded
[ ] glossary and known ASR risks are recorded
[ ] candidate windows have keep/drop decisions
[ ] selected segments have story roles
[ ] output timeline has source and output timecodes
[ ] subtitle/PIP/cover/audio notes are attached to risky scenes
[ ] must-keep and must-remove constraints are visible
[ ] open questions are marked instead of invented
```
