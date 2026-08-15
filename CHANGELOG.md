# Changelog

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
