# to-process/ — the intake queue

One subfolder per pending speaker card. Name a speaker to the assistant
(the `new-speaker` skill) and it creates the folder, then asks for the
rest of the details and the two images in chat and writes them in itself
— the folder's own README lists what it must end up holding (the filled
`intake.md` plus `company-logo` and `speaker` images). The demo run (line
1 of the session menu) drops a copy of `_internal/demo-speaker/` here.

Processing the queue runs every folder here; each success **moves whole**
to `processed/` and its finished PNG lands in
`generated-images/<speaker>-final.png`. Failures stay put with the reason.
This file is not a speaker folder and is ignored by processing.
