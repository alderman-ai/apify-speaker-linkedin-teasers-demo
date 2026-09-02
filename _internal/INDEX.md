# _internal/ — machinery

Everything in here is used by the assistant's procedures and never edited
day to day. Operators: nothing for you here. Assistants: reference these
files; do not restructure this folder.

| path | what |
|---|---|
| `skills/` | the three procedure files the assistant runs end to end: `novice-walkthrough.md` (guided first run for a complete beginner — a wrapper over the other two), `new-speaker.md` (scaffold a speaker folder) and `apify-speaker-card.md` (the whole generator) |
| `core-templates-please-dont-touch/` | the two source-of-truth files everything is generated from: the canon visual template and the blank intake form. **Edit nothing in there** — its README explains why, and how to restore |
| `render/` | the render shell (verified card CSS + the speaker element + tokens), the static footer icon, and `particles.svg` (the starfield's source pattern) |
| `fonts/` | self-hosted Inter + IBM Plex Mono (OFL) the shell links; renders never touch a network. Its `jic/` subfolder holds byte-identical reference copies of the two core templates, used for the drift check when there is no `.git` (a ZIP download) |
| `speaker-folder-README.md` | template copied into each new speaker folder as its `README.md` |
| `sample-assets/` | synthetic images used by the retired fictional worked examples in `04 Demo and More Help/Example Speakers/Fictional Characters/` |
