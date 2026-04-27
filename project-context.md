# Project Context

## Product Summary

TTS Tutor is a Claude skill that generates learning guides optimised for text-to-speech consumption.
The skill is distributed as a single `SKILL.md` file and as a packaged `.skill` archive for upload to Claude.ai.
The audience is anyone who learns by listening, including people with ADHD, dyslexia, visual impairments, commuters, and runners.
The skill is invoked when a user asks Claude for a learning guide, lesson, study material, revision guide, teaching material, or audio-friendly content on any topic.
The skill produces a structured guide with TTS-friendly formatting, defined teaching techniques, tutor-style questions, learning takeaways, and practical next steps.

## Domain Concepts

A skill is a markdown instruction file with YAML frontmatter consumed by a Claude harness at runtime.
A learning guide is the structured output the skill produces in response to a user topic.
A topic is the user-supplied subject the guide teaches.
Calibration is a pre-generation question set that adapts the guide to the listener's background, depth preference, length target, and tutor-question preference.
A teaching technique is a labelled pedagogical move the guide uses, such as analogy, contrast, common mistakes, or building from simple to complex.
A tutor-style question is a short prompt-and-answer block inserted after every two major topics to reinforce learning.
A TTS engine is the downstream consumer of the guide, for example Speechify, Apple VoiceOver, or NaturalReader.

## Scope

The skill owns the pedagogy, output structure, TTS formatting rules, and listener calibration.
The skill does not own topic selection, source curation, or research scope; the user supplies those.
The skill checks whether web search is available at runtime and grounds the guide in researched sources when it is.
The skill generates from its own knowledge with a disclaimer when search is unavailable.
The skill synthesises from user-provided text, documents, or links when supplied, regardless of search availability.
Scripts, executables, and bundled code resources are out of scope; the skill is a pure-instruction file.
Publishing to the Claude.ai skill marketplace is out of scope for the current version.

## Important Constraints

`SKILL.md` must stay well under 500 lines per the project spec.
`project-context.md` must stay under 300 lines per the context-management skill.
Paragraphs in generated guides must not exceed three sentences.
Personal context, named individuals, named institutions, named conditions, and specific app names must not appear in `SKILL.md`.
Direct commits and direct pushes to `main` and `master` are blocked by `.ai-policy/hooks/block-protected-branch-bash.sh` and the equivalent MCP hook.
Pre-commit and pre-push hooks require `.ai-policy/state/validation.status` to read `passed` before allowing the action.
The pre-push hook rejects any push that bumps the `Version:` header in `ai-workflow.md` without a matching `## <version>` entry in `CHANGELOG.md`.
Git `core.hooksPath` must be set to `.githooks` for the policy layer to enforce.
Every implementation task must follow `ai-workflow.md`, which requires a linked GitHub issue, an issue-scoped branch, an approved plan, and explicit human approval before each remote GitHub action.

## Architecture Summary

The repository ships a single instruction artefact rather than a running application.
The runtime layer is the Claude harness (Claude.ai, Claude Code, or API) that loads `SKILL.md` and acts on its instructions when the description trigger matches a user request.
The distribution layer is `tts-tutor-skill.skill`, a zip archive built from the repository for upload into harnesses that accept packaged skills.
The data flow at runtime is: user prompt → harness matches the skill description → harness loads `SKILL.md` → Claude follows its instructions to produce the guide.
The repository also installs an external policy layer under `.ai-policy/` that enforces branch protection, validation state, and changelog discipline through git hooks and per-harness hook configurations.

## Key Dependencies

The skill has no runtime code dependencies because it is a markdown instruction file.
Git, bash, and the `gh` CLI are used by the workflow scripts under `.ai-policy/scripts/` and by the `.githooks/` hooks.
The Agent Skills open standard defines the `SKILL.md` frontmatter and loading contract.

## Project Structure

`SKILL.md` is the primary skill instruction file with YAML frontmatter (including a `metadata.version` field) and the full pedagogy, formatting, and calibration rules.
`README.md` documents what the skill does, install paths for Claude.ai and Claude Code, example prompts, and what to expect from the output.
`CHANGELOG.md` records skill versions in Common Changelog format and is the canonical source of the current version number alongside the `SKILL.md` frontmatter.
`LICENSE` is the MIT licence for the skill.
`examples/example-output.md` is a single complete guide demonstrating the format the skill produces.
`tts-tutor-skill.skill` is the packaged distribution archive built from the repository.
`tts-tutor-skill-spec.md` records the original product spec, milestones, and success criteria for the first public release.
`CLAUDE.md` is the Claude Code entry file and references `ai-workflow.md` and this `project-context.md`.
`ai-workflow.md` defines the mandated implementation loop, planning, validation, and GitHub-action approval rules for AI-assisted work in the repo.
`.github/copilot-instructions.md` directs VS Code Copilot to read `ai-workflow.md` and `project-context.md` before responding.
`.ai-policy/policy.env` declares protected branches, validation requirements, and the validation command.
`.ai-policy/scripts/project-validation.sh` runs the policy-layer validation: shell-script syntax checks plus per-harness enforcement tests for whichever agent directories exist.
`.ai-policy/scripts/install-hooks.sh` installs `.githooks` as the active git hooks path.
`.ai-policy/hooks/` contains the protected-branch and changelog hooks invoked by the harnesses' tool-use hook configurations.
`.ai-policy/state/validation.status` records the most recent validation result (`passed` or `failed`) consulted by the pre-commit and pre-push hooks.
`.githooks/pre-commit` enforces the protected-branch check and the validation-state check before allowing a commit.
`.githooks/pre-push` enforces the protected-branch check, validation-state check, and changelog-version check before allowing a push.
`.claude/settings.json` configures Claude Code permission allowlists and routes git-commit and git-push tool calls plus GitHub MCP calls through the protected-branch hooks.
`.claude/settings.local.json` holds developer-local Claude Code settings and is gitignored.
`.codex/config.toml` and `.codex/hooks.json` configure the Codex CLI sandbox policy and route shell and MCP calls through the same protected-branch hooks.
`.gemini/settings.json` configures the Gemini CLI tool permissions and protected-branch hooks.
`.github/hooks/block-protected-branch.json` configures VS Code Copilot's protected-branch enforcement.
`.agents/skills/` and `.claude/skills/` each contain the same eight `aiw-*` helper skills referenced by `ai-workflow.md` (planning, testing, failure-analysis, issue-creation, logging, performance-profiling, telemetry-setup, project-context-management).
`.vscode/settings.json` holds editor settings.

## Testing Overview

The project has no application test framework because the deliverable is an instruction file rather than executable code.
Validation is `./.ai-policy/scripts/project-validation.sh`, which runs `bash -n` syntax checks on every script under `.ai-policy/scripts/`, `.ai-policy/hooks/`, and `.githooks/`, then runs each `test-*-enforcement.sh` script for harness directories that exist in the repo.
Validation state is written to `.ai-policy/state/validation.status` and read by the commit and push hooks.
There is no automated quality check for `SKILL.md` content; skill output quality is judged by human review against `examples/example-output.md` and the Quality Check section of `SKILL.md`.
The repository runs no GitHub Actions or external CI; validation is enforced locally through git hooks.

## Maintenance Checklist

Update this file when `SKILL.md` gains, loses, or restructures a top-level section.
Update this file when a new agent harness directory is added or removed at the repo root (for example `.cursor/`, `.continue/`).
Update this file when the policy layer under `.ai-policy/` changes its commands, hooks, validation flow, or state contract.
Update this file when a new top-level file or directory is added to the repo.
Update this file when the distribution method or `.skill` packaging process changes.
Update this file when calibration questions, teaching methodology, or TTS formatting rules in `SKILL.md` change in a way that alters how the skill behaves.
Update this file when the `ai-workflow.md` workflow changes the steps, checkpoints, or required artefacts.
Update this file and `CHANGELOG.md` together when the `version` field in `SKILL.md` is bumped.
