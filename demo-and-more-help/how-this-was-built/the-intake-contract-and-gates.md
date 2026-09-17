# The intake contract: one markdown file, executable gates

[`intake-template.md`](../../_internal/core-templates-please-dont-touch/intake-template.md)
is four things at once: the form an operator types into, the schema every
field is validated against, the budget table, and the failure-mode spec.
There is no JSON schema, no validator binary -- the spec text *is* the
check, and the assistant executes it.

## Two surfaces, one record

Operators type only into labelled body fences. Step 1 of processing
transfers each fence's trimmed contents into its matching YAML frontmatter
key; the frontmatter is the machine record, the fences are the authoring
surface, and **on any disagreement the fences win**.

````markdown
## Topic category

```topic-category
Agentic Social Content Ops
```
````

...mirrored into the frontmatter as `topic_category: "Agentic Social Content Ops"`.

Every non-fence key -- `version`, `base_image`, `level`, the eight geometry
constants, `card_width`, `desc_lines`, and every comment line -- is compared
byte-for-byte against the core template before anything is transferred. Any
difference is **reverted to the template's text** and mentioned in one
friendly line. It never halts, never keeps the edited value, never argues.
The form carries its own `version` / `versioned_at` (currently v4), shared
with the visual template: one number covers the geometry constants and the
PNG they were measured from.

Scaffolding forks at the folder: fill the form by hand and say "done" (the
gate reverts frontmatter edits, then asks inline for whatever is missing or
non-compliant and writes it in), or answer in chat and the assistant writes
both surfaces as each value arrives. Folder names are kebab-cased from the
speaker name; no name gives `new-speaker-<NN>`, and a taken name gives
`<name>-<NN>` -- lowest free zero-padded number, first dupe = `01`.

## The gates, with their numbers

| Gate | Rule | On breach |
|---|---|---|
| `speaker_name` | 30 chars, Title Case | warn; card ellipsises |
| `speaker_position` / `speaker_company` | 39 chars combined, incl. the 3-char ` / ` joiner; `^[a-z0-9-]+$` | over budget warns; non-kebab warns and offers the kebab form |
| description fence | budget parsed from the fence label: 115 chars | **reject** with the count |
| `topic_category` | 26 chars, Title Case | warn |
| `duration_minutes` | number only, no unit | required |
| `level` | fixed `For All Levels` | not an input; left as is |
| `company_logo` | square, ideally 80x80+ | missing file = hard fail |
| `speaker_image` | exactly square, PNG/JPG/JPEG, max 800x800 (262x262 ideal) | **halt**, actual size reported |
| geometry (8 keys) | read verbatim from section 6 | hand edits already reverted |
| card ratio | `implied_h = card_h / (card_w / card_width)` within +/-2px of the card's real height | **halt** |

Order matters. The frontmatter comparison runs first, before any transfer.
Then the fences move into their keys and the required set is checked -- the
description budget enforced exactly, the two kebab fields merely flagged
off-spec and rendered as given. Assets are checked second. Geometry is
loaded third, read from the form rather than measured (the Apify header logo
shares the placeholder green, a company logo may share the purple, so colour
masks are unreliable by construction). The ratio check is fourth, and only
then does anything render.

## Halt, do not degrade

Each failure mode has a defined report, and none of them has a workaround:

- **Over-budget description** -- rejected with the character count. The text
  is never trimmed.
- **Missing asset** -- stop and ask: resubmit with the images added, or
  render now with a dashed placeholder outline drawn *inside* the block, so
  a real image pasted over it covers it completely.
- **Off-spec photo** -- halt, reporting the actual dimensions and format. An
  accepted square is scaled to the slot; nothing is ever cropped, padded or
  reframed. Squaring it is the operator's call.
- **Ratio mismatch** -- halt, reporting the block's ratio, the card's actual
  ratio, and the height the block would need for the current text
  (`actual_h x scale`). The card is never stretched or letterboxed, because
  its height is quantised in 16px steps by description line count.
- **Name collision** -- never blocked and never overwritten: `-NN` suffix,
  the same number on the archive folder and the PNG.

Failed folders stay in `to-process/` with nothing partial written, and the
rest of the batch continues.

A filled form, every fence and key populated, is at
[../filling-in-the-form/intake-template-completed-example.md](../filling-in-the-form/intake-template-completed-example.md).

Back to the [index](INDEX.md).
