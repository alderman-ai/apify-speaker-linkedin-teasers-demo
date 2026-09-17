# demo-and-more-help/

The operator-facing help and showcase folder: reference material, worked
examples, and documentation about the project itself. **Nothing here is
read by the generator at run time** — it exists for humans, and for
assistants orienting a human.

The folder root holds only this file and `README.md` (the same routing,
written for a person rather than an assistant). Everything else lives in
one of five subfolders.

## Routing — where to send someone

| folder | send them here when | contains |
|---|---|---|
| `filling-in-the-form/` | they're writing an intake and don't know what a field means, where it lands on the card, or how much text fits | the annotated field-mapping graphic, the character-budget table, both HTML sources, and a fully filled-in example intake |
| `example-speakers/` | they want to see the range of output, or a real example of a completed speaker folder | eighteen speakers already through the pipeline — finished PNGs plus the archived folders that produced them |
| `about-this-project/` | they're wary of running a cloned repo, or want evidence the pipeline holds up | the code-and-security inventory and the ten-speaker stress-test report |
| `probabilistic-vs-deterministic/` | they picked line 2 of the session menu, or want the idea behind the project: why brand assets need deterministic code and probabilistic prose, and how this pipeline combines both | a short explainer in seven one-screen pages, starting at its `INDEX.md` |
| `how-this-was-built/` | they picked line 3 of the session menu, or are technical and want the internals: the architecture, the verified card CSS, the template geometry, the intake contract, the render command, the git timeline and what the live demo added | seven one-screen pages plus `images/`, starting at its `INDEX.md` |

Each subfolder carries its own `INDEX.md` with file-level detail.

## Catalogue

```
README.md                          human orientation (points here)
INDEX.md                           you are here

filling-in-the-form/
  Actor card field mapping.png     annotated card — every field, where it lands
  Actor card text budgets.png      character budget per text field, as a table
  intake-template-completed-example.md   what a finished intake looks like
  field-mapping.html               source of the mapping graphic (1400x440)
  text-budgets.html                source of the budgets graphic (1400x640)

example-speakers/
  fictional-characters/            7 speakers — folklore + synthetic personas
    generated-images/              their finished 1200x1200 PNGs
    processed/                     their archived folders (form + assets)
  real-people-stress-test/         5 speakers — real Czech public figures
    generated-images/              their finished 1200x1200 PNGs
    processed/                     their archived folders (form + assets)
  live-demo-rehearsals/            6 — 2026-09-17 rehearsal runs of the on-stage skill
    generated-images/              their finished 1200x1200 PNGs
    processed/                     their archived folders (form + assets)

about-this-project/
  scripts-and-security.md          every piece of code in the repo, and when
                                   it runs (short version: no scripts at all)
  pipeline-evaluation.md           the 2026-09-01 ten-speaker stress test
  apify-live-demo-qr.png           QR code for alderman.ai/apify-live-demo (slide asset)

probabilistic-vs-deterministic/
  INDEX.md                         the concept in ten sentences + the page list (menu line 2 lands here)
  the-concept.md                   prose vs assets, when each wins, how to split a task
  this-project-as-an-example.md    which pieces here vary, which never do, where the gates sit
  coded-elements-as-the-unifying-layer.md   why a verified CSS card beats an image model
  the-gates.md                     the mechanical checks, with the real numbers
  gallery.md                       the example cards — same code, different words
  try-it-yourself.md               back to menu line 1
  faq.md                           own template? cost? official Apify? — three answers

how-this-was-built/
  INDEX.md                         the build story in ten sentences + the page list (menu line 3 lands here)
  architecture.md                  text + two images in, HTML/CSS render, PNG out; no scripts by design
  the-card-css-reproduction.md     the verified ActorStoreItem CSS and its load-bearing oddities
  template-geometry-and-constants.md   the canon image, the eight constants, how the template evolved
  the-intake-contract-and-gates.md the form as schema; the gates as executable checks
  the-render-step.md               the headless Chromium command, local fonts, no network
  timeline.md                      the git history in seven phases; the stress test's findings
  the-live-demo.md                 what the on-stage version added
  images/                          the author's template drafts, an early render, one screenshot
```

## Two standing cautions

- **Archived intakes under `example-speakers/` reference the old
  pre-2026-09-01 paths.** They are historical records, deliberately left
  unrewritten — never take a current path from one.
- **`about-this-project/scripts-and-security.md` must stay truthful.** If
  any change adds code or scripts anywhere in the repo, update that file
  in the same change.

Lost beyond what's here? Just ask the assistant in plain words — "what is
this repo?", "how do I make a card?" — orientation is what it's for.
