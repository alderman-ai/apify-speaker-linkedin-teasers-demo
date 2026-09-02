# skills/ — the assistant's procedures

Three plain markdown instruction files. Any assistant runs one by reading it
and following it end to end — no runner, no framework; `CLAUDE.md` /
`AGENTS.md` map the operator phrases to them. Each file opens with a short
description of when it applies.

| file | say | what it does |
|---|---|---|
| `novice-walkthrough.md` | "Walk me through making my first speaker card" | the guided first run for someone who has never used GitHub or an AI tool: preflight, folder tour, then one question at a time through scaffolding, the form, the images and the render — **a wrapper**, it reads and follows the two skills below rather than repeating them |
| `new-speaker.md` | "new speaker Alex Alderman" | scaffolds one folder in `01 To Process/` with the blank form and its README |
| `apify-speaker-card.md` | "process the queue" | the generator: validate the form → measure the template → render the card → composite → verify → PNG to `03 Generated Images/`, folder to `02 Processed/` |

The core templates the skills consume are locked — drift is auto-restored
from git, or from the reference copies in `_internal/fonts/jic/` when there
is no git history (a ZIP download); changes are maintainer-only (see
`CLAUDE.md`).
