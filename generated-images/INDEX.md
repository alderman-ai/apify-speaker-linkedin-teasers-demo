# generated-images/ — the deliverables

One finished 1200×1200 PNG per speaker, named `<speaker-name>-final.png`.
This is the only folder anything gets published from — grab the image here,
not from `processed/`.

The pipeline stages: `to-process/<speaker>/` (pending intake + assets) →
`processed/<speaker>/` (the archived folder after a successful run) →
`generated-images/<speaker-name>-final.png` (the render itself).

**Nothing here is ever overwritten** — re-rendering a speaker requires
moving or renaming their existing final first.

`alex-alderman-final.png` is the author's own card, the live worked example; a
second card for the same name lands beside it as `alex-alderman-<NN>-final.png`.
Cards made from the session menu land here under the speaker's own name.
The demo-era renders
(fictional characters and the simulated-"real" stress-test speakers) were
archived into `demo-and-more-help/example-speakers/fictional-characters/` and
`demo-and-more-help/example-speakers/real-people-stress-test/`.
