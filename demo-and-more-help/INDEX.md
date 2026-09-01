# demo-and-more-help/

Two jobs, one folder:

1. **See what this demo is** — this repo is Alex Alderman's live demo of a
   docs-driven, script-free image pipeline: an AI assistant reads plain
   markdown procedures and turns one small form per speaker into a
   finished LinkedIn teaser PNG. The finished results are in
   `generated-images/`; the evaluation below shows how hard it was tested.
2. **Get unstuck** — you read the root `README.md` and still aren't sure
   what goes where or how much text fits: the annotated graphics and the
   filled-in example here answer that.

| file | what |
|---|---|
| `scripts-and-security.md` | **read this if you're wary of cloned repos** — a plain-English inventory of every piece of code in this project, what it does, and when it runs (short version: there are no executable scripts at all) |
| `intake-template-completed-example.md` | the intake form with every box filled in — exactly what "done" looks like before you say "process the queue" |
| `Actor card field mapping.png` | the annotated card: every form field and where it lands on the image. Show this to anyone filling in a form |
| `Actor card text budgets.png` | the character budgets for every text field, as a table |
| `field-mapping.html` / `text-budgets.html` | the sources of those two graphics; re-render with headless Chrome (the generator skill has the flags), window sizes 1400x440 and 1400x560 |
| `pipeline-evaluation.md` | the 2026-09-01 stress test: ten speakers, two independent executors reading the docs cold — what it proved and what it found |
| `fictional-speakers-as-demo/` | the fictional demo speakers — folklore characters and synthetic personas — archived out of the live queue: their `fictional-processed/` folders and `fictional-generated-images/` PNGs. Browse these to see the range of what the pipeline produces |
| `mid-project-snapshots/` | the simulated-"real" stress-test speakers (real Czech public figures used during development), kept as snapshots: their `processed/` folders and `generated-images/` PNGs |

Lost beyond what's here? Just ask the assistant in plain words — "what is
this repo?", "how do I make a card?" — orientation is what it's for.
