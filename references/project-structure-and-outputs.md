# Project Structure And Outputs

This skill has two separate scopes:

- Skill repository: the reusable workflow itself. It must contain only instructions, templates, and small scripts.
- Video project folder: one concrete editing project with source links, transcripts, assets, renders, and review files.

Do not mix them. Never commit raw videos, rendered MP4s, API keys, or user assets into the skill repository.

## Recommended Video Project Location

Create one folder per video project in the user's working area:

```text
YYYYMMDD-topic-slug/
```

Examples:

```text
20260514-tbhlive-liukaixin-books/
20260515-live-ai-workflow-clips/
20260601-interview-guest-name/
```

If the user has a dedicated `Work/08-短视频/...` directory, keep final delivery there. If Codex needs a writable workspace copy, keep the source-of-truth files synchronized and document the delivery path in `project.yaml`.

## Project Folder Contract

```text
project/
  media/
    raw/                         # symlink or metadata only; do not copy huge source videos
      raw.mp4                    # preferred symlink name when possible
    proxy/                       # extracted audio, low-res proxies, ASR chunks
    assets/                      # user-provided photos, book covers, B-roll, screenshots
      asset-log.md               # source, usage, time range, placement, audio
  notes/
    glossary.md                  # canonical names, aliases, pronouns, known ASR errors
    raw-transcript.json
    raw-transcript.srt
    candidate-windows.md
    asr-issues.md
  frames/
    cover-options/
      v01-topic/
        contact-sheet.jpg
        option-01.jpg
    qa-checks/
      vNN/
        opening.jpg
        context.jpg
        emotional-peak.jpg
        ending.jpg
  versions/
    subtitles/
      subtitles-vNN-slug.srt
    covers/
      cover-vNN-slug.jpg
    index/
      index-vNN-slug.html
    edit-notes/
      edit-notes-vNN-slug.md
    qa/
      qa-vNN-slug.md
    version-log.md
  renders/
    versions/
      final-vNN-slug.mp4
    current/
      final.mp4                  # optional convenience copy of latest approved render
      cover.jpg                  # optional convenience copy of latest approved cover
  project.yaml
  cut-plan.md
  subtitles.srt                  # current working subtitle
  edit-notes.md                  # current working notes
```

## File Roles

`project.yaml`:

- project identity, source path, delivery path, glossary, ASR provider, latest version, QA status.

`cut-plan.md`:

- story arc, candidate windows, EDL, and rationale before rendering.

`subtitles.srt`:

- current working subtitle. It may be overwritten while editing, but every reviewed subtitle must also be copied into `versions/subtitles/`.

`edit-notes.md`:

- current working explanation of source timecodes, structure, changes, and tradeoffs.

`asset-log.md`:

- every inserted image/video/audio asset, where it came from, where it appears, whether it has sound, and whether it may be shared.

`version-log.md`:

- what changed in every draft and which one is current.

## Output Contract

Every reviewed or delivered version should have a matching set:

```text
renders/versions/final-vNN-slug.mp4
versions/subtitles/subtitles-vNN-slug.srt
versions/covers/cover-vNN-slug.jpg
versions/edit-notes/edit-notes-vNN-slug.md
versions/qa/qa-vNN-slug.md
```

If HyperFrames is used, also keep:

```text
versions/index/index-vNN-slug.html
```

The root-level or `renders/current/` files are only convenience copies for the latest approved version. They are not the archive.

## Draft Vs Final Output

Draft output:

- lower render quality is acceptable
- watermark-free
- must have synced subtitles
- must include enough context for review
- should be named `final-vNN-draft-slug.mp4` if it leaves the operator

Final output:

- 1080x1920 unless the platform asks otherwise
- 30fps unless the source or platform requires otherwise
- AAC audio present
- burned Chinese subtitles
- final cover image
- QA file completed
- exact version recorded in `project.yaml`

## Delivery Package

For handoff to the user or teammate, provide:

```text
final-vNN-slug.mp4
cover-vNN-slug.jpg
subtitles-vNN-slug.srt
edit-notes-vNN-slug.md
qa-vNN-slug.md
project.yaml
```

For editable continuation by another operator, also provide:

```text
cut-plan.md
notes/glossary.md
notes/candidate-windows.md
media/assets/asset-log.md
versions/version-log.md
versions/index/index-vNN-slug.html
```

## Naming Rules

- Use `vNN` for every visible draft or user-reviewed output.
- Use a short English slug after the version: `v05-pip-assets`, `v06-cover-title`.
- Do not overwrite any file already shown to the user.
- Do not name a reviewed file only `final.mp4`; keep `final.mp4` as a convenience copy only.
- Keep raw source paths in `project.yaml` instead of copying source media into the project.

