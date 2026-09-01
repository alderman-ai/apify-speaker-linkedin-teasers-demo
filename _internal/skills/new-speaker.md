---
name: new-speaker
description: >
  Adds a new speaker folder to the queue — the single place to drop all the
  visual assets needed to generate that speaker's teaser image. Use when the
  operator says "new speaker", "/new-speaker", "add a speaker", "scaffold a
  speaker folder", or names a person to add to the lineup.
---

# new-speaker — scaffold one speaker folder

Creates one folder under `to-process/` holding everything one card needs:
the intake form and (added by the operator) the two images.

## Procedure

1. **Ask for the speaker's name** — one question, e.g. *"Speaker's name? (or
   'skip' to use a numbered folder)"*. If the operator already gave a name in
   their request, don't re-ask.

2. **Pick the folder name.**
   - **Name given** → kebab-case it: lowercase, every run of non-alphanumerics
     becomes a single `-`, trimmed — `Jana Novakova` → `jana-novakova`.
   - **Declined / unknown** → `new-speaker-<NN>` where **NN is the LOWEST
     available number, zero-padded to two digits**. Scan `to-process/` for
     existing `new-speaker-<digits>` folders, reading any digit format as its
     number (`new-speaker-1` counts as 1, `new-speaker-04` as 4), and take
     the smallest positive integer not in use. Accept N as input; always emit
     NN as output.
     - `new-speaker-02` + `new-speaker-04` exist → create `new-speaker-01`
     - `new-speaker-1` + `new-speaker-3` exist → create `new-speaker-02`

3. **Suffix duplicates — never block, never overwrite.** Repeat names are
   legitimate (a rebuilt card, a fresh start after text edits, a true
   namesake). If the chosen name already exists in `to-process/`,
   `processed/`, or as a `generated-images/` PNG, create `<name>-<NN>`
   instead — NN the LOWEST number free across all three locations,
   zero-padded to two digits, **first dupe = 01**. Say what you created
   and why. Duplicates are detected by folder and file names only; the
   `speaker_name` frontmatter stays the human's real name and may repeat
   freely across folders.

4. **Template integrity preflight — before copying anything.** The blank
   template must be pristine. Compare
   `_internal/core-templates-please-dont-touch/intake-template.md` against
   its reference copy `_internal/fonts/jic/BU_intake-template.md`: the
   frontmatter `version` values must match, and the file hashes must be
   equal. On a mismatch, **scaffold from the reference copy** (`BU_`) —
   never from the drifted live file — then: tell the operator plainly that
   the live template has drifted, and include the tag `MISMATCH` in the
   subject line of any commit made while the drift exists, so it is
   discoverable later. An *intentional* template change is fine — it bumps
   `version`/`versioned_at` and updates both copies together, and then no
   mismatch exists.

5. **Create the folder with two files:**
   - `intake.md` — a copy of
     `_internal/core-templates-please-dont-touch/intake-template.md` (the
     blank template, NOT the completed example in `demo-and-more-help/`).
     If the name is known, prefill it in both the `speaker-name` body fence
     and the frontmatter `speaker_name`.
   - `README.md` — a copy of `_internal/speaker-folder-README.md`.

6. **Tell the operator what the folder still needs**, briefly: fill the
   fenced inputs in `intake.md` (everything to type lives in the "Input
   presentation details here" section; the description fence label is its
   character budget), and drop in the two images —
   `company-logo.(png/jpg)` (square, ideally 80×80+) and the speaker photo:
   **square (1:1), PNG/JPG/JPEG, at most 800×800** — 262×262 is the ideal
   supply, and the generator scales any accepted square into the slot
   without cropping. Then "process the queue" generates the card.

## Notes

- A numbered folder is renamed to the speaker's kebab name automatically at
  processing time, taken from the form's `speaker_name`.
- This skill only scaffolds. Generation belongs to the `apify-speaker-card`
  skill.
