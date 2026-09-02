# 03 Generated Images/ — the deliverables

One finished 1200×1200 PNG per speaker, named `<speaker-name>-final.png`.
This is the only folder anything gets published from — grab the image here,
not from `02 Processed/`.

The pipeline stages: `01 To Process/<speaker>/` (pending intake + assets) →
`02 Processed/<speaker>/` (the archived folder after a successful run) →
`03 Generated Images/<speaker-name>-final.png` (the render itself).

**Nothing here is ever overwritten** — a re-render of a speaker whose
filename is already taken lands as `<speaker-name>-<NN>-final.png`, the
lowest free number, zero-padded, first duplicate = `01`, carrying the same
`NN` as the archived folder in `02 Processed/`. You never move or rename
an existing final to make room.

`alex-alderman-final.png` is the live worked example. The demo-era renders
(fictional characters and the simulated-"real" stress-test speakers) were
archived into `04 Demo and More Help/Example Speakers/Fictional Characters/` and
`04 Demo and More Help/Example Speakers/Real People Stress Test/`.
