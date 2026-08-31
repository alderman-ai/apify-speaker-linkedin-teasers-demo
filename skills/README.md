# skills/ — the assistant's procedures

Two plain markdown instruction files. Any assistant runs one by reading it
and following it end to end — no runner, no framework; `CLAUDE.md` /
`AGENTS.md` map the operator phrases to them. Each file opens with a short
description of when it applies.

| file | say | what it does |
|---|---|---|
| `new-speaker.md` | "new speaker Jana Novakova" | scaffolds one folder in `to-process/` with the blank form and its README |
| `apify-speaker-card.md` | "process the queue" | the generator: validate the form → measure the template → render the card → composite → verify → PNG to `final-output/`, folder to `processed/` |
