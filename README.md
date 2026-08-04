<div align="center">

<h1>Design brief</h1>

**Turn a messy transcript, project doc, or pasted chat thread into a structured 5-bullet design brief - with a gap report for what's missing, for design teams and PMs kicking off new work.**

[![CI](https://img.shields.io/github/actions/workflow/status/humbleteam/design-brief/validate.yml?branch=main&style=for-the-badge&logo=github&label=CI)](https://github.com/humbleteam/design-brief/actions/workflows/validate.yml)
[![GitHub stars](https://img.shields.io/github/stars/humbleteam/design-brief?style=for-the-badge&logo=github&color=181717)](https://github.com/humbleteam/design-brief/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/humbleteam/design-brief?style=for-the-badge&color=339933)](https://github.com/humbleteam/design-brief/commits/main)
[![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude_Code-skill-D97757?style=for-the-badge&logo=anthropic&logoColor=white)](https://github.com/humbleteam/design-brief/blob/main/SKILL.md)

</div>

design-brief turns whatever a team has scattered around - a call transcript, project doc, pasted freetext, chat thread - into a structured design brief: Problem, Audience, Success metric, Must-haves, Constraints. Its gap report keeps it honest: when the input does not answer the problem or the audience, the skill lists what is missing and stops, rather than writing a plausible paragraph to fill the hole. When a brief already exists and the project pivots, the change folds into the right bullet and the full brief re-emits, so a two-word directive never wipes the rest of the document.

## Table of contents

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Usage](#usage)
- [Example output](#example-output)
- [How it works](#how-it-works)
- [How is this different from just asking the model?](#how-is-this-different-from-just-asking-the-model)
- [FAQ](#faq)
- [Related skills](#related-skills)
- [Who maintains this](#who-maintains-this)

## What it does

- Reads every source in the input - transcripts, docs, freetext, chat history - before asking a question, and never asks for a field already answered.
- Extracts a fixed 5-bullet brief: Problem, Audience, Success metric, Must-haves, Constraints.
- Refuses to build a brief on guesses: if Problem or Audience is absent from every source, it returns a gap report naming what is missing, then stops.
- Marks Success metric, Must-haves, or Constraints `(not stated - ...)` when the input does not cover them, instead of guessing.
- Handles incremental edits ("now it's B2C", "add constraint: iOS only") by folding the change into the right bullet and re-emitting the brief, unchanged elsewhere.
- Surfaces conflicting sources ("the doc says B2B, the transcript says B2C") instead of silently picking one.

## Quick start

**Personal - available in every project:**

```bash
git clone https://github.com/humbleteam/design-brief ~/.claude/skills/design-brief
```

**Project-scoped - checked into one repo:**

```bash
git clone https://github.com/humbleteam/design-brief .claude/skills/design-brief
```

**Any other agent:** the skill is plain markdown in the Agent Skills format. Paste [`SKILL.md`](SKILL.md) into the system prompt of Cursor, Codex, or any LLM agent - no Claude-specific runtime required.

To verify: restart Claude Code and ask it to list its skills. Skills load from `~/.claude/skills/` (personal) and `.claude/skills/` (project) - `design-brief` should appear.

## Usage

- **"Turn this call transcript into a design brief."** (transcript pasted or attached) - reads it, extracts every bullet it can source, and gap-reports anything missing.
- **"Here's our project doc - what's missing from the brief?"** - runs the same extraction and returns the gap report if Problem or Audience is not covered.
- **"Update the brief - we're now targeting enterprise buyers, not solo users."** - an incremental edit: folds the pivot into Audience, leaves every other bullet untouched, and re-emits the brief.

## Example output

Example project and notes are invented for illustration.

Input pasted by the user:

```
Notes from kickoff call, July 8:
- Client is a mid-size trucking dispatch company, 40-80 trucks per client.
- Dispatchers currently juggle 3 different spreadsheets to assign loads -
  it's slow and error-prone, they miss delivery windows.
- Want something that cuts load-assignment time in half within the first
  month of use.
- Needs to work offline in the truck yard where wifi is spotty.
- Must integrate with their existing ELD (electronic logging device) system.
- No budget yet for a native mobile app - web only for v1.
```

Skill output:

```
## Design brief - Pathfinder (dispatch scheduling)

- **Problem:** Dispatchers assign loads across 3 disconnected spreadsheets,
  which is slow and causes missed delivery windows.
- **Audience:** Freight dispatchers at mid-size trucking companies running
  40-80 trucks.
- **Success metric:** Cut load-assignment time in half within the first
  month of use.
- **Must-haves:** Works offline in low-connectivity yard environments;
  integrates with the client's existing ELD system.
- **Constraints:** Web only for v1 - no budget for a native mobile app.

_Source - Problem, Audience, Success metric, Must-haves, Constraints: kickoff
call notes, July 8._
```

## How it works

- **Read everything before asking anything.** Freetext, docs, transcripts, chat history - all read before a field is marked missing.
- **Two required bullets, three optional ones.** Problem and Audience block the brief if absent; the rest get a `(not stated - ...)` marker instead.
- **The gap report is one compact block, then a full stop.** No partial brief gets built on a guess - a missing Problem or Audience triggers one gap-report block, nothing else.
- **Audience and constraints need to be specific and checkable.** "Users" fails; "freelance expedition guides booking 5-20 trips a year" passes. "Mobile-first, WCAG AA, no native app in v1" survives; "make it modern" gets dropped.
- **Every bullet carries a source.** A footer names which input and section each bullet came from.
- **An incremental edit touches exactly one bullet.** The directive maps to the bullet it changes; everything else stays byte-for-byte and the full brief is re-emitted - never a reply that reads as if the rest got deleted.
- **Conflicting sources get surfaced, not resolved silently.** A doc saying B2B against a transcript saying B2C gets named as a conflict, not silently picked.

## How is this different from just asking the model?

A bare "write a design brief from this" prompt tends to paraphrase the input into one paragraph and invent a success metric or constraint that sounds reasonable but was never stated. Asking for an update - "now it's B2C" - usually rebuilds the brief from that one sentence, erasing the Problem, Must-haves, and Constraints it never repeated. This skill pins the structure to 5 named bullets, refuses to fabricate the two required ones, and treats an edit as a patch to one bullet, not a rewrite of the document.

## FAQ

**What should a design brief include?**
5 bullets: Problem (the pain or business need), Audience (a role, size, and behavior - not just "users"), Success metric (one observable outcome), Must-haves (3-5 non-negotiable features), and Constraints (2-4 enforceable limits like platform, accessibility, or budget).

**How do I write a design brief from a meeting transcript?**
Paste or attach the transcript and ask for a brief. The skill reads it in full, extracts what it can source, and returns a gap report instead of a guess for anything the transcript never covers.

**How do I keep a design brief up to date?**
Give the skill a directive naming what changed ("now it's B2C", "add constraint: iOS only"). It folds the change into the correct bullet, leaves the rest untouched, and re-emits the full brief.

**Can Claude write a design brief for me?**
Yes, from whatever unstructured input you already have - transcripts, docs, chat threads, freetext. It will not invent a problem or audience it cannot source; those two fields trigger a gap report.

**What if my brief is missing information?**
If Problem or Audience is missing, the skill stops and lists what is needed. If Success metric, Must-haves, or Constraints is missing, the brief still gets built with that bullet marked `(not stated - ...)`.

## Related skills

Part of a 10-skill open-source kit for design teams by Humbleteam.

- [design-review](https://github.com/humbleteam/design-review) - structured UX critique with a 0-4 score, Before/After/Why fixes, and a citation for every claim.
- [ascii-wireframes](https://github.com/humbleteam/ascii-wireframes) - three distinct layout hypotheses as ASCII wireframes before any hi-fi work.
- [html-mockup](https://github.com/humbleteam/html-mockup) - census-first HTML mockups that match a reference screenshot: exact palette, item counts, component states.
- [extract-design-tokens](https://github.com/humbleteam/extract-design-tokens) - pull palette, type, spacing, radii, and shadows from a URL or screenshot into CSS variables and JSON.
- [audit-design-tokens](https://github.com/humbleteam/audit-design-tokens) - find token drift in a codebase: raw hex values, off-scale spacing, near-duplicate colors.
- [design-qa](https://github.com/humbleteam/design-qa) - a pre-ship design QA gate: states, contrast, touch targets, breakpoints, keyboard paths.
- [design-handoff](https://github.com/humbleteam/design-handoff) - turn a finished mockup into a dev-ready spec: tokens, states, accessibility annotations, open questions.
- [accessibility-audit](https://github.com/humbleteam/accessibility-audit) - WCAG 2.2-grounded accessibility review with success-criterion citations and severity levels.
- [ux-writing](https://github.com/humbleteam/ux-writing) - interface copy that reads human: plain-verb microcopy rules and an AI-tell strip pass.

## Who maintains this

[Humbleteam](https://humbleteam.com/) is a digital product design and AI-engineering studio: founded in 2017, working from Prague and Dubai, with 80+ digital awards to the name, including 14 Awwwards wins, a Webby, and a Red Dot. We design digital products for startups and enterprises in fintech, healthtech, sports, and AI, and we build AI infrastructure for design teams - agents, workflows, and skills like this one.

This skill is distilled from the internal playbooks we run on client work: the same checklists behind the case studies at [humbleteam.com/work](https://humbleteam.com/work), for clients like Tinder and Acronis.

- The full 10-skill kit: [Related skills](#related-skills) above, or all repos at [github.com/humbleteam](https://github.com/humbleteam)
- What we do with AI for design teams: [humbleteam.com/ai](https://humbleteam.com/ai)
- Design and AI writing: [humbleteam.com/blog](https://humbleteam.com/blog)
- LinkedIn: [linkedin.com/company/humbleteam](https://www.linkedin.com/company/humbleteam/)
- Talk to us: [hi@humbleteam.com](mailto:hi@humbleteam.com)

Issues and PRs welcome.

MIT - see [LICENSE](LICENSE).
