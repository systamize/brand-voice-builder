# Brand Voice Builder

Brand Voice Builder is a guided setup that produces 2 files — one that captures who you're writing for and what they already believe, one that gives Claude writing defaults, drift controls, and rewrite patterns specific enough that they couldn't describe a different brand without being wrong.

You share existing writing if you have it. Claude reads it and maps the moves before asking anything. Then 8 questions. Then the files.

Install and run /brand-voice-builder in Claude Code.


---

## What it produces

- **`brand-context.md`** — who the brand is, who it serves, what the audience already believes, and what the brand stands for. Claude reads this before drafting anything.
- **`voice.md`** — writing defaults, drift controls, channel handling, language instructions, rewrite patterns, and anti-patterns. The working reference for every piece of copy.

See [`examples/`](./examples) for a sample of what each file looks like.

---

## How it works

The skill runs as a guided consultation — one question at a time, in Claude Code's interactive UI. Three starting paths:

- **Starting from scratch** — no existing writing; go straight to the 8-question setup interview
- **Brand or professional copy** — website, social, email; Claude analyses it before building the system
- **Informal writing** — messages, notes, voice memos; Claude extracts rhythm and voice from it
- **Both** — Claude runs all passes and combines signals before the interview

The process ends with a draft test: Claude writes something new using only the finished files. If the output could have come from any brand, the files get tightened until it can't.

---

## Install

Copy the skill into your project or user-level skills directory:

```bash
# Project-level (applies to one project)
mkdir -p .claude/skills/brand-voice-builder
cp path/to/SKILL.md .claude/skills/brand-voice-builder/SKILL.md

# User-level (applies to all Claude Code sessions)
mkdir -p ~/.claude/skills/brand-voice-builder
cp path/to/SKILL.md ~/.claude/skills/brand-voice-builder/SKILL.md
```

Then in Claude Code, type:

```
/brand-voice-builder
```

---

## Use

Once the two output files exist, add them to your project root (or wherever you keep Claude's reference files). Before any writing task, instruct Claude to read both:

```
Read brand-context.md and voice.md before drafting.
```

Or reference them in your project's `CLAUDE.md` so Claude reads them automatically.

---

## When to use

- A brand has never documented how Claude should write for it
- Claude's drafts are close but still generic
- Copy exists across channels but there's no consistent thread
- A writer, agency, or AI tool needs concrete writing instructions rather than loose adjectives
- The brand has evolved and the writing system has not caught up
- Existing copy feels off and no one can articulate why

## When not to use

- Writing specific campaign copy — this skill builds the reference system; a copywriting skill applies it
- Visual identity — colour, typography, logo
- Full brand strategy and positioning — that needs to come before this skill runs
