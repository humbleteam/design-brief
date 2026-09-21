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
- **Constraints** - 2-4 enforceable limits: accessibility, platform, brand, regulatory, budget, timeline. Optional but flag if missing. Drop anything unenforceable ("make it modern") rather than rephrasing it into something that only sounds checkable, and name what you dropped under the brief - see below.

### What the counts mean

The counts on the three optional bullets - one metric, 3-5 must-haves, 2-4 constraints - are the shape to aim for, not a gate the bullet has to pass. A bullet is stated once the input supports one real entry, and it ships with exactly what the input supports:

- **Under the range.** Ship the entries you have and mark the shortfall inside the bullet, after the entries: `(2 of 3-5 - what else is non-negotiable here?)`. Never pad to reach the floor. The bullet is not `(not stated - ...)` either, because that marker asks for a bullet the input never answered, and re-asking for what the input already gave is the mistake Step 2 exists to prevent. It is not a gap report: Problem and Audience are still the only two bullets that block a brief.
- **Over the range.** Keep every entry the input calls non-negotiable and ask, under the brief, which of them truly are. Cutting nine must-haves down to five to fit the shape drops information the team gave you, and nothing in the input says which four matter least.
- **Zero entries.** The `(not stated - ...)` marker, unchanged.

Dropping an unenforceable constraint is the commonest way a bullet lands under its floor - the rule above requires it, and a brief that starts with three constraints and drops two of them ends at one. That is the correct outcome, recorded as a shortfall. Putting "make it modern" back to reach 2 is not.

### A dropped entry is named, not just gone

Dropping is required. Dropping silently is not, and it is what the skill did. The brief comes back with `(1 of 2-4 - what else is enforceable here?)` on Constraints and no sign anywhere that "make it modern" was read at all, so the only fact the team has is that the bullet is short. The rational answer to a short bullet is to supply what is missing, and the thing they supply is the line they already gave, which gets dropped again. Step 2 exists to stop this skill asking twice for what the input carried; a drop nobody can see does the same damage from the other end.

A dropped entry ships as its own line under the brief, after the source footer, quoting the entry as the input stated it and saying what would make it checkable:

```
_Dropped as unenforceable - "make it modern": what would make this checkable? A product it should look current against, a design system to match, or a specific visual quality someone other than the author can confirm._
```

One line per dropped entry. This is not a gap report and it blocks nothing: the brief above it is complete and shippable. The line is an offer to convert a wish into a constraint, which is the only route by which "make it modern" can legitimately come back.

Quote the entry as stated. Paraphrasing it into something tidier hides which words were the problem, and the point of the line is to hand those words back.

The line is standing state, not a note on the run that produced it. It rides every later edit of the brief until the user answers it, and answering it is what retires it - see Step 6.

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

_Dropped as unenforceable - "<entry as the input stated it>": <what would make it checkable>._
_Above 3-5 must-haves - <n> stated: which of these are truly non-negotiable?_
```

For a large document, cite the section or heading, not just the filename: `Problem: kickoff-notes.docx, section "Current process"`.

A bullet short of its range carries the shortfall marker from Step 3 inside the bullet, after its entries. The footer is unaffected: a short bullet has a source like any other.

The two lines under the footer are where everything that happened to a bullet but is not in it goes: one per dropped entry, and one per bullet over its ceiling. Both are present only when they apply and are never shipped empty, and neither blocks the brief. On an incremental edit they are carried forward with the bullets, under the rules in Step 6. Until now the over-the-ceiling question was named in Step 3 and had nowhere in this format to sit, which is the same hole as the silent drop, one bullet along.

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
7. A directive that removes entries runs the same count rule as Step 3. What is left ships with a shortfall marker if the removal lands the bullet under its range, and a bullet the directive empties completely is marked `(none - removed on this edit)`, not `(not stated - ...)`, which would ask the user for something they just took out.
8. A directive that adds an entry runs Step 3's rules on it before it lands. An added constraint that someone other than the author cannot check is dropped exactly as it would be on the fresh path, and the drop line ships under the re-emitted brief. Without that line the brief comes back byte-for-byte identical to the one the user pasted, with nothing anywhere saying their directive was read - the silent absence the paragraph below names, arrived at from the other side and harder to spot, because here the skill did the right thing and said nothing about it. An added audience that is only "users", or a success metric that is an ambition rather than an observable outcome, goes to the clarifying question in 3 rather than into the bullet.
9. A drop line is part of the brief, not a note on the run that made it. Carry every drop line on the existing brief forward unchanged, the way an untouched bullet is carried - unless this directive answers it. Answering one is what the line exists for: it asks the user to restate a wish as something checkable, and until now the skill made that offer and had no rule for receiving the reply.
   - **The restatement is checkable.** It is not a new entry. It lands in the bullet it was dropped from, sourced to the directive, and its drop line goes with it. Keeping the line would ship a constraint with a question underneath asking for that same constraint, which is the asking-twice failure Step 2 exists to prevent, reached from a third direction.
   - **The restatement is still unenforceable.** It replaces that entry's drop line, quoting the new wording. One line per entry, not one per attempt: two lines for one unresolved wish read as two separate things the team owes you.
   - **Telling a restatement from a new entry.** Either signal is enough: the directive names or quotes the dropped wording ("by modern I mean..."), or the entry it produces is the same requirement made checkable. Neither one present, it is a new entry and the standing drop line stays.
   - A brief pasted with no drop lines has none to carry. Never reconstruct one from a bullet that looks short - what an earlier round dropped is not recoverable from what survived, and an invented drop line names a wish the user never wrote.
10. Re-emit the full 5-bullet brief using the Step 5 format. Never reply with only the changed bullets - a partial answer reads as if the rest of the brief was deleted.

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
| The input supports fewer entries than a bullet's range (2 must-haves, 1 constraint) | Ship what the input supports and mark the shortfall inside the bullet: `(2 of 3-5 - what else is non-negotiable here?)`. Never pad to the floor, and never use `(not stated - ...)`, which asks again for what the input already gave. |
| The input carries more than a bullet's ceiling (9 must-haves) | Keep all of them and ask under the brief which are truly non-negotiable. Never cut the list down to five to fit the shape. |
| Dropping unenforceable constraints leaves fewer than 2 | Correct outcome. Mark the shortfall, and name each dropped entry on its own line under the brief with what would make it checkable. A shortfall marker alone tells the team the bullet is short and not that their line was read, so they supply it again. |
| A directive adds a constraint that is not checkable ("make it feel premium") | Drop it by the same rule and ship the drop line with the re-emitted brief. Never return the brief unchanged and silent - that is indistinguishable from ignoring the directive. |
| A directive's only change is one Step 3's rules drop | The brief re-emits byte-for-byte by design, and the drop line is the whole answer. Say which entry was dropped and what would make it checkable, so the user can restate it as something enforceable. |
| A directive restates a dropped entry in checkable terms ("by modern I mean: match the density of the Stripe dashboard") | Not a new entry. Fold it into the bullet it was dropped from, source it to the directive, and retire its drop line. A brief that ships the constraint and keeps the question under it asks twice for the same thing. |
| A directive restates a dropped entry and it is still unenforceable | Replace that entry's drop line with one quoting the new wording. Never ship two lines for one unresolved entry - one line per entry, not one per attempt. |
| An edit arrives on a brief that already carries drop lines | Carry them forward unchanged with the bullets, except any this directive answers. A line that vanished while still unanswered would read as the question having been settled. |
| A brief is pasted with no drop lines under it | There are none to carry. Never reconstruct one from a bullet that looks short - an invented drop line names a wish the user never wrote. |
| A directive removes the last entry in a bullet | Mark it `(none - removed on this edit)`. The `(not stated - ...)` marker would ask the user for the thing they just removed. |
| Every bullet traces back to the same single input | The footer still names all five, one by one. Do not compress them into one shared source line: the grouped form has no slot for a `not found` bullet, and the first edit that re-sources one bullet has to expand it again. |

## Rules that hold in every mode

- Never invent a value. If it is not in the input, it is either a gap-report line (Problem/Audience) or a `(not stated - ...)` marker (the other three).
- The counts are a target shape, not a gate. A bullet ships with what the input supports, plus a shortfall marker when that is under its range. Padding a bullet to reach 3 must-haves or 2 constraints is inventing a value, and trimming one to fit a ceiling is dropping a fact the team gave you.
- Audience needs a role, a rough size or segment, and a defining behavior - never just "users" or "customers."
- Constraints must be checkable by someone other than the author. Drop a vague constraint rather than dressing it up as an enforceable one, and name the drop under the brief. A drop the output never mentions reads as a line nobody read, and it comes back next round.
- A drop line is standing state. It rides every later edit until the user answers it; a checkable restatement retires the line and lands in the bullet, and one that is still unenforceable replaces the line rather than adding a second.
- The source footer is not optional, and it names every bullet separately even when one input answered all five. Every bullet traces back to a named input, or to `carried from the brief as given` when the input never named one. An invented document name is a worse answer than an honest gap.
- An incremental edit always re-emits all 5 bullets. A one-bullet reply is a bug, not a shortcut.
- An incremental edit changes every bullet the directive names, and no others. One bullet is the common case, not a cap - a directive carrying two clear changes gets both.
