# TTS Tutor Skill

A Claude skill that generates high-quality learning guides optimised for text-to-speech consumption.

## What it does

TTS Tutor produces structured, listen-friendly learning guides on any topic. Every output is designed to sound natural when read aloud by a TTS engine (Speechify, Apple VoiceOver, NaturalReader, etc.) while also reading well on screen.

The skill encodes a tested teaching methodology refined across 50+ learning sessions. It is not generic lesson generation. It combines three things:

1. **TTS-optimised formatting.** Short paragraphs (max three sentences), headings with full stops, clear sentence structure. Every output sounds natural when read aloud.

2. **Structured teaching methodology.** Define-then-explain for terms, multiple teaching techniques per guide (analogy, contrast, common mistakes, building simple-to-complex), tutor-style Q&A checkpoints, and learning takeaways.

3. **Accessibility as a design principle.** Built for how people actually consume audio learning material. One idea per paragraph. Important terms repeated after gaps. Varied pacing so guides stay engaging.

## Who it's for

Anyone who learns by listening. That includes people with ADHD, dyslexia, or visual impairments, but also commuters, runners, and anyone who prefers audio learning material.

## Install

### Claude.ai

1. Download the latest `.skill` file from [Releases](../../releases).
2. Open Claude.ai and go to **Customize > Skills**.
3. Upload the `.skill` file.
4. Enable the skill.

### Claude Code

Clone the repo and copy the skill file into your project or personal skills directory:

```bash
# Project-level (available in one project)
mkdir -p .claude/skills/tts-tutor-skill
cp path/to/SKILL.md .claude/skills/tts-tutor-skill/SKILL.md

# Personal (available across all projects)
mkdir -p ~/.claude/skills/tts-tutor-skill
cp path/to/SKILL.md ~/.claude/skills/tts-tutor-skill/SKILL.md
```

## Usage

Ask Claude to create a learning guide on any topic. The skill triggers automatically when it detects requests for learning guides, lessons, study material, or TTS-friendly content.

### Example prompts

```
Create a learning guide on how transformer attention mechanisms work.
```

```
Write a revision guide on Python async and await, focusing on practical patterns.
```

```
Teach me about DNS resolution. Focus on official documentation as sources.
```

```
Create a listen-friendly guide on Kubernetes networking for someone who
already understands Docker networking.
```

### Inputs

The skill responds to more than just a topic name. You can control:

- **Topic scope.** Be as broad or specific as you like. "Teach me about DNS" and "Teach me about DNS recursive resolution specifically" produce different guides.
- **Source preferences.** State what sources you want emphasised, e.g. "focus on official documentation" or "include community and practitioner sources." If you say nothing, the skill favours official documentation but includes community sources where they strengthen the teaching.
- **Your own materials.** Paste text, upload documents, or provide links. The skill will synthesise from what you provide rather than searching or generating from its own knowledge.

### Research behaviour

The skill checks whether web search is available before writing.

- **Search available.** The skill searches for reputable sources (official docs, engineering blogs, academic papers, recognised publications) and grounds the guide in researched material. Sources are cited at the end.
- **Search not available.** The skill generates from its own knowledge and adds a disclaimer noting the output has not been cross-referenced with external sources.
- **Materials provided.** If you provide source material directly, the skill uses it regardless of whether search is available.

### What to expect

Each guide includes:

- An overview of what the guide covers and why it matters.
- Teaching sections with defined terms, analogies, common mistakes, and tutor notes.
- Tutor-style Q&A checkpoints after every two major topics.
- A learning takeaway at the end of each section.
- A conclusion linking the main ideas together.
- Practical next steps you can do independently.
- Sources used (when web search is available).

Paragraphs never exceed three sentences. Acronyms are expanded on first use and repeated after gaps. The output is ready to paste into any TTS app.

## Example output

See [`examples/`](examples/) for a complete example guide.

## License

[MIT](LICENSE)
