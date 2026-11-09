# GAP artifact polisher: claim schema and checks

Read by the `gap-artifact-polisher` agent before writing any claim.

## Claim fields

`id`, `point`, `claim`, `description` (HTML `<ul><li>` allowed), `competencies` (codes), `links` ([{url,label}]), `images`, `dataset` (legacy, no longer created), `problem`, `before`, `after`, `consequence`, `method`, `figureStatus`, `proofStatus`, `cycle`, `doneOn` (YYYY-MM), `selectedForPack`, `level`, `createdAt`, `status` (`draft` or `ready`; missing means ready).

Use only the 12 codes in Appendix A: OWN, DOM, TST, BUG, TPS, COM, TLS, PSC, AUT, LDR, LRN, CID.

## Suggestions file

`suggestions.json` sits at `source/suggestions/suggestions.json`, separate from the data file, and is not part of the claims. Shape: `{version, candidate, level, exportedAt, suggestions: [{claimId, agentSuggestion}]}`. Each `agentSuggestion` is the agent's reply for that claim, shown on the Suggestions page. The file is git-ignored.

## Input document

The user gives a document (attached or by path) with one block per artifact. Labels are case-insensitive and order does not matter:

- Title: artifact name, used only in the report.
- Point: one-line summary of the work.
- Problem: why the work existed.
- Before: baseline with a number and unit, or "no baseline".
- After: result with a number and unit.
- Consequence: what the change enabled.
- Method: how an estimated or no-baseline figure was reached. Required only when Before is an estimate or "no baseline".
- Proof: at least one link (http or https) or image path.

Any other text in a block is optional context for suggestions.

A `Level:` line sets the target level: SE, SSE or LE. ASE has no matrix in the knowledge base, so skip the tier checks and say so.

## Evidence check

Checked per artifact, using the knowledge base's evidence guidance. Missing items are listed in the reply and left blank in the JSON. The check describes what is missing; it does not block writing.

1. Point: the artifact in one line.
2. Problem: why the work existed (S15, S14-01).
3. Before: a number and unit, or "no baseline" plus a Method (S16-04). Never invent a number.
4. After: a number and unit (S16-03).
5. Consequence: baseline, delta and consequence (S16-03).
6. Proof: at least one link or image (S14-04, S15).

## Suggested fields

The user can untick or edit any field in the app.

- `claim`: one sentence from `point`.
- `description`: 2 to 4 `<ul><li>` bullets from the notes.
- `competencies`: primary first, then the secondary codes the work evidently touches (S14-03 for one primary; S11-01 suggests four to six). Use only the 12 codes in Appendix A.
- `figureStatus`: `measured` when the notes name a source for the figures, `estimated` when they say estimate, `no-baseline` when there is no baseline.
- `proofStatus`: `needs-proof` when a proof item is weak or missing, `verified` when the notes say the proof was checked, otherwise `self-reported`.
- `doneOn` (YYYY-MM) and `cycle`: only when the notes state them; otherwise empty.
- `selectedForPack`: true only when all six Evidence check items are present (the S15 pack contents).
- Flag units that differ between `before` and `after`, and Critical competencies for the target level (its tier list in section 9.1 to 9.3) with no primary artifact across the file (S15-06, section 14.4).
- Keep every number and link exactly as given. Emojis are not allowed in JSON fields.
