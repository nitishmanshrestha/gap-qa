---
description: Architecture, data model and export rules for the GAP Handim single-file app. Use when editing JavaScript, saving or loading claims, or the HTML slides/markdown export.
applyTo: "**/*.html"
---

# App architecture

## Data

- Stored as `{ version: 1, candidate: {name, project}, claims: [...] }` in a user-owned JSON file via the File System Access API. `commitClaims()` is the only write path; it checks `lastModified` before writing.
- Claim fields: `id, point, claim, description (sanitized HTML), competencies[], links[{url,label}], images[] (data URLs), problem, before, after, consequence, method, figureStatus, proofStatus (`verified|self-reported|needs-proof`or absent), cycle, doneOn (YYYY-MM), selectedForPack, level, dataset (legacy: kept if present in saved JSON, no longer created or edited;`{name, headers[], rows[][], valueCol, chart}` or null), createdAt`.
- Links may be legacy strings; read them through `asLink`.
- New fields must be optional and defaulted when reading.

## Where things live

- Competencies, levels and tiers: `COMPETENCIES`, `LEVELS`, `LEVEL_TIERS`, `tierOf()`. Must match the knowledge base (section 9, Appendix E).
- Form read/write: `readEvidenceFields`, `fillEvidenceFields`, `saveClaim`, `editClaim`, `resetForm`.
- Shared row builder: `impactRows(claim)` feeds the card, the markdown pack and the HTML slides. Add new evidence fields there once.

## HTML slides export (button: Export HTML slides)

- `exportDeck()` (code name kept) asks for a folder (`showDirectoryPicker`) and writes `<name>_<level>_<mon>_<dd>_<hhmm>/` with `index.html`, `styles.css`, `data.json` and `images/`. The name comes from `deckFolderName()`.
- `deckHtml(claims, shots)` builds slides from `DECK_CSS` and `DECK_JS`; the slides have no external requests and work from `file://`. Images are written as files, never embedded.
- Artifact slides show a thumbnail strip; clicking a thumbnail opens a zoom carousel (Prev/Next, arrow keys, click to enlarge).
- Every user string goes through `esc()`, rich text through `sanitizeHtml()`, links must be `http(s)`, image data URLs must pass `SAFE_IMAGE`.
- Never put `</script>` literally in `DECK_JS`; the page would end its own script block.
- Test by calling `deckHtml` with sample claims in the page and rendering it.

## Safety

- Escape with `esc()`, sanitize rich text with `sanitizeHtml()`, restrict image data URLs with `SAFE_IMAGE`, only allow `http(s)` links.
- No network calls from app logic. UI fonts come from the Google Fonts links in `<head>`; the HTML slides use system fonts and load nothing. No secrets in the file.
