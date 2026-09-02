# _internal/ — machinery

Everything in here is used by the assistant's procedures and never edited
day to day. Operators: nothing for you here. Assistants: reference these
files; do not restructure this folder.

| path | what |
|---|---|
| `skills/` | the two procedure files the assistant runs end to end: `new-speaker.md` (scaffold a speaker folder and fill it from chat) and `apify-speaker-card.md` (the whole generator, including the demo run) |
| `demo-speaker/` | the bundled demo speaker — a complete, ready-to-run speaker folder for the repo author. Line 1 of the session menu copies it into `to-process/` and processes the copy; never process or edit it in place |
| `core-templates-please-dont-touch/` | the two source-of-truth files everything is generated from: the canon visual template and the blank intake form. **Edit nothing in there in passing** — its README explains why, and how a deliberate change is versioned |
| `render/` | the render shell (verified card CSS + the speaker element + tokens), the static footer icon, and `particles.svg` (the starfield's source pattern) |
| `fonts/` | self-hosted Inter + IBM Plex Mono (OFL) the shell links; renders never touch a network |
| `speaker-folder-README.md` | template copied into each new speaker folder as its `README.md` |
| `sample-assets/` | synthetic images used by the retired fictional worked examples in `demo-and-more-help/example-speakers/fictional-characters/` |
