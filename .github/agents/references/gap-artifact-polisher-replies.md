# GAP artifact polisher: reply format and example

Read by the `gap-artifact-polisher` agent before writing any reply.

## Draft reply sections

Judge the content against the knowledge base, not against a template. Give each section at least one suggestion, or praise the part that already works best.

1. Problem and purpose: is the reason for the work clear and specific (S15 "Problem it solved", S14-01)? Give the **Strongest** line in the Problem, then one **Suggestion** to make it clearer, with a **Reason**.
2. Numbers: is there a baseline, a delta and a consequence (S16-03)? If the figure is an estimate or there is no baseline, is the method stated (S16-04)? Ask one question per missing number.
3. Proof: is there a link or image the panel can check (S14-04, S15 Proof, 19.1)? Give the **Strongest** proof, then one **Suggestion** for stronger proof, with a **Reason**, or ask for one.
4. Competency tags: read the Point, Problem and Consequence together. Suggest only the competencies the text strongly supports, named in full (for example, Test Automation, not AUT) (S14-03, S11-01, Appendix A). For each, give the **Suggested** competency and a **Reason** that quotes the words in the Point, Problem or Consequence that support it. Phrase the list as suggestions, such as "based on this, X may fit best", never as tiers or importance. Do not pad the list to reach a number.
5. Level and wording: is the work described in the language of the target level (S07, N07)? Give the **Strongest** sentence, then one **Suggestion** to shorten the longest sentence, with a **Reason**.
6. Questions: only when the text is too thin for sections 1 to 5, ask the **Question** bullets described under Reply format.

Phrase each suggestion as an improvement, for example "It would be clearer to...". Do not use "must" or "required" for wording suggestions.

## Reply format

- One `###` heading per section, with bullets only (no paragraphs). Every section, including Competency tags, uses bullets.
- Every **Suggestion** bullet is followed by a **Reason** bullet saying why the change helps. Never leave a Suggestion without a Reason.
- Each **Suggestion** names the concrete change to make (what to add, rename or measure), not only the problem.
- Competency tags: one bullet per competency, each followed by a **Reason** bullet. There is no fixed count; suggest only strongly supported competencies. If the text supports them only weakly, name the closest fit and say so in its **Reason**.
- Questions: add a `### Questions` section only when a claim's text is too thin to judge a competency or number. Use one **Question** bullet per missing item, at most three, asking for the exact detail needed (for example, the baseline figure or the competency the test covered). Omit the section otherwise.
- Bold labels: **Strongest**, **Suggestion**, **Reason**, **Suggested**, **Question**.
- End each **Strongest** bullet with ✅ and each **Suggestion** bullet with ⚠️, after the last word.
- Use only ✅ and ⚠️ as status icons.
- Keep each section other than Competency tags to 2 or 3 bullets. Use a table of missing items when there are two or more artifacts.
- Do not include claim phrasings or description bullets. Do not name tiers (Critical, Important, Supporting).

## Example

Input block:

```
Title: Login regression suite
Point: Automated login regression tests in CI
Level: SE
Problem: Manual login checks took two days per release.
Before: 2 days per release
After: 2 hours per release
Consequence: Releases are checked every sprint.
```

Reply, Problem section:

### Problem and purpose

- **Strongest:** The release delay is named and measured. ✅
- **Suggestion:** It would be clearer to name the release that was blocked. ⚠️
- **Reason:** A named release makes the problem easy to check.
