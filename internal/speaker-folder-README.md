# Speaker folder — what goes in here

Three things. The skill refuses (or asks about placeholders) until it has them.

| file | what |
|---|---|
| `intake.md` | the filled-in form — type only in its labelled fences; the assistant fills the frontmatter from them |
| `company-logo.(png/jpg)` | the speaker's company logo — square, ideally 80×80+ |
| `speaker.(png/jpg)` | the speaker photo |

## Image sizing — two accepted forms

**Either** supply the image at **exactly the template's photo-area size**
(PNG or JPG/JPEG; 339×391 on the current template — the green area inside
the blue ring) — it is used as-is and, dropped over a placeholder later,
fully covers the placeholder's outline, which sits inside those bounds.

**Or** supply any size of those file types **with a visible mark at the
intended centre** — an "x", a scribble, any obvious annotation on top of the
image. The skill reads the mark, records its coordinates as `speaker_focus:
[x, y]` in the form, and the generator cuts a correctly proportioned crop
centred there. The original file is never modified.

No mark on an odd-sized image means a plain centre crop.

## If an image is missing

The run stops and asks: resubmit this folder with the image added, or render
now with a dashed placeholder outline (drawn inside the block, so the real
image pasted in later covers it exactly — the logo slot keeps the card's 8px
corner rounding).

## Text limits (measured, includes an 8% buffer)

name 30 · position/company 39 (with the `/`) · description 82 · topic 33.
The description budget is enforced from the fence label; over it, the form is
rejected rather than truncated. Everything else ellipsises.
