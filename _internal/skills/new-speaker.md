---
name: new-speaker
description: >
  Adds a new speaker folder to the queue and collects that speaker's
  details in chat — name, role, company, topic, minutes, blurb and
  the two images — writing them into the form itself. Use when the
  operator says "new speaker", "/new-speaker", "add a speaker", "scaffold a
  speaker folder", "make me a card for <name>", or names a person to add
  to the lineup.
---

# new-speaker — scaffold one speaker folder and fill it from chat

Creates one folder under `to-process/` holding everything one card needs —
the intake form and the two images — and fills the form from what the
operator tells you in chat. The operator never has to open the form.

## Procedure

1. **Ask for the speaker's name** — one question, e.g. *"Speaker's name? (or
   'skip' to use a numbered folder)"*. If the operator already gave a name in
   their request, don't re-ask.

2. **Pick the folder name.**
   - **Name given** → kebab-case it: lowercase, every run of non-alphanumerics
     becomes a single `-`, trimmed — `Alex Alderman` → `alex-alderman`.
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

4. **Create the folder with two files:**
   - `intake.md` — a copy of
     `_internal/core-templates-please-dont-touch/intake-template.md` (the
     blank template, NOT the completed example in
     `demo-and-more-help/filling-in-the-form/` and NOT the bundled demo
     speaker in `_internal/demo-speaker/`).
     If the name is known, prefill it in both the `speaker-name` body fence
     and the frontmatter `speaker_name`.
   - `README.md` — a copy of `_internal/speaker-folder-README.md`.

5. **Collect the rest in chat.** In one message, ask for everything still
   missing, each with its rule, so the operator can answer in a single
   reply (in any order, across several messages if they prefer):
   - **position** and **company** — lowercase kebab-case each; together
     they fit 39 characters including the ` / ` joiner
   - **topic category** — Title Case, up to 26 characters
   - **duration** — minutes, number only
   - **blurb** — one plain paragraph, up to 115 characters
   - **company logo** — a path to a square image, ideally 80×80 or larger
   - **speaker photo** — a path to an exactly square PNG/JPG/JPEG, at most
     800×800 (262×262 is the perfect fit); it is scaled into the slot,
     never cropped or reframed, so the operator squares it themselves

   Anything the operator already said in their request counts as given —
   don't re-ask for it. If they say they'd rather fill the form by hand,
   stop here and tell them what the folder still needs (step 7).

6. **Write each value into the form as it arrives** — into its labelled
   body fence AND its frontmatter key, so the two never disagree.
   Validate on the way in, out loud and briefly:
   - count the blurb; over 100 → say the count and ask for a shorter one —
     **never trim it yourself**
   - position/company not lowercase-kebab → offer the kebab-cased form and
     use it only on a yes; never silently alter what they typed
   - name over 30, topic over 26, position+company over 39 → warn (the card
     ellipsises these; the operator decides)
   - images: **copy** the file into the folder as `company-logo.<ext>` /
     `speaker.<ext>` — never move or edit the operator's original. Read the
     photo's real dimensions now: non-square, larger than 800×800, or
     another format → report the actual size and ask for a square resubmit
     rather than letting it fail at processing time.

7. **Close the loop.** If anything is still missing, list exactly what (the
   folder's README says the same). If everything is present: say the
   folder is ready and — if the operator asked for the image, not just the
   folder — hand straight over to the `apify-speaker-card` skill for that
   one folder without asking again. Otherwise tell them to say the word
   ("process the queue") when they want it rendered.

## Notes

- A numbered folder is renamed to the speaker's kebab name automatically at
  processing time, taken from the form's `speaker_name`.
- A form the operator filled by hand (fences typed, frontmatter untouched)
  is still accepted as is — the generator transfers the fences at
  processing time. Chat input is the documented path; hand-filling is
  merely not forbidden.
- This skill only scaffolds and fills. Generation belongs to the
  `apify-speaker-card` skill.
