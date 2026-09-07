# _internal/ — machinery

Everything in here is used by the assistant's procedures and never edited
day to day. Operators: nothing for you here. Assistants: reference these
files; do not restructure this folder.

| path | what |
|---|---|
| `skills/` | the two procedure files the assistant runs end to end: `new-speaker.md` (scaffold a speaker folder, then fill it by hand or from chat) and `apify-speaker-card.md` (the whole generator, including the menu's line 1 flow) |
| `demo-speaker/` | the bundled demo speaker — the repo author's complete, filled speaker folder plus its finished card, `alex-alderman-final.png`. The worked example: line 1 of the session menu opens that PNG for the visitor before asking anything, and the form is what "done" looks like. Not an input to line 1; never process or edit it in place — an explicit request to render it copies it into `to-process/` under the duplicate rule |
| `core-templates-please-dont-touch/` | the two source-of-truth files everything is generated from: the canon visual template and the blank intake form. **Edit nothing in there in passing** — its README explains why, and how a deliberate change is versioned |
| `render/` | the render shell (verified card CSS + the speaker element + tokens), the static footer icon, and `particles.svg` (the starfield's source pattern) |
| `fonts/` | self-hosted Inter + IBM Plex Mono (OFL) the shell links; renders never touch a network |
| `speaker-folder-README.md` | template copied into each new speaker folder as its `README.md` |
