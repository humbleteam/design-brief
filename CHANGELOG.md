# Changelog

## [1.1.0] - 2026-08-10

- Resolved a deadlock on the most common incremental-edit path. A brief pasted in without a source footer, plus a directive like "now it's B2C", left the skill with two illegal moves and no legal one: the output format and the rules both demand a footer naming an input per bullet, and four of the five bullets had no stated origin anywhere in the input - so it either dropped the footer or invented a document name.
- Untouched bullets in that case are now sourced `carried from the brief as given`, and the changed bullet is sourced to the directive. A brief this skill produced earlier in the conversation carries its existing source names forward unchanged.
- The footer rule now states the honest marker as a legal value, and states plainly that an invented document name is the worse answer - it sends the next reader looking for a file that does not exist.
- New edge cases: a pasted brief with no footer, and a pasted brief whose footer covers only some bullets. Provenance is tracked per bullet, not per brief.

## [1.0.0] - 2026-07-12

- Initial release: fresh-brief extraction (freetext, transcripts, attached docs in - a 5-bullet Problem/Audience/Success metric/Must-haves/Constraints brief out) and a gap report for the two required fields when they are genuinely absent.
- Added the incremental-edit path: a directive folds into the one bullet it changes, every other bullet re-emits byte-for-byte, and the full brief always comes back complete.
- Documented edge cases: conflicting sources, multiple projects in one input, long documents needing section-level citation, and ambiguous directives.
