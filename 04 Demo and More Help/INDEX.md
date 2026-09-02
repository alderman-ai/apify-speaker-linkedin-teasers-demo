# 04 Demo and More Help/

The operator-facing help and showcase folder: reference material, worked
examples, and documentation about the project itself. **Nothing here is
read by the generator at run time** — it exists for humans, and for
assistants orienting a human.

The folder root holds only this file and `README.md` (the same routing,
written for a person rather than an assistant). Everything else lives in
its subfolders.

## Routing — where to send someone

| folder | send them here when | contains |
|---|---|---|
| `Getting Started/` | they have never used GitHub or an AI coding tool, don't know where to start, or say it's their first time | the six-step human entrance — download, install an assistant, open the folder, paste one sentence — which routes them to `_internal/skills/novice-walkthrough.md` |
| `Filling in the Form/` | they're writing an intake and don't know what a field means, where it lands on the card, or how much text fits | the annotated field-mapping graphic, the character-budget table, both HTML sources, and a fully filled-in example intake |
| `Example Speakers/` | they want to see the range of output, or a real example of a completed speaker folder | twelve speakers already through the pipeline — finished PNGs plus the archived folders that produced them, split into `Fictional Characters/` (5 folklore characters) and `Mid-Project Snapshots/` (7 — the stress-test five plus two early synthetic personas) |
| `About This Project/` | they're wary of running a cloned repo, or want evidence the pipeline holds up | the code-and-security inventory and the ten-speaker stress-test report |

Each subfolder carries its own `INDEX.md` with file-level detail.

## Catalogue

```
README.md                          human orientation (points here)
INDEX.md                           you are here

Getting Started/
  getting-started.md               the beginner entrance: six steps from
                                   "download the folder" to "post it"

Filling in the Form/
  Actor card field mapping.png     annotated card — every field, where it lands
  Actor card text budgets.png      character budget per text field, as a table
  intake-template-completed-example.md   what a finished intake looks like
  field-mapping.html               source of the mapping graphic (1400x440)
  text-budgets.html                source of the budgets graphic (1400x560)

Example Speakers/
  Fictional Characters/            5 speakers — folklore characters
    Generated Images/              their finished 1200x1200 PNGs
    Processed/                     their archived folders (form + assets)
  Mid-Project Snapshots/           7 speakers — 5 real Czech public figures
                                   used as stress-test material, plus 2 early
                                   synthetic personas
    Generated Images/              their finished 1200x1200 PNGs
    Processed/                     their archived folders (form + assets)

About This Project/
  scripts-and-security.md          every piece of code in the repo, and when
                                   it runs (short version: no scripts at all)
  pipeline-evaluation.md           the 2026-09-01 ten-speaker stress test
```

## Three standing cautions

- **Archived intakes under `Example Speakers/` reference the old
  pre-2026-09-01 paths.** They are historical records, deliberately left
  unrewritten — never take a current path from one.
- **`About This Project/scripts-and-security.md` must stay truthful.** If
  any change adds code or scripts anywhere in the repo, update that file
  in the same change.
- **`Getting Started/getting-started.md` is written for a human with no
  technical background.** It deliberately points at no assistant-facing
  file — no `CLAUDE.md`, no `INDEX.md`, no skills — except one line saying
  the assistant reads `CLAUDE.md` by itself. Keep it that way if you edit
  it.

Lost beyond what's here? Just ask the assistant in plain words — "what is
this repo?", "how do I make a card?" — orientation is what it's for.
