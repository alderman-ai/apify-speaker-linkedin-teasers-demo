# Architecture: text and two images in, PNG out

One speaker folder goes in; one 1200x1200 PNG comes out. Nothing in
between is a program.

## The pipeline

```
to-process/<speaker>/                _internal/render/
  intake.md  (fences -> YAML)   -->  shell.html  --copy-->  _run-<name>.html
  company-logo.*                        {{TOKEN}}s filled: geometry, text,
  speaker.*  (roughly square)            file:/// image URIs
        |                                        |
        v                                        v
   validate + ratio check              headless Chromium --screenshot
   (halt, never degrade)                (page = the canon 1200x1200
                                         template as background, both
                                         elements absolutely positioned
                                         on it at the fixed geometry)
                                                 |
                                                 v
                              generated-images/<speaker>-final.png
                              to-process/<speaker>/ -> processed/<speaker>/
```

`intake.md` is the machine record and the contract. Operator values live in
labelled body fences (fenced blocks tagged `speaker-name`,
`presentation-description-115-char-max`, and so on); processing transfers each
fence into its matching YAML frontmatter key -- fences win. Frontmatter that
differs from the core template in any non-fence key is reverted before
anything renders, because section 6 of that frontmatter holds the eight
geometry constants (`card_x/y/w/h`, `speaker_x/y/w/h`) measured once when
template v4 was built. A run reads them verbatim and never measures the
image.

Validation halts rather than degrades: over-budget description, a missing
asset, a non-square or oversized photo, or a card-ratio mismatch
(`card_w / card_width` checked against the card's quantised real height,
+/-2px) each stop that folder with a printed reason. Card height is quantised
by description line count, so the card is never stretched to fit a block.

The render page carries a pixel-verified reproduction of apify.com's
`ActorStoreItem` CSS plus the `.SpeakerCard` element, and links
`../fonts/fonts.css` -- Inter and IBM Plex Mono, self-hosted, so a render
never touches a network. The screenshot is the composite; there is no second
image step.

## The deliberate decision

No build scripts, no package manifests, no network. The agentic assistant is
the engine: the "skills" are plain markdown procedures in `_internal/skills/`
that any harness can follow by reading them, not deployed harness skills that
auto-load. `CLAUDE.md` / `AGENTS.md` (identical copies) route operator intent
to the right one. The single external requirement is a Chromium-based browser
already on the machine. See
[../about-this-project/scripts-and-security.md](../about-this-project/scripts-and-security.md)
for the itemised code inventory -- zero executable scripts.

## Queue and layout

Three stages, one folder in exactly one of them: `to-process/` (pending),
`processed/` (archive after success), `generated-images/` (the deliverable).
Nothing is ever overwritten -- a taken name gets the lowest free zero-padded
`-NN` suffix (first dupe = `01`), the same `NN` on both the archive folder and
the PNG, detected by folder/file names only.

```
to-process/ processed/ generated-images/     the queue
_internal/skills/                            the two markdown procedures
_internal/render/                            shell.html, footer icon, particles
_internal/fonts/                             Inter + IBM Plex Mono (OFL)
_internal/core-templates-please-dont-touch/  intake-template.md (v5) + the v4 PNG
demo-and-more-help/                          help, examples, this page
CLAUDE.md / AGENTS.md                        intent routing
```

The generator's full procedure is
[../../_internal/skills/apify-speaker-card.md](../../_internal/skills/apify-speaker-card.md).

Back to the [index](INDEX.md).
