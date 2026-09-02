# _internal/fonts/jic/

Just-in-case reference copies of the two locked core templates:

| file | reference for |
|---|---|
| `BU_intake-template.md` | `_internal/core-templates-please-dont-touch/intake-template.md` |
| `BU_speaker-teaser-linkedin_v3.png` | `_internal/core-templates-please-dont-touch/speaker-teaser-linkedin_v3.png` |

They exist for one purpose: the drift check both skills run before
scaffolding or rendering. Where the folder is a git checkout, git HEAD is
the reference; **where there is no `.git` — a ZIP download — hash-compare
each canon file against the `BU_` copy here and, on a mismatch, copy the
`BU_` file over the drifted live one and say so.**

These two files must stay **byte-identical** to the canon files. Never
edit them, and never edit a canon file to match a `BU_` copy — the copy
follows canon, not the other way round. They are refreshed only by the
maintainer's local template-update procedure, in the same change that
moves the canon baseline.
