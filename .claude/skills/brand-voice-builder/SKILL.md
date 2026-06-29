---
name: brand-voice-builder
description: Create the two files Claude needs before drafting for a brand: brand-context.md and voice.md. Use when a brand needs reusable writing instructions for Claude, a practical voice setup for AI-assisted writing, an audit of existing copy, or a clearer system for avoiding generic AI output. Triggers on: brand voice, voice and tone, tone of voice, voice document, voice guidelines, writing style, copy audit, voice training, how should we write, sounds like us, Claude writing style, AI writing instructions, brand context. Also triggers when the user says copy "feels off" or Claude's drafts "don't sound right" even without using the word voice.
---

# Brand Voice Builder

A guided setup that gives Claude the context and writing rules it needs before drafting for a brand.

The process produces two files:

- `brand-context.md` — the foundation document that captures who the brand is, who it serves, what the audience already believes, and what the brand stands for
- `voice.md` — the working reference Claude reads before writing, with writing defaults, drift controls, channel handling, language instructions, rewrite patterns, and anti-patterns

Voice without context is decoration. `brand-context.md` comes first.

---

## When to use

- A brand has never documented how Claude should write for it
- Claude's drafts are close but still generic
- Copy exists across channels but there is no consistent thread between them
- A writer, agency, or AI tool needs concrete writing instructions rather than loose adjectives
- The brand has evolved — new audience, new positioning — and the writing system has not caught up
- Existing copy feels off and no one can articulate why

## When NOT to use

- Writing specific campaign copy — this skill builds the reference system; a copywriting skill applies it
- Visual identity — colour, typography, logo — that is a separate system
- Full brand strategy and positioning — that needs to come before this skill runs
- A complete brand style guide — `voice.md` can feed into one, but this skill covers the writing layer only

---

## Consultation model

This skill runs as a guided setup. Every user-facing question is delivered one at a time using `AskUserQuestion` — never listed in plain text. The user selects from options and moves forward. Claude does not ask multiple questions in the same message.

Three starting paths — which can combine:

**Path A — Starting from scratch.** No existing writing. Go straight from source check to the setup interview.

**Path B — Brand or professional copy.** Website copy, social posts, LinkedIn, emails written for an audience. Claude runs the Copy Evidence Pass and Signal Map. After confirming the read, asks if the user also has informal writing to add.

**Path C — Informal writing.** Personal messages, emails, notes, voice memos, anything not written for an audience. Claude runs the Natural Voice Pass and Rhythm Extraction Map. After confirming the read, asks if the user also has professional or brand copy to add.

**Path B+C — Combined sources.** Both types are provided. Claude runs all four passes across all samples, then runs a Combined Synthesis before the interview. This is the richest input — rhythm and structure signals can come from either source type. Do not assume that professional copy yields only structure signals or that informal writing yields only rhythm signals. Extract both from both.

If the user is unsure which path applies, ask them to paste any writing they have. If they paste something, judge whether it reads as written for a brand audience (Path B) or written informally for themselves or someone they know (Path C). After the first pass is confirmed, always ask if they have the other type.

---

## Question delivery

All user-facing questions use the `AskUserQuestion` tool — one call per question. Use the `question`, `header`, `options`, and `multiSelect` fields from the JSON below directly as tool parameters. The `id`, `step`, and `order` fields are for reference only.

```json
{
  "consultation_questions": [
    {
      "id": "source_check",
      "step": "pre-interview",
      "question": "Do you have existing writing Claude can learn from?",
      "header": "Starting point",
      "options": [
        { "label": "I have brand copy to share", "description": "Website copy, social posts, emails, or anything written for an audience — Claude will analyse it before building the writing system" },
        { "label": "I have informal writing to share", "description": "Personal messages, emails to friends, notes, or anything written without a brand audience in mind — Claude will distil the rhythm and voice from it" },
        { "label": "I have both", "description": "Mix of brand copy and informal writing — Claude will run separate passes on each and combine the rhythm and structure signals before building the system" },
        { "label": "Starting fresh", "description": "No existing writing, or nothing useful enough to learn from — we build from the setup interview" }
      ]
    },
    {
      "id": "default_stance",
      "step": "interview",
      "order": 1,
      "question": "(1 of 8) How should your writing come across for this brand?",
      "header": "Default stance",
      "options": [
        { "label": "Warm and useful", "description": "Helpful, human, and direct — the reader feels looked after without being over-managed" },
        { "label": "Sharp and direct", "description": "No fluff, respects the reader's time, gets to the point" },
        { "label": "Detailed and precise", "description": "Goes deep where it matters, earns trust through specifics" },
        { "label": "Opinionated and confident", "description": "Has a clear point of view and does not hide behind neutral language" }
      ]
    },
    {
      "id": "audience",
      "step": "interview",
      "order": 2,
      "question": "(2 of 8) Who your main audience?",
      "header": "Reader",
      "options": [
        { "label": "Founders and entrepreneurs", "description": "Running their own business, time-poor, results-driven" },
        { "label": "Professionals in organisations", "description": "Inside larger companies, navigating teams and process" },
        { "label": "Creative or technical specialists", "description": "Deep domain knowledge, high standards, spot jargon immediately" },
        { "label": "Mixed audience", "description": "The brand writes for more than one distinct group" }
      ]
    },
    {
      "id": "generic_output_risk",
      "step": "interview",
      "order": 3,
      "question": "(3 of 8) What kind of generic output should be avoided most?",
      "header": "Drift risk",
      "options": [
        { "label": "Corporate and stiff", "description": "Reads like a press release or enterprise landing page" },
        { "label": "Over-written", "description": "Too much personality, too many flourishes, exhausting to read" },
        { "label": "Vague and forgettable", "description": "Could have come from any brand in the category" },
        { "label": "Too casual", "description": "Loses credibility by sounding unserious" }
      ]
    },
    {
      "id": "reference_style",
      "step": "interview",
      "order": 4,
      "question": "(4 of 8) Which reference style is closest to aim for?",
      "header": "Reference style",
      "options": [
        { "label": "Confident and minimal", "description": "Precise, no excess, every word earns its place" },
        { "label": "Warm and human", "description": "Approachable, personal, sounds like a person rather than a brand committee" },
        { "label": "Witty but controlled", "description": "Dry, relaxed, memorable — never joke-first" },
        { "label": "Sharp and authoritative", "description": "Expert, no-nonsense, high standards" }
      ]
    },
    {
      "id": "differentiation",
      "step": "interview",
      "order": 5,
      "question": "(5 of 8) How should your writing differ from competitors?",
      "header": "Difference",
      "options": [
        { "label": "Simplifies complexity", "description": "Makes hard things feel approachable and clear" },
        { "label": "Sounds more human", "description": "Less corporate, more like a person having a real conversation" },
        { "label": "Goes deeper", "description": "More expertise, more substance, more rigour than anyone else" },
        { "label": "Gets there faster", "description": "Less setup, more useful substance" }
      ]
    },
    {
      "id": "off_limits",
      "step": "interview",
      "order": 6,
      "multiSelect": true,
      "question": "(6 of 8) What should your brand's writing never do? Select all that apply.",
      "header": "Off-limits",
      "options": [
        { "label": "Use humour", "description": "The brand stays serious — humour undermines trust here" },
        { "label": "Touch controversy", "description": "No political, social, or polarising topics" },
        { "label": "Use jargon or buzzwords", "description": "Plain language only — no industry filler or corporate speak" },
        { "label": "Use pushy sales language", "description": "No pressure tactics, manufactured urgency, or hard sell" }
      ]
    },
    {
      "id": "reader_reaction",
      "step": "interview",
      "order": 7,
      "question": "(7 of 8) What should the reader feel quickly when the writing lands?",
      "header": "Reader reaction",
      "options": [
        { "label": "Understood", "description": "Like the brand is speaking directly to their situation" },
        { "label": "Ready to act", "description": "The next step feels clear and useful" },
        { "label": "Confident", "description": "They made a smart choice paying attention" },
        { "label": "Safe", "description": "They are in good hands — someone reliable and credible is speaking" }
      ]
    },
    {
      "id": "address_style",
      "step": "interview",
      "order": 8,
      "question": "(8 of 8) When writing to the audience, address them as:",
      "header": "Address style",
      "options": [
        { "label": "One person — \"you\"", "description": "Direct, personal, individual address throughout" },
        { "label": "A team — \"your team\"", "description": "Group-oriented, collective framing" },
        { "label": "Varies by context", "description": "Different formats call for different address styles" },
        { "label": "Undecided", "description": "This has not been locked down yet" }
      ]
    },
    {
      "id": "synthesis_confirmation",
      "step": "post-synthesis",
      "question": "How accurate is this combined synthesis of your writing?",
      "header": "Synthesis check",
      "options": [
        { "label": "Spot on", "description": "That matches how I think about the writing across both sources" },
        { "label": "Mostly right", "description": "Close, but I would adjust a few things — I will explain" },
        { "label": "Partially off", "description": "Some of it resonates, some does not — let me clarify" },
        { "label": "Off the mark", "description": "That is not how I would describe it — let me correct it" }
      ]
    },
    {
      "id": "reader_context_confirmation",
      "step": "post-reader-context",
      "question": "Does this reader picture match who you're actually writing for?",
      "header": "Reader check",
      "options": [
        { "label": "Yes, that's them", "description": "The description captures the reader accurately" },
        { "label": "Mostly, with nuance", "description": "Right overall, but I would add or adjust something — I will explain" },
        { "label": "Partially off", "description": "Some of it is right, some is not — let me clarify" },
        { "label": "No, not quite", "description": "The picture needs correcting — let me explain" }
      ]
    },
    {
      "id": "evidence_confirmation",
      "step": "post-evidence",
      "question": "Based on the writing you shared, how accurate is this read?",
      "header": "Evidence check",
      "options": [
        { "label": "Spot on", "description": "That matches how I feel about the writing" },
        { "label": "Mostly right", "description": "Close, but I would adjust a few things — I will explain" },
        { "label": "Partially off", "description": "Some of it resonates, some does not — let me clarify" },
        { "label": "Off the mark", "description": "That is not how I would describe it — let me correct it" }
      ]
    },
    {
      "id": "has_other_source",
      "step": "post-evidence",
      "question": "Do you have [other type] writing to add? Combining both gives a fuller picture — rhythm and structure signals can come from either source.",
      "header": "Add more writing",
      "options": [
        { "label": "Yes, I have more to share", "description": "Paste it after selecting — Claude will run a second pass and combine the findings before the interview" },
        { "label": "No, let's continue", "description": "Work with what's here" }
      ]
    }
  ]
}
```

---

## Workflow

### Opening overview

Before asking anything, output this in chat:

> Here's what we'll cover:
> 1. **Source** — share existing writing (brand copy, informal, or both) so Claude can analyse rhythm, structure, and voice before building anything
> 2. **Interview** — 8 questions about your reader, tone, and limits
> 3. **Build** — writing defaults, drift controls, channel handling, and rewrite patterns, written into two files
> 4. **Test** — draft something real to confirm the files work

Then immediately use `AskUserQuestion` with question `source_check`.

---

### Step 1: Copy Evidence Pass (Paths B and B+C)

Read every sample before writing anything. For each piece:

- Mark what proves how the brand already writes well — specific phrases, rhythms, structures, or word choices worth preserving
- Flag where the copy drifts — generic phrasing, tonal inconsistency, vague claims, over-polished language, moments that could have come from anyone
- Note the pressure defaults — what the writing reaches for when the topic gets complex, technical, commercial, or sensitive
- Note where the rhythm is **alive** — where sentence length varies, where energy shifts, where a move lands unexpectedly. These are as valuable as content signals
- Note where the rhythm **flattens** — three or more consecutive sentences of the same length and structure. This is where the writing loses its human feel. Flag it explicitly

Share observations in chat, then use question `evidence_confirmation`. Do not rewrite anything yet.

After confirmation, use question `has_other_source` — replacing "[other type]" with "informal personal writing (messages, emails, notes)". If yes, run Step 1C before proceeding to Step 2.

---

### Step 1C: Natural Voice Pass (Paths C and B+C)

Read all samples before writing anything. These were not written for a brand audience — they are unfiltered writing from which the voice will be distilled. The goal is not to find the dominant pattern. It is to map the full range of moves this writer makes and understand what triggers each one.

What makes informal writing feel human is variation — sentence length shifts, energy rises and falls, the rhythm breathes. Extract the range, not the average.

For each sample, observe:

- **The full range of moves** — what are all the different things this writer does at the sentence level? Short bursts, long flowing lists, fragments, flat observations, pivots, reactions. Document every distinct move that appears, not just the most common one
- **Triggers for each move** — what causes the writing to shift? Excitement lengthens. Emphasis shortens. Humour fragments. Weight slows. Map the trigger to the move
- **Where the writing is most alive** — which moments feel most distinctly like this person? These are the moves to preserve above all others
- **Flattening zones** — where does the rhythm go uniform? Three or more sentences of the same length and structure in a row. This is the pattern that kills the human feel. Note it even in informal writing — it shows the writer's own drift defaults
- **How thoughts connect** — contrast, addition, consequence, juxtaposition? The connective instinct shapes how ideas flow
- **Closing moves** — clean stop, trail, pivot, punchline? Note every variant
- **The flat delivery** — if dry humour or deadpan appears, document the move precisely: what they say, what they leave unsaid, what makes it land. This is a rhythm move, not a tone note
- **Punctuation and spelling as rhythm signals** — ellipses, question marks, repeated letters ("Noooooo") are signals about pacing and energy, not errors

Do not evaluate the writing as good or bad. Do not flag errors for correction. This is source material.

Share observations in chat, then use question `evidence_confirmation`. Do not rewrite anything yet.

After confirmation, use question `has_other_source` — replacing "[other type]" with "professional or brand copy (posts, emails, website copy)". If yes, run Step 1 before proceeding to Step 2C.

---

### Step 2: Signal Map (Path B only)

Analyse all samples together as a set — not piece by piece. The Copy Evidence Pass is editorial. This step turns the samples into instructions Claude can reuse.

Extract and document:

- **Reusable context** — facts, assumptions, audience realities, and brand positions Claude should know before drafting
- **Writing behaviours to preserve** — patterns that already work and should be repeated deliberately
- **Writing behaviours to stop** — patterns that make the copy generic, stiff, vague, over-written, or off-brand
- **Rhythm and structure** — sentence length, paragraph shape, pacing, and how complexity is handled
- **Openings and closings** — how pieces begin and end; note the moves that appear repeatedly and the moves that never appear
- **Recurring language** — words, phrases, constructions, or proof styles that appear across multiple samples; note frequency where useful
- **Absence evidence** — punctuation, vocabulary, structures, or opening types consistently missing. Back each with a count: "em dashes: 0 of 6 samples", "rhetorical questions: 0 of 6 samples". Only list what the samples clearly and consistently avoid
- **Drift risk** — where Claude is most likely to produce generic output if not given a clear instruction
- **Instruction candidates** — 5–8 possible rules that could be saved into `voice.md`

Output the Signal Map in chat before moving to Step 3. It feeds directly into Writing Defaults, Drift Controls, and Reusable Rewrite Patterns.

---

### Step 2C: Rhythm Extraction Map (Paths C and B+C)

Analyse all samples together. The Natural Voice Pass is observational. This step turns those observations into a usable Move Repertoire — the full set of rhythm moves this writer makes, with the trigger for each one.

The goal is not to find a dominant pattern to repeat. Writing feels human because it varies. The Move Repertoire gives Claude the full range so it can shift deliberately — the way the writer does — rather than defaulting to uniform rhythm.

Extract and document:

**Move Repertoire**
List every distinct rhythm move that appears across the samples. For each move, name it, describe it precisely, and state its trigger. Do not rank by frequency. A move that appears three times is as worth preserving as one that appears ten times — if it's distinctly theirs.

Format each entry:
> **Move:** [name]
> **What it looks like:** [precise description — length, structure, punctuation]
> **Trigger:** [what causes the writer to reach for this move — excitement, emphasis, weight, humour, complexity, reaction]
> **Example from samples:** [exact quote]

Example entries:
> **Move:** The accelerating list
> **What it looks like:** Items stacked in short successive sentences or fragments, each one landing before the next begins. No connective tissue between items.
> **Trigger:** Excitement or enthusiasm — the writer has more to say than one sentence can hold.
> **Example:** "Tulips and all the varieties… daffodils… azaleas and rhododendrons…"

> **Move:** The flat-delivery undercut
> **What it looks like:** Three fragments, each shorter than the last. Stakes a claim, then undercuts it, then pivots or completes. No setup, no signal, no explanation after.
> **Trigger:** When the writer wants to deflate something without making a joke of it.
> **Example:** "maybe fatal / Or maybe not / Let's test"

> **Move:** The clean drop
> **What it looks like:** A longer sentence followed immediately by a very short one (under 6 words) that lands the point. No qualifier after the short sentence.
> **Trigger:** Emphasis — the short sentence is the thing that matters.
> **Example:** "Your brain doesn't know the difference between pretending and actually being so why not"

**Flattening zones**
Where does the rhythm go uniform in the samples? Three or more sentences of similar length and structure in a row — this is the writer's own drift default. Name it explicitly so the brand copy knows to break it.

Example: "When explaining something factual, sentences tend to become similar in length and structure. This is where the writing loses its human feel. Break the pattern by dropping to a short sentence or switching to a fragment."

**Connective instincts**
How ideas link. Not what connective words appear — which connective instinct drives the writing. Juxtaposition, addition, contrast, consequence. Example: "Juxtaposition. Ideas sit next to each other; the contrast does the work. 'Which means' and 'therefore' are absent."

**Absence evidence**
Moves, structures, and punctuation that never appear. Note them — they define the negative space of the voice as clearly as what does appear.

**Voice Calibration table**
Map every move in the repertoire: does it carry into brand copy as-is, with adjustment, or not at all?

| Move | Carries over? | Brand adjustment |
|---|---|---|
| [move name from repertoire] | Yes / Partially / No | [what changes, if anything] |

Every row must reference a specific move from the repertoire above. "Casual tone — yes" is not a row. "Flat-delivery undercut — yes, reserve for deflating hype, never for serious points" is.

**Instruction candidates**
6–10 rhythm rules for `voice.md` — written as Claude instructions, not observations. Each instruction should describe a move and its trigger, not just a preference.

Example: "When making a strong observation, follow it with a sentence under 6 words. Do not add a qualifier after it."
Example: "When the writer is listing things they're enthusiastic about, let the list accelerate — short successive sentences, no connective words between items."

Output the Rhythm Extraction Map in chat before moving to the next step. If Path C only, proceed to Step 3 (interview). If Path B+C, proceed to Step 2D (Combined Synthesis).

---

### Step 2D: Combined Synthesis (Path B+C only)

Both passes are complete. This step combines the signals before the interview so the output files reflect both sources.

Do not treat professional copy as the structure source and informal writing as the rhythm source. Signals can come from either. A LinkedIn post might contain the clearest example of a flat delivery move. A personal message might reveal a structural instinct — how the writer builds toward a point — that's worth preserving in professional writing.

Produce a Combined Move Repertoire that draws from both sources:

**Step 1 — Merge the move repertoires**
Take every move documented in the Signal Map and the Rhythm Extraction Map. Remove duplicates (same move, different source — note that it appears in both). Flag moves that only appear in one source and decide whether they carry into brand copy.

**Step 2 — Map rhythm onto structure**
For each channel or format identified in the Signal Map, apply the Move Repertoire:
- What move opens this format?
- How does the rhythm shift through the body?
- What move closes it?
- Where is the writing most at risk of flattening?

This produces a format-level rhythm template. Example for a LinkedIn post:
> **Hook:** Clean drop or specific fact first — short sentence (under 8 words), no preamble.
> **Body:** Mix of longer explanatory sentences and short emphasis drops. If three sentences are the same length, break the third.
> **Close:** Flat-delivery undercut or clean stop. No rhetorical question. No "what do you think?"

**Step 3 — Flag the flattening risk**
State explicitly where the combined sources show the writer is most likely to produce uniform rhythm — in the professional copy OR the informal writing. This is the primary drift risk for brand output.

Output the Combined Synthesis in chat. Then use `AskUserQuestion` with question `synthesis_confirmation`. Incorporate any corrections before proceeding to the setup interview.

---

### Step 3: Setup interview

Deliver the 8 interview questions in two batched `AskUserQuestion` calls — not one at a time.

**Batch 1** (questions 1–4): include `default_stance`, `audience`, `generic_output_risk`, and `reference_style` as four questions in a single `AskUserQuestion` call. Wait for the user to answer all four before continuing.

**Batch 2** (questions 5–8): include `differentiation`, `off_limits`, `reader_reaction`, and `address_style` as four questions in a single `AskUserQuestion` call. This is the final submit.

---

### Step 4: Reader Context

Before defining how Claude should write, synthesise what the interview and, if Path B, the Copy Evidence Pass and Signal Map revealed about the reader. Write this directly into `brand-context.md` as a named section.

Include:

- Who they are and what they do
- What they already believe before they find this brand
- What they are trying to accomplish
- What would make them dismiss the brand immediately
- What Claude must understand about them before drafting

This section anchors every decision in Steps 5–9. If a writing instruction does not serve this reader, it does not belong in the document.

Output the Reader Context in chat before writing it to `brand-context.md`. Then use `AskUserQuestion` with question `reader_context_confirmation`. Incorporate any corrections, then write the finalised section into the file and continue to Step 5.

---

### Step 5: Writing Defaults

Generate exactly 4 candidate defaults based on everything gathered so far. These are the behaviours Claude should fall back on when no more specific instruction is given.

Do not format them as simple adjectives. Each default must include the useful behaviour, the version that goes too far, and the instruction Claude should follow.

**Format:**

> **Default:** [the writing behaviour Claude should use]
> **Too far:** [how this behaviour fails when overdone]
> **Claude instruction:** [one concrete instruction Claude can apply while drafting]

Example:

> **Default:** Say the useful thing plainly.
> **Too far:** Sound blunt, impatient, or dismissive.
> **Claude instruction:** Make the point directly, but keep one human cue that shows there is a person on the other side.

Then use `AskUserQuestion` with `multiSelect: true` to let the user select which defaults feel essential. Build the options dynamically from the 4 candidates. Instruct: "We need 3–4. If removing a default would not change how Claude writes, it is not doing anything."

Defaults that could fit any brand — "be clear", "be professional", "be helpful" — should be replaced or made more specific before they are offered as options.

---

### Step 6: Drift Controls

For each selected Writing Default, define how Claude should recognise and correct drift.

Cover:

- The generic version Claude is likely to produce
- The signal that tells Claude the draft is drifting
- The correction instruction
- One example phrase to avoid
- One better move to make instead

**Format:**

> **Default:** [selected default]
> **Common drift:** [what generic or off-brand output looks like]
> **Correction:** [how Claude should pull it back]
> **Avoid:** [phrase, construction, or behaviour]
> **Use instead:** [replacement move, not necessarily a fixed phrase]

These controls are the guardrails for AI output. They should be specific enough that Claude can check a draft against them before sending it.

---

### Step 7: Channel Handling

The brand should still sound like itself everywhere. Claude changes the handling, not the underlying writing system.

Map the formats this brand actually writes in. Use this as a starting point and adapt:

| Format | How Claude should handle it |
|---|---|
| Homepage / hero | Strongest point of view. No hedging, no setup paragraph, no generic promise. |
| Landing or lead magnet page | Clear and instructional. Explain what it is, what it does, what it produces, and how to use it. No sales pressure. |
| Social post | Looser rhythm, stronger hook, still specific. Avoid scaffold labels left in the final copy. |
| Email subject line | Specific over clever. State what the reader gets. No emojis unless the brand explicitly uses them. |
| Email opening | Start with the useful observation, not the announcement that there is an update. |
| Product or feature copy | Concrete use case first. Translate features into what the reader can actually do. |
| Practical steps / how-to | Shortest sentences. Most direct. Personality steps back; usefulness moves forward. |
| Error or friction moment | Calm and factual. Tell the user what happened and what to do next. No apology theatre. |
| Confirmation / success state | One beat of confirmation, then the next useful action. |
| Sensitive or difficult topic | Quieter. Factual. Name uncertainty if it is real, but do not hedge by default. |

Keep only the formats relevant to this brand. Add any that are missing.

---

### Step 8: Language Instructions

Define the specific settings Claude should follow. Skip anything that does not genuinely distinguish this brand from the default.

**Vocabulary**

- Words and phrases Claude should use naturally
- Words and phrases Claude should avoid — with the reason, not just "we do not like it"
- Terms the brand has claimed, redefined, or uses in a specific way
- Industry language: use as a signal of expertise, or strip for accessibility?
- Proof style: examples, numbers, metaphors, screenshots, lived experience, data, or comparison

**Grammar and style**

- Contractions: yes (warmer) or no (more formal)
- Default sentence length: short and punchy / medium and considered / varies by format
- Pronouns: "we", "I", "you", "your team" — and which appears where
- Capitalisation: sentence case, title case, or deliberate lowercase
- Punctuation: em dashes, Oxford comma, ellipses — use or avoid?
- Numbers: spell out under ten, or always use digits?
- Active voice as default unless there is a specific reason not to

---

### Step 9: Reusable Rewrite Patterns

Write 15–20 rewrite patterns. This section teaches Claude how to correct generic output into brand-correct output.

**Format for each pattern:**

> **When Claude writes this:** [generic or drifting draft]
> **Steer it here:** [brand-corrected draft]
> **Instruction to save:** [one reusable rule Claude should apply next time]

The generic version should be plausible, not cartoonish. Real drift is subtle — the contrast should feel like the difference between Claude writing from vague instructions and Claude writing from this brand's actual system.

Cover at least:

- Homepage headline
- Supporting line or subheadline
- Primary CTA
- Feature or product description (2–3 sentences)
- Email subject line
- Email opening line
- Practical step or how-to instruction
- Social post opening
- Error or friction message
- Confirmation message
- One paragraph from an About page or lead magnet page

For Path B, transform real examples from the Copy Evidence Pass where possible. For Path A, write a plausible generic version first, then the brand-corrected version.

For Paths C and B+C, the brand-corrected draft must use moves from the Move Repertoire — not just correct the content. The corrected draft should vary sentence length the way the source samples do. It should use the specific moves identified — the clean drop, the accelerating list, the flat delivery undercut — in the right context, triggered by the right conditions. At least one pattern should demonstrate the full range: a longer sentence followed by a short drop, or a list that accelerates, or a flat-delivery close. The goal is copy that sounds like this person wrote it after deciding to be clear and presentable — not copy that sounds like a competent writer who happened to avoid buzzwords.

---

### Step 10: Draft Test

Apply the finished files to something new. Ask the user for a brief — a product update, a lead magnet page section, a campaign line, a social post, or a subject line for next week's email — and write a first draft using only `brand-context.md` and `voice.md` as the guide.

If the output could have come from any brand, the Writing Defaults or Drift Controls are not specific enough. Return to Step 5 or Step 6 and tighten.

If it sounds right, the files are ready.

Before closing, make the files operational: where they live, who uses them, and when Claude should read them. A document that does not get opened cannot do anything.

---

## Failure checks

Watch for these. They produce files that look complete but do not improve Claude's drafts.

- **Defaults that fit any brand.** If a competitor could use the same instruction without changing a word, it is not doing anything. Push until the rule is specific enough to be wrong about most brands.
- **No drift control.** A positive instruction is not enough. Claude also needs to know the bad version it is likely to produce and how to correct it.
- **Rewrite patterns that are too obvious.** If the generic draft is something no one would actually write, it teaches nothing. Use plausible AI drift, not parody.
- **Target voice presented as current reality.** If the brand does not currently sound the way the document says it does, label the gap. A clearly marked gap is more useful than a polished fiction.
- **Format handling mistaken for voice.** A social post and an error message do not use the same handling, but they should still sound like the same brand. Do not confuse channel adaptation with a different voice.
- **No operating instruction.** Before closing, state where `brand-context.md` and `voice.md` live and when Claude should read them.
- **Move Repertoire not applied (Paths C and B+C).** If the rewrite patterns correct vocabulary and buzzwords but the brand-corrected drafts still sound like generic AI output at the sentence level, the Move Repertoire has not been used. Check: does the corrected draft vary its sentence length the way the source samples do? Does it use the specific moves from the repertoire — the clean drop, the accelerating list, the flat delivery? If three consecutive sentences in the corrected draft are the same length and structure, the rhythm has flattened. Return to the Rhythm Extraction Map and apply the moves.
- **Uniform rhythm treated as safe default.** Writing where every sentence is roughly the same length feels robotic — even if the vocabulary is correct and the content is specific. The Move Repertoire exists to prevent this. If the voice.md does not include explicit instructions to vary sentence length and use the identified moves with their triggers, it will not prevent uniform output.

---

## Output format

Two files, saved to the project root or wherever the user specifies.

**`brand-context.md`** — the foundation everything else derives from

- Brand name and what it does
- Reader Context: who they are, what they already believe, what they are trying to accomplish, what would make them dismiss the brand, and what Claude must understand before drafting
- Positioning and what makes this brand different
- Reference styles and what specifically resonates about each
- What the brand stands for and what it stands against

**`voice.md`** — the working reference Claude reads before writing

- Writing Defaults: 3–4 default behaviours with "too far" versions and Claude instructions
- Drift Controls: how generic output shows up and how Claude should correct it
- Channel Handling: how Claude adjusts across formats and situations
- Language Instructions: vocabulary, proof style, grammar, and style settings
- Move Repertoire *(Paths C and B+C)*: the full range of rhythm moves with triggers for each, flattening zones to avoid, connective instincts, absence evidence, and format-level rhythm templates where both sources were provided — written as Claude instructions, not observations
- Reusable Rewrite Patterns: generic draft / brand-corrected draft / saved instruction
- Anti-patterns: specific phrases, constructions, and behaviours Claude should avoid

`voice.md` can stand alone or feed into a wider brand style guide. Every downstream writing task — posts, emails, product copy, scripts, lead magnet pages, support copy — should read `brand-context.md` and `voice.md` before drafting.
