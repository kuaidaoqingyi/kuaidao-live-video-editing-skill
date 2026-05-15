# Changelog

All notable changes to this skill are tracked here.

## [0.2.1] - 2026-05-15

### Added

- Added explicit reminder that users must apply for and configure an ASR API key before generating subtitles.
- Added `api_key_env` and `api_doc` fields to `templates/project.yaml`.
- Added Stepfun/StepAudio documentation pointer as an example ASR provider.

### Changed

- Updated `SKILL.md` to stop before transcription if no ASR key is configured.

## [0.2.0] - 2026-05-15

### Added

- Added GitHub-facing `README.md` with implemented capabilities, usage, delivery contract, and version policy.
- Added `VERSION` file as the single current version marker.
- Added explicit version management process for future updates.
- Added project folder and output contract in `references/project-structure-and-outputs.md`.

### Changed

- Updated `SKILL.md` to point agents to the project structure and output contract before ingest and delivery.
- Clarified that `final.mp4` and `cover.jpg` are convenience copies, not archived delivery versions.

## [0.1.0] - 2026-05-15

### Added

- Initial `dedao-live-video-editing` Skill.
- Core workflow for Chinese livestream/interview/course short-video editing.
- References for subtitle cleanup, story editing, cover/overlays, versioning, and Liu Kaixin case study.
- Templates for `project.yaml`, cut plan, review, QA, version log, and asset log.
- `scripts/subtitle_qa.py` for lightweight Chinese SRT QA.
