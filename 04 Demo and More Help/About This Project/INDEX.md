# About This Project/

The two documents about the project rather than the pipeline: what code is
in here, and how well it stood up to testing.

| file | what |
|---|---|
| `scripts-and-security.md` | **read this if you're wary of cloned repos** — a plain-English inventory of every piece of code in this project, what it does, and when it runs (short version: there are no executable scripts at all) |
| `pipeline-evaluation.md` | the 2026-09-01 stress test: ten speakers, two independent executors reading the docs cold — what it proved, and the four findings it turned up |

## Keeping the inventory truthful

`scripts-and-security.md` is a promise to anyone who clones this repo. **If
any change adds code or scripts anywhere in the repo, that file must be
updated in the same change.** A stale inventory is worse than none.

## Reading the evaluation

`pipeline-evaluation.md` opens with a note that it names the pre-2026-09-01
layout (`final-output/` is now `03 Generated Images/`; `visual-templates/` and
`skills/` now live under `_internal/`). Its findings are unchanged; only
the paths moved.

The five simulated speakers it used are archived at
`../Example Speakers/Real People Stress Test/`, including the one PNG that
shipped with the known clipped description — finding #1, kept as evidence
rather than quietly regenerated.
