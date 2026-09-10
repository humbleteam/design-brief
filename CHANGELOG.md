# Changelog

## [1.4.0] - 2026-09-10

- Fixed the count ranges on the three optional bullets, which were written as requirements and had no state for a bullet that is stated but short. Step 3 asks for 3-5 must-haves and 2-4 constraints, and a real input rarely lands inside either range: the README's own worked example ships two must-haves and one constraint, under both floors, with no rule permitting it. The three moves available were all illegal - pad the bullet to the floor, which invents a non-negotiable and breaks the skill's first rule; mark it `(not stated - ...)`, which is false when two entries are stated and re-asks for what the input already gave; or ship short and silent, which is what the example did.
- The counts are now defined as the shape to aim for, not a gate. A bullet is stated once the input supports one real entry, and it ships with what the input supports plus a shortfall marker inside the bullet: `(2 of 3-5 - what else is non-negotiable here?)`. `(not stated - ...)` is now the zero-entry marker only, and Problem and Audience are still the only two bullets that can block a brief.
- Above the ceiling nothing is cut. Nine stated must-haves all ship, with one question under the brief about which are truly non-negotiable, because trimming to five drops information the team gave and the input never says which four matter least.
- Named the case that manufactures the shortfall: Step 3 requires dropping unenforceable constraints, so a brief that starts with three and drops two ends at one. That is a correct outcome recorded as a shortfall, not a reason to put "make it modern" back.
- Step 6 covers removal directives with the same rule, and a bullet a directive empties completely is marked `(none - removed on this edit)` rather than `(not stated - ...)`, which would ask the user for what they just removed.
- Fixed the README example, which was the one place the unmarked short bullet appeared, and it now shows the marker clearing itself when the follow-up edit takes Constraints to two. New FAQ answer on a brief with only two must-haves, four new edge cases, and a rule in "Rules that hold in every mode".

## [1.3.0] - 2026-09-05

- Fixed the source footer, which shipped in two different shapes without a rule saying which one applies when. Step 5's template names all five bullets one by one (`Problem: <x>; Audience: <x>; ...`) and the edge cases say provenance is per bullet, not per brief, but the README's fresh-brief example collapsed all five onto one shared source (`Problem, Audience, Success metric, Must-haves, Constraints: kickoff call notes, July 8`) while the incremental-edit example two blocks below used the per-bullet form. A reader had to work out which shape they were looking at before they could tell where a bullet came from, and nothing said whether the short form was a permitted compression or a mistake.
- One shape, always: the footer names all five bullets separately, including when a single input answered every one of them. Grouping is defensible only on the day it is written - it has nowhere to put `not found` for a bullet that gap-marked, and the first incremental edit re-sources one bullet and has to expand the line anyway.
- New edge case for a brief whose five bullets all trace back to the same single input, and the rule in "Rules that hold in every mode" now says the footer names every bullet separately rather than only that it is not optional.
- Fixed the README's fresh-brief example, which was the one place the grouped form appeared.

## [1.2.0] - 2026-08-15

- Fixed the incremental-edit path for a directive that changes more than one bullet. Step 6 said to identify "which single bullet the directive changes" and to keep every other bullet byte-for-byte, and the only escape hatch in the edge cases covered an ambiguous directive - one change with an unclear target. A compound directive like "we're B2C now and drop the offline requirement" is the opposite case: two clear targets, nothing ambiguous. It had no legal move. Applying one change and dropping the other silently destroys a fact the user had just given, which is the failure mode Step 6 exists to prevent, and routing it to the clarifying question is worse, because the user can only answer with one bullet and the other change disappears anyway.
- Step 6 now splits a directive into its separate changes and routes on how many bullets they target, first match wins: one clear target, two or more clear targets, one unclear target, or a mix of clear and unclear.
- A mixed directive applies its clear changes and asks about the unclear part alone, naming what was already applied, so a clarifying question never sends understood changes back to the user.
- "Every other bullet stays byte-for-byte" is now measured against the whole directive rather than against the first change found in it, and the source footer updates for each changed bullet.
- New edge cases for a compound directive and for a compound directive with one unclear part, plus a worked two-bullet edit in the README, which showed a fresh brief only while naming incremental edits in its third usage line.

## [1.1.0] - 2026-08-10

- Resolved a deadlock on the most common incremental-edit path. A brief pasted in without a source footer, plus a directive like "now it's B2C", left the skill with two illegal moves and no legal one: the output format and the rules both demand a footer naming an input per bullet, and four of the five bullets had no stated origin anywhere in the input - so it either dropped the footer or invented a document name.
- Untouched bullets in that case are now sourced `carried from the brief as given`, and the changed bullet is sourced to the directive. A brief this skill produced earlier in the conversation carries its existing source names forward unchanged.
- The footer rule now states the honest marker as a legal value, and states plainly that an invented document name is the worse answer - it sends the next reader looking for a file that does not exist.
- New edge cases: a pasted brief with no footer, and a pasted brief whose footer covers only some bullets. Provenance is tracked per bullet, not per brief.

## [1.0.0] - 2026-07-12

- Initial release: fresh-brief extraction (freetext, transcripts, attached docs in - a 5-bullet Problem/Audience/Success metric/Must-haves/Constraints brief out) and a gap report for the two required fields when they are genuinely absent.
- Added the incremental-edit path: a directive folds into the one bullet it changes, every other bullet re-emits byte-for-byte, and the full brief always comes back complete.
- Documented edge cases: conflicting sources, multiple projects in one input, long documents needing section-level citation, and ambiguous directives.
