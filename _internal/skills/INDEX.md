# skills/ — the assistant's procedures

Two plain markdown instruction files. Any agentic assistant runs one by
reading it and following it end to end — no runner, no framework;
`CLAUDE.md` / `AGENTS.md` route operator intent to them. Each file opens
with a short description of when it applies.

| file | when | what it does |
|---|---|---|
| `new-speaker.md` | a speaker is named or asked for | scaffolds one folder in `to-process/`, then collects the speaker's details and the two images in chat and writes them into the form |
| `apify-speaker-card.md` | the queue is to be processed, or line 1 of the session menu is picked | the generator: validate the form → load the fixed geometry → render the card → composite → verify → PNG to `generated-images/`, folder to `processed/`. Its "Line 1" section is the demo: name → scaffold → fill-it-yourself or in-chat fork → gate → render |
