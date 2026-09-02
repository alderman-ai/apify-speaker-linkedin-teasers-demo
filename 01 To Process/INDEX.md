# 01 To Process/ — the intake queue

One subfolder per pending speaker card. Scaffold one by telling the
assistant **"new speaker <Name>"** (the `new-speaker` skill); it arrives
with its own README saying exactly what to drop in (the filled `intake.md`
plus `company-logo` and `speaker` images).

"Process the queue" runs every folder here; each success **moves whole** to
`02 Processed/` and its finished PNG lands in
`03 Generated Images/<speaker>-final.png`. Failures stay put with the reason.
This file is not a speaker folder and is ignored by processing.
