# Speaker folder — what goes in here

Three things. The skill refuses (or asks about placeholders) until it has them.

| file | what |
|---|---|
| `intake.md` | the filled-in form — type only in its labelled fences; the assistant fills the frontmatter from them |
| `company-logo.(png/jpg)` | the speaker's company logo — square, ideally 80×80+ |
| `speaker.(png/jpg/jpeg)` | the speaker photo — **square (1:1), at most 800×800** |

## The speaker photo — what's accepted

**Square, PNG/JPG/JPEG, no bigger than 800×800.** The perfect supply is
**262×262** — the photo slot's exact size on the current template — but any
square within the cap works: the generator scales it into the slot without
cropping a single pixel, and archives the slot-sized copy it used.

Frame it yourself, the way you want to be seen. The pipeline **never crops,
pads or reframes** a photo — a non-square, oversized or wrong-format image
halts the run with the actual dimensions reported and a square resubmit
requested.

## If an image is missing

The run stops and asks: resubmit this folder with the image added, or render
now with a dashed placeholder outline (drawn inside the block, so the real
image pasted in later covers it exactly — the logo slot keeps the card's 8px
corner rounding).

## Text limits

name 30 · position/company 39 (including the ` / ` joiner) ·
description 140 · topic 33.
The description budget is enforced from the fence label; over it, the form is
rejected rather than truncated. Everything else ellipsises.
