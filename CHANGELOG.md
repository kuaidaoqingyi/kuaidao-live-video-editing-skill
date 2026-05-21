# Changelog

All notable changes to this skill are tracked here.

## [0.3.0] - 2026-05-21

### Added

- Added Kuaidao naming across the skill, README, usage examples, and install path.
- Added director-style story gates in `references/story-spec-and-gates.md`.
- Added startup mode checks for new edits, continuing edits, iteration work, and single-surface tasks.
- Added iteration impact levels for light, medium, and heavy review changes.
- Expanded `templates/cut-plan.md` with a gate check, executable storyboard timeline, and iteration notes.
- Expanded `templates/project.yaml` with planning, iteration, and StepFun/StepAudio TTS defaults.

### Changed

- Updated ASR and TTS guidance to prefer StepFun / StepAudio first.
- Updated the public skill id to `kuaidao-live-video-editing`.
- Updated current version marker to `0.3.0`.

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

- Initial live video editing Skill.
- Core workflow for Chinese livestream/interview/course short-video editing.
- References for subtitle cleanup, story editing, cover/overlays, versioning, and Liu Kaixin case study.
- Templates for `project.yaml`, cut plan, review, QA, version log, and asset log.
- `scripts/subtitle_qa.py` for lightweight Chinese SRT QA.
