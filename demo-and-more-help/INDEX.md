# demo-and-more-help/

The operator-facing help and showcase folder: reference material, worked
examples, and documentation about the project itself. **Nothing here is
read by the generator at run time** — it exists for humans, and for
assistants orienting a human.

The folder root holds only this file and `README.md` (the same routing,
written for a person rather than an assistant). Everything else lives in
one of three subfolders.

## Routing — where to send someone

| folder | send them here when | contains |
|---|---|---|
| `filling-in-the-form/` | they're writing an intake and don't know what a field means, where it lands on the card, or how much text fits | the annotated field-mapping graphic, the character-budget table, both HTML sources, and a fully filled-in example intake |
| `example-speakers/` | they want to see the range of output, or a real example of a completed speaker folder | twelve speakers already through the pipeline — finished PNGs plus the archived folders that produced them |
| `about-this-project/` | they're wary of running a cloned repo, or want evidence the pipeline holds up | the code-and-security inventory and the ten-speaker stress-test report |

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

about-this-project/
  scripts-and-security.md          every piece of code in the repo, and when
                                   it runs (short version: no scripts at all)
  pipeline-evaluation.md           the 2026-09-01 ten-speaker stress test
  apify-live-demo-qr.png           QR code for alderman.ai/apify-live-demo (slide asset)
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
