# Speaker folder — what goes in here

Three things. The skill refuses (or asks about placeholders) until it has them.

| file | what |
|---|---|
| `intake.md` | the filled-in form — type only in its labelled fences; the assistant fills the frontmatter from them |
| `company-logo.(png/jpg)` | the speaker's company logo — square, ideally 80×80+ |
| `speaker.(png/jpg)` | the speaker photo — **exact 262×262 square** |

## Image sizing — required dimensions

The speaker photo is **required as an exact 262×262 square** (PNG or
JPG/JPEG), framed the way you want it shown — the photo slot on the speaker
card is square, so nothing is cropped, and a correctly sized image dropped
over a placeholder later fully covers the placeholder's outline. The
pipeline never reframes a photo: any other size halts the folder with the
actual dimensions reported and a 262×262 resubmit requested.

## If an image is missing

The run stops and asks: resubmit this folder with the image added, or render
now with a dashed placeholder outline (drawn inside the block, so the real
image pasted in later covers it exactly — the logo slot keeps the card's 8px
corner rounding).

## Text limits

name 30 · position/company 39 (with the `/`) · description 140 · topic 33.
The description budget is enforced from the fence label; over it, the form is
rejected rather than truncated. Everything else ellipsises.
