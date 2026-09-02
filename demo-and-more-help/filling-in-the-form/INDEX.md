# filling-in-the-form/

Everything that answers "what goes in this field, and how much of it?"
Reference material for whoever is writing an intake form — none of it is
read by the generator at run time.

| file | what |
|---|---|
| `Actor card field mapping.png` | the annotated card: every form field with a line drawn to where it lands on the image. Show this to anyone filling in a form |
| `Actor card text budgets.png` | the character budget for every text field, as a table |
| `intake-template-completed-example.md` | the intake form with every fence and frontmatter value filled in — exactly what "done" looks like before it is rendered. Byte-identical to the bundled demo speaker's form in `_internal/demo-speaker/` |
| `field-mapping.html` | the source of `Actor card field mapping.png` — re-renderable with headless Chrome at window size 1400x440 |
| `text-budgets.html` | the source of `Actor card text budgets.png` — same, at window size 1400x640 |

## Scaffold from the template, never from the example

`intake-template-completed-example.md` is a **reference only**. New speaker
folders are scaffolded from the blank contract at
`_internal/core-templates-please-dont-touch/intake-template.md`, and the
assistant fills them from what the operator says in chat. Copying the
completed example into a speaker folder would carry Alex Alderman's
answers in with it — the one place that is meant to happen is the demo
run, which copies `_internal/demo-speaker/` (the same form plus the two
images) into the queue.

## The two HTML files are depth-sensitive

Both link `../../_internal/fonts/fonts.css`, and `field-mapping.html` also
pulls the footer icon from `../../_internal/render/`. Those relative paths
assume the files sit exactly here. Move either file and the render loses
its fonts and icon — repoint the paths in the same change.

`field-mapping.html` contains the only JavaScript in the repo (~25 lines,
drawing the grey connector wires). It is inventoried in
`../about-this-project/scripts-and-security.md`.
