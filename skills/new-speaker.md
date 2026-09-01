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

3. **Refuse collisions.** If the chosen folder already exists in
   `to-process/` (or, for a named speaker, in `processed/` or as a
   `final-output/` PNG), say so and ask how to proceed — never overwrite or
   silently suffix.

4. **Create the folder with two files:**
   - `intake.md` — a copy of the repo-root `intake-template.md` (the blank
     template, NOT the completed example). If the name is known, prefill it
     in both the `speaker-name` body fence and the frontmatter
     `speaker_name`.
   - `README.md` — a copy of `internal/speaker-folder-README.md`.

5. **Tell the operator what the folder still needs**, briefly: fill the
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
