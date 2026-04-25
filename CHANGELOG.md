# Changelog

All notable changes to TTS Tutor are documented in this file.

This project follows [Common Changelog](https://common-changelog.org/) and [Semantic Versioning](https://semver.org/).

## [1.1.0] - 2026-04-25

### Added

- Calibration step before generation. Asks the listener for background level, tutor-question preference, re-explanation depth, and length target. Skips when the user has already specified preferences in their request or asked for something quick.
- `Proper nouns in audio` rule under TTS formatting. Collapses dense names that read as a database dump aloud while keeping names the listener will use later.
- `Symbols and notation in audio` rule under TTS formatting. Covers equations, code, regex, file paths, command-line invocations, URLs, hashes, and chemical formulas; describes the shape in plain language and points to the source for the exact form.
- `Internal Versus Reader-Facing Content` section. Bans rubric shorthand and evaluation criteria from any user-facing brief.
- Stakes-first opening rule. Requires an opening that lands for someone outside the writer's technical context.
- Per-topic problem framing rule. Requires establishing the problem in non-builder language before describing what was tested or built.
- Quality Check items covering all of the above.
- `version` field in `SKILL.md` frontmatter.

### Changed

- Topic introduction guidance rewritten to require non-builder problem framing before method or results.
- Overall guide opening guidance rewritten to require stakes the listener can place themselves inside, with an explicit warning against vivid examples that need shared technical context.
- Practical next steps now match the calibrated background level.

## [1.0.0] - 2026-04-08

### Added

- Initial public release of the TTS Tutor skill.
- `SKILL.md` with YAML frontmatter, structured teaching methodology, TTS-optimised formatting rules, and tutor-style question and answer cadence.
- Research and source-handling rules covering search-available, search-unavailable, and user-supplied-material modes.
- `README.md` with install paths for Claude.ai and Claude Code, example prompts, and expected output behaviour.
- `examples/example-output.md` demonstrating the full guide structure.
- Packaged `tts-tutor-skill.skill` archive for distribution.
- MIT licence.
