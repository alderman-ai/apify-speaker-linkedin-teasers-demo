# 01 To Process/ — the intake queue

One subfolder per pending speaker card. Scaffold one by telling the
assistant **"new speaker <Name>"** (the `new-speaker` skill), or by asking
it to *"walk me through making my first speaker card"* if this is your
first one; the folder arrives with its own `README.md` saying exactly what
to drop in — the filled-in `intake.md` plus the `company-logo` and
`speaker` images.

Saying **"process the queue"** runs every folder here. The pipeline stages:
`01 To Process/<speaker>/` (pending intake + assets) →
`02 Processed/<speaker>/` (the whole folder, moved on a successful run) →
`03 Generated Images/<speaker-name>-final.png` (the render itself). A
folder that fails validation stays put, with the reason reported and
nothing written.

**Nothing is ever overwritten** — scaffolding a speaker whose name is
already taken here creates `<name>-<NN>`, the lowest free number,
zero-padded, first duplicate = `01`, and that same `NN` carries through to
the archived folder in `02 Processed/` and the PNG in
`03 Generated Images/`. No existing folder is cleared or renamed to make
room; a repeat name is legitimate (a rebuilt card, a fresh start after
text edits, a namesake).

This file is not a speaker folder and is ignored by processing.
