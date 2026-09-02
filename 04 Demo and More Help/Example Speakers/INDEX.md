# example-speakers/

Twelve speakers that have already been through the pipeline, archived out
of the live queue and kept purely as a showcase. Each half mirrors the
live tree shape, so a folder here reads exactly like a folder there:

- `processed/<speaker>/` — the archived speaker folder: `intake.md`,
  `README.md`, `company-logo.*`, `speaker.*`, exactly as they ran.
- `generated-images/<speaker>-final.png` — the finished 1200×1200 result.

| folder | speakers | what it's for |
|---|---|---|
| `fictional-characters/` | 7 — folklore characters (Baba Yaga, Krteček, Pat a Mat, Santa Claus, Vodník) and synthetic personas (Jana Novakova, Petr Svoboda) | showing the range of what the pipeline produces, with nobody real involved |
| `real-people-stress-test/` | 5 — real Czech public figures (Antonín Dvořák, Jaromír Jágr, Karel Čapek, Martina Navrátilová, Václav Havel) | the 2026-09-01 stress test, kept as a record of the project mid-flight |

## Three cautions

- **Archived intakes reference the old pre-2026-09-01 paths.** They are
  historical records, deliberately not rewritten — never take a current
  path from one.
- **Some fictional portraits and one logo derive from CC BY-SA sources.**
  Publishing those images carries an attribution obligation; the credit
  lines are in the prep manifests inside the folders.
- **The real-people set uses real names and likenesses for an internal
  test.** Don't publish any of it as real event material.

The pipeline's own live output goes to the repo root's `generated-images/`,
not here. Nothing in this folder is ever read at run time.
