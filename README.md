# GAP Handim

A single-file tool that helps QA engineers organize, tag and export their achievements for a GAP promotion case.

![GAP Handim tagger](images/app-screenshot.png)

## Who this is for

QA Chapter engineers preparing a GAP promotion case. Pick your target level in the app. ASE has no tier list yet. Your evidence stays in your own JSON file.

## Find your situation

| Situation      | You want to                                     | Go to                                                              |
| -------------- | ----------------------------------------------- | ------------------------------------------------------------------ |
| Starting fresh | Create a data file and add your first artifacts | [Starting fresh](#starting-fresh)                                  |
| Continuing     | Reopen a saved file and add more                | [Continuing with saved data](#continuing-with-saved-data)          |
| Streamlining   | Cut a long list to the strongest artifacts      | [Streamlining a long list](#streamlining-a-long-list)              |
| Consolidating  | Export the HTML slides or Markdown              | [Consolidating the output](#consolidating-the-output)              |
| Drafting       | Turn notes into claims with the agent           | [Drafting claims from notes](#drafting-claims-from-notes-optional) |

## Folders

| Folder         | Contents                                                       |
| -------------- | -------------------------------------------------------------- |
| `view/`        | The app. Open `view/gap-handim-tagger.html` in Edge or Chrome. |
| `source/`      | Your data files, including the file the agent writes.          |
| `images/`      | Screenshots used in this README.                               |
| `source-deck/` | The original GAP deck. Do not edit.                            |
| `knowledge/`   | The GAP competency matrix knowledge base.                      |

## Using the app

### Starting fresh

1. Open `view/gap-handim-tagger.html` in Edge or Chrome.
2. Enter the candidate name. Project is optional. Pick the target level. Each level shows only its own artifacts and coverage.
3. Click **Start** and save or choose the suggested file, for example `your-name-l3.json`.

| If the file is   | Then                                   |
| ---------------- | -------------------------------------- |
| Existing         | It is loaded.                          |
| New              | It is created.                         |
| Asked to replace | Choose **Replace**. Your data is kept. |

### Adding artifacts

1. Add an artifact and fill in the fields:

| Field                                    | Required | Notes                                                                       |
| ---------------------------------------- | -------- | --------------------------------------------------------------------------- |
| Artifact, Problem it solved, Description | Yes      |                                                                             |
| Competencies                             | Yes      | Tick what the work proves.                                                  |
| Claim story                              | No       | Drafted from your fields. Edit it, or clear it to resume automatic updates. |
| Impact                                   | No       | Before, after, and how it was measured.                                     |
| Proof                                    | No       | Links, screenshots or a flow.                                               |

2. Click **Save artifact**. Save is blocked until the required fields and one competency are filled. Red tiles in Coverage are critical competencies with no evidence yet.

- **Short on time:** enter the name, the problem and a few words of description, then click **Suggest competency touched**. It ticks likely competencies by keyword match. Check each one.
- **Agent drafts** are tagged Draft and stay out of Coverage, the HTML slides and the Markdown export until you edit them and click **Save artifact**.
- **Copy for agent** copies the artifact for the `gap-artifact-polisher` agent (see below).

### Streamlining a long list

Keep only the strongest artifacts.

1. Edit or delete artifacts as needed.
2. Drag the grip on an artifact to reorder it.

### Consolidating the output

1. Click **Export HTML slides** and choose a parent folder, for example `source/`. The app creates a folder named `<name>_<level>_<mon>_<dd>_<hhmm>` (for example `your-name_le_nov_07_1432`) containing:

| File         | Contents                                                      |
| ------------ | ------------------------------------------------------------- |
| `index.html` | The HTML slides. Open it in any browser. No internet needed.  |
| `styles.css` | The slide styling.                                            |
| `data.json`  | A snapshot of the exported claims, with images as file paths. |
| `images/`    | Your screenshots at full resolution.                          |

2. Open `index.html`. The overview lists every artifact as a link, and the Contents button on each slide returns to it. Each artifact slide shows the claim story, figures, description, links and screenshot thumbnails together (a very long claim story moves to its own slide). Click a thumbnail to open a carousel of that artifact's screenshots. Use the arrow keys or the Prev and Next buttons, click a screenshot to enlarge it, and print from the browser for a PDF with one slide per page.
3. Or click **Export Markdown** for a text pack. Files are named `<candidate>-<level>-evidence.md`.
4. Both use all ready artifacts for the current level.

The order you set by dragging is saved in your data file. It sets the artifact numbers in the HTML slides and the order of the Markdown export.

## The agent: `gap-artifact-polisher`

The agent is an optional helper for GitHub Copilot Chat and Claude Code. It turns notes about your artifacts into claims in your JSON file. The app itself has no AI model.

| Assistant      | Where the agent is defined                      | How to call it                                  |
| -------------- | ----------------------------------------------- | ----------------------------------------------- |
| GitHub Copilot | `.github/agents/gap-artifact-polisher.agent.md` | Choose `gap-artifact-polisher` in Copilot Chat. |
| Claude Code    | `.claude/agents/gap-artifact-polisher.md`       | Ask for the `gap-artifact-polisher` subagent.   |

Both follow the same workflow and rules.

### What it does

- Checks each artifact against six required items: Point, Problem, Before, After, Consequence and Proof. Missing items are listed per artifact.
- Drafts every artifact that passes: a claim line, 2 to 4 description bullets, competencies (primary first), figure status and proof status.
- In draft mode, drafts incomplete artifacts, leaves missing numbers and proof empty, and asks one question per missing item.
- Shows all results in one message, then writes nothing until you confirm.
- After you confirm, appends the claims to `source/your-name-<level>.json`, keeps existing claims unchanged and checks that the JSON parses.

### What it does not do

- Invent numbers, links, competencies or policy.
- Change or delete existing claims.
- Edit the knowledge base, the source deck or the app.
- Write outside `source/`.

### Best ways to use it

1. Start the document with a `Level:` line (SE, SSE or LE). Then write one block per artifact with the labels Title, Point, Problem, Before, After, Consequence, Method and Proof. Labels are not case-sensitive and the order does not matter.
2. Fix every artifact the gate lists as failing before you ask for drafts.
3. For incomplete work, use **Copy for agent** in the app, then say "draft".
4. Describe the work in the language of the level you are applying for. The knowledge base says you are assessed against that level, not the one you hold.
5. Plan for about five to six strong artifacts. The agent flags Critical competencies for the target level that have no primary artifact.
6. Check each suggested competency, figure status and proof status in the app before you save.
7. Ask what a competency means, or which knowledge base section supports a suggestion. Answers come from the knowledge base only.

### What to expect

- One review message listing each artifact: passed or failed, what is missing, and for drafts, ranked competencies with knowledge base references, two claim story phrasings, description bullets and questions.
- Confirm all drafts at once, or name the changes and exclusions. Nothing is written before you confirm.
- Written claims are `status: ready` (or `draft`), `level` set to the target level and `createdAt` 2026-11-06.
- Then click **Open existing** in the app and choose the file. Drafts stay out of Coverage, the HTML slides and the Markdown export until you edit them and click **Save artifact**.

### Limitations

- Writes claims for the target level only: the `level` you give in the notes (SE, SSE or LE), with that level's matrix and file.
- Competency, figure and proof suggestions are judgement calls based on your wording. Check them.
- Accepts only `http` or `https` links, and does not open them.
- Uses only the 12 competency codes in Appendix A.
- `createdAt` is fixed to 2026-11-06, not the real date. To change it, edit the JSON and click **Reload**.
- Your notes go to the assistant you use (Copilot or Claude). Do not paste confidential customer data.

### What the knowledge base covers

`knowledge/GAP-competency-matrix-knowledge-base.md` is written from the GAP deck for QA Chapter engineers preparing for the promotion cycle. The agent reads sections 6, 9.1 to 9.3 (the matrix for the target level), 12, 13, 14, 15, 19 and Appendix A.

| Section         | Content                                                              |
| --------------- | -------------------------------------------------------------------- |
| 4               | What GAP is and is not                                               |
| 5               | Eligibility: Team Manager approval, self willing, confirmed with QSG |
| 6               | The 12 competencies and their codes                                  |
| 7               | Tiers: Critical, Important, Supporting                               |
| 8               | Role ladder: ASE, SE, SSE, LE                                        |
| 9               | Role matrices for SE, SSE and LE (no ASE matrix)                     |
| 10 to 11        | One piece of work, many competencies, with a worked example          |
| 12              | Coverage, not collection                                             |
| 13 to 14        | How to prepare and the evidence pack template                        |
| 15              | Numbers and impact: baseline, delta, consequence                     |
| 16              | How the promotion decision is made in three layers                   |
| 17              | Team activity and Q and A                                            |
| 19 to 20        | Rules digest and FAQ                                                 |
| 21              | Source integrity notes: known gaps in the deck                       |
| Appendix A to C | Competency code legend, artifact mappings and glossary               |

The knowledge base marks which facts come from the slides, which are presenter notes, and which are derived. Example numbers in it are illustrative, not benchmarks. Read section 21 before relying on counts or the team activity answers.

## Drafting claims from notes (optional)

Use the `gap-artifact-polisher` agent in GitHub Copilot Chat or Claude Code to draft claims from your notes. Notes you paste go to that assistant. Do not paste confidential customer data.

1. In Copilot Chat, choose `gap-artifact-polisher`. In Claude Code, ask for the `gap-artifact-polisher` subagent.
2. Paste a document with one block per artifact: Point, Problem, Before, After, Consequence, Method (estimates only) and Proof.
3. Complete any artifacts the agent lists as incomplete. It drafts the rest.
4. Confirm the drafts. The agent writes `source/your-name-<level>.json` (for example `source/your-name-le.json`).
5. In the app, click **Open existing** and choose that file. If it is already open, click **Reload**.

Draft mode is for unfinished artifacts. Paste the output of **Copy for agent** and say "draft" (the agent also uses it when required items are blank). The agent skips the required-items gate, saves the artifact with `status: draft`, ranks competencies with knowledge base references, offers wording, and asks one question per missing item. It never fills in numbers or proof.

### Required and suggested

| You provide                                                                                             | The agent suggests                                                                        |
| ------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Point, Problem, Before and After (each with a number and unit), Consequence, Proof (link or image path) | Claim line, description bullets, competencies (first is primary), figure and proof status |
| Method, when Before is an estimate or "no baseline"                                                     |                                                                                           |

Check the suggestions in the review.

## How suggestions and the claim story work

The app has no AI model. Both features are rule-based, run offline and send nothing anywhere.

- **Suggest competency touched** searches the artifact name, description, problem, consequence, before, after and method for trigger words. Each competency has specific trigger words, for example "regression" for TST and "pipeline" for CID, and generic ones, for example "risk" for TPS. Matches in the artifact name or problem count double. A competency is ticked when it has a specific match, or a generic match in the name or problem. It ticks up to five, strongest first, and ties go to the critical tiers. Check each tick.
- **Claim story** is built from the artifact name, problem, description, one sentence per ticked competency and the outcome. It updates as you type until you edit it. Clear the box to return to automatic updates. The empty box shows an example as placeholder text.
- To change the trigger words, edit `COMPETENCY_HINTS` in `view/gap-handim-tagger.html`.

## Continuing with saved data

- Open the app, click **Open existing** and choose your JSON file. If the app is already open, click **Reload**.
- Edited the JSON by hand? Click **Reload**.
- Before big changes, click **Backup** to download a copy of your data file.
- Notes from the agent: see [Drafting claims from notes](#drafting-claims-from-notes-optional).

## Troubleshooting

| Problem                                         | Fix                                                                                     |
| ----------------------------------------------- | --------------------------------------------------------------------------------------- |
| Start is greyed out                             | Enter the candidate name.                                                               |
| Access refused                                  | Click **Grant access** and choose Allow. If there is no prompt, IT policy may block it. |
| "File changed outside the app"                  | The file was reloaded. Review and save again.                                           |
| "This browser cannot create or save data files" | Open the app in Microsoft Edge or Chrome.                                               |
| Export Markdown downloads a file                | The save dialog could not open. Find the file in your Downloads folder.                 |
| Where is my data?                               | Only in your JSON file. Keep a backup.                                                  |
