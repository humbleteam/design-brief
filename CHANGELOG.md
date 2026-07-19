# Changelog

## [1.0.0] - 2026-07-12

- Initial release: fresh-brief extraction (freetext, transcripts, attached docs in - a 5-bullet Problem/Audience/Success metric/Must-haves/Constraints brief out) and a gap report for the two required fields when they are genuinely absent.
- Added the incremental-edit path: a directive folds into the one bullet it changes, every other bullet re-emits byte-for-byte, and the full brief always comes back complete.
- Documented edge cases: conflicting sources, multiple projects in one input, long documents needing section-level citation, and ambiguous directives.
