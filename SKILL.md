---
name: design-brief
description: Extract a 5-bullet design brief (Problem, Audience, Success metric, Must-haves, Constraints) from messy inputs - transcripts, docs, freetext - with a gap report instead of guessing at missing fields. Trigger phrases - "write a design brief", "turn this transcript into a brief", "what's missing from this brief", "update the brief, now it's B2C". Do not use for visual design tokens (extract-design-tokens) or a dev handoff spec (design-handoff).
---

# Design brief

Extract a 5-bullet design brief from whatever a team already has, and say exactly what's missing instead of filling gaps with guesses.

## Step 1 - decide fresh brief or incremental edit

- **Fresh brief**: no design brief exists yet anywhere in the input or the conversation so far.
- **Incremental edit**: a 5-bullet brief already exists - pasted by the user or produced earlier in this conversation - and the new message is a directive that changes part of it. Examples: "now it's B2C", "add constraint: iOS only", "the success metric is signup conversion now, not retention".

Fresh brief goes to Step 2. Incremental edit skips straight to Step 6.

## Step 2 - read every source before asking anything

Collect everything provided: freetext, pasted transcripts, attached documents, meeting notes, chat threads, screenshot captions, earlier messages in the conversation. Read all of it before deciding anything is missing. Never ask for a field that is already answered somewhere in the input, even if it is buried in paragraph three of a 50-page document.

If the input describes more than one distinct project, stop and ask which project the brief is for before extracting anything.

## Step 3 - extract the 5 bullets

- **Problem** - the pain or business need this product solves. Required.
- **Audience** - the specific user role, rough size or segment, and a defining behavior or need. Required. "Users" is not an audience; "freelance expedition guides booking 5-20 trips a year" is.
- **Success metric** - one observable, measurable outcome. Optional but flag if missing.
- **Must-haves** - 3-5 non-negotiable features or qualities. Optional but flag if missing.
- **Constraints** - 2-4 enforceable limits: accessibility, platform, brand, regulatory, budget, timeline. Optional but flag if missing. Drop anything unenforceable ("make it modern") rather than rephrasing it into something that only sounds checkable.

Source every value directly from the input. Do not infer a success metric from a vague ambition, and do not round a stated audience up into a broader one.

If two sources disagree (one doc says B2B, a transcript says B2C), do not silently pick one. Surface the conflict and ask which is canonical.

## Step 4 - gap check

Problem and Audience are the only two bullets that block a brief. If either is genuinely absent from every source - not stated, not inferable - stop and emit a gap report instead of a brief. See format below.

Success metric, Must-haves, and Constraints never block. If any of those three is missing, still build the brief and mark that bullet `(not stated - <what would satisfy it>)`.

### Gap report format

Emit exactly one block, no brief, no other prose:

```
Missing to build a brief:
- Problem: not found in any source - what pain or business need does this solve?
- Audience: not found in any source - who specifically will use this (role, rough size, key behavior)?
```

One bullet per missing required field. Stop after this block - the user's next reply re-supplies the missing piece, and you re-run Step 2 with the combined input.

## Step 5 - write the brief

Output format:

```
## Design brief - <project name if known, else omit the suffix>

- **Problem:** <...>
- **Audience:** <...>
- **Success metric:** <... or "(not stated - need one observable, measurable outcome)">
- **Must-haves:** <... or "(not stated - need 3-5 non-negotiable features or qualities)">
- **Constraints:** <... or "(not stated - need 2-4 enforceable limits)">

_Source - Problem: <input name>; Audience: <input name>; Success metric: <input name or "not found">; Must-haves: <input name>; Constraints: <input name or "not found">._
```

For a large document, cite the section or heading, not just the filename: `Problem: kickoff-notes.docx, section "Current process"`.

The footer names all five bullets one by one, and it keeps that shape when a single input answered every one of them. Collapsing them onto one shared source (`Problem, Audience, Success metric, Must-haves, Constraints: kickoff notes, July 8`) is shorter and reads fine on the day it is written, but it is a form the brief outgrows: it has nowhere to put `not found` for a bullet that gap-marked, and the first incremental edit re-sources one bullet and has to expand the line anyway. Two footer shapes for one skill also means a reader has to work out which one they are looking at before they can tell where a bullet came from. Five named bullets say the same thing in every state of the brief.

A bullet whose origin the input never names is written `carried from the brief as given` (the incremental-edit case, Step 6) or `not found`. Never write a document name the input did not state.

This ends the fresh-brief path.

## Step 6 - incremental edit

1. Start from the existing brief exactly as given.
2. Split the directive into its separate changes, then map each one to the bullet it targets. A copy/platform/regulatory rule -> Constraints. An audience pivot -> Audience. A new target number -> Success metric. A new non-negotiable feature -> Must-haves. A reframed pain point -> Problem.
3. Route on what step 2 produced, first match wins:
   - **One change, target clear.** Fold it into that bullet.
   - **Two or more changes, every target clear** ("we're B2C now and drop the offline requirement"). Fold each one into its own bullet. A compound directive is ordinary, not an error: the user stated two facts, and applying one while dropping the other loses information they just gave you.
   - **One change, target unclear.** Ask one clarifying question naming the 1-2 bullets it could plausibly belong to. Do not guess.
   - **Several changes, some clear and one not.** Apply the clear ones, re-emit the brief, then ask about the unclear part alone, listing what you already applied. Sending the whole directive back makes the user restate changes you understood; ignoring the unclear part drops a change they asked for.
4. Every bullet that no part of the directive touches stays byte-for-byte identical - do not rephrase, tidy, or "improve" a bullet the user did not touch. "Every other bullet" is measured against the whole directive, not against the first change found in it.
5. Update the source footer for each bullet you changed, appending the new input (e.g. `Must-haves: kickoff-notes.docx + update, Jul 10`). A brief this skill produced earlier in the conversation already has source names: carry them forward exactly.
6. If the brief was pasted with no source footer, the footer still ships. Every bullet you did not touch is sourced `carried from the brief as given`, and each bullet you changed is sourced to the directive. Do not reach for a filename to fill the line: four bullets with no stated origin is a fact about the input, and naming a document the user never mentioned sends the next reader hunting for a file that does not exist. This is the one place where "the footer is not optional" and "never invent a value" would otherwise collide.
7. Re-emit the full 5-bullet brief using the Step 5 format. Never reply with only the changed bullets - a partial answer reads as if the rest of the brief was deleted.

Rebuilding the whole brief from the directive alone is the primary failure mode of this skill: a two-word directive like "now it's B2C" contains no information about Problem, Success metric, Must-haves, or Constraints, and guessing them from scratch silently destroys real information the team already gave you. Applying half of a compound directive is the same loss on a smaller scale, and it is harder to spot: the brief comes back complete and well formed, with one of the changes the user asked for simply absent.

## Edge cases

| Situation | What to do |
|---|---|
| Input describes more than one distinct project | Stop before extracting. Ask which project the brief is for. |
| Two sources disagree on a fact (doc says B2B, transcript says B2C) | Surface the conflict by name, quoting both sources, and ask which is canonical. Never pick silently. |
| A 50-page document is pasted or attached | Extract normally, but cite the specific section or heading per bullet in the source footer, not just the document name. |
| Only Problem or only Audience is missing, not both | Gap-report the one missing field (one bullet) and stop - do not build a half brief while waiting on it, and never list a field the input already answered. |
| Success metric, Must-haves, or Constraints is missing but Problem and Audience are present | Build the brief. Mark the missing bullet `(not stated - ...)`. Do not gap-report for these three. |
| A directive arrives but no brief exists yet in the conversation | Treat it as a fresh brief with very thin input - most fields will gap-report. Do not fabricate a brief around a bare directive. |
| Directive is ambiguous about which bullet it targets | Ask one clarifying question naming the 1-2 bullets it could plausibly belong to, rather than guessing. |
| One directive changes two or more bullets ("we're B2C now and drop the offline requirement") | Apply every change to its own bullet, leave the untouched bullets byte-for-byte, re-emit all 5. A compound directive is not an ambiguous one - it has two clear targets, not one unclear target, so it does not go to the clarifying question. |
| A compound directive where one part is clear and another is not | Apply the clear parts and re-emit the brief, then ask about the unclear part alone, naming the changes already applied. A clarifying question that covers the whole directive makes the user restate what you understood. |
| The existing brief was pasted with no source footer | Re-emit the footer anyway. Untouched bullets are sourced `carried from the brief as given`; the changed bullet is sourced to the directive. Never fill the gap with a guessed document name. |
| The pasted brief carries a footer for some bullets only | Carry the named sources forward as they stand, and mark the rest `carried from the brief as given`. Provenance is per bullet, not per brief. |
| Every bullet traces back to the same single input | The footer still names all five, one by one. Do not compress them into one shared source line: the grouped form has no slot for a `not found` bullet, and the first edit that re-sources one bullet has to expand it again. |

## Rules that hold in every mode

- Never invent a value. If it is not in the input, it is either a gap-report line (Problem/Audience) or a `(not stated - ...)` marker (the other three).
- Audience needs a role, a rough size or segment, and a defining behavior - never just "users" or "customers."
- Constraints must be checkable by someone other than the author. Drop a vague constraint rather than dressing it up as an enforceable one.
- The source footer is not optional, and it names every bullet separately even when one input answered all five. Every bullet traces back to a named input, or to `carried from the brief as given` when the input never named one. An invented document name is a worse answer than an honest gap.
- An incremental edit always re-emits all 5 bullets. A one-bullet reply is a bug, not a shortcut.
- An incremental edit changes every bullet the directive names, and no others. One bullet is the common case, not a cap - a directive carrying two clear changes gets both.
