# The gates: mechanical checks between prose and pixels

A gate is a rule a computer can answer yes or no to. Words — yours or a
model's — only reach the picture after passing every gate below. Here are this
project's actual gates, with its actual numbers.

## 1. Character budgets

A budget is the most characters a field can hold before it stops fitting on the
card. These are measured on a real render, not guessed.

| field | where it lands | budget |
|---|---|---|
| speaker name | card title, top line | **30** characters |
| position / company | the line under the name, joined as `position / company` | **39** together, counting the 3 characters of the ` / ` joiner |
| description (the talk blurb) | card body, two lines | **115** characters |
| topic category | card footer, left | **26** characters |
| duration | card footer, after the star | a number — no character budget |

The annotated versions: [field mapping](../filling-in-the-form/Actor%20card%20field%20mapping.png)
and [text budgets](../filling-in-the-form/Actor%20card%20text%20budgets.png).

## 2. Fixed vocabularies

Some fields aren't questions at all.

- **Audience level** is not an input. Every card reads `For All Levels`.
- **Duration** is a number only — type `20`, and the card prints `20 (mins)`.
- **Folder names and the position/company line** are kebab-case: all lowercase,
  words joined by hyphens, like `head-of-growth`. Anything else is flagged, and
  a fix is offered rather than applied behind your back.

## 3. Images

- **Company logo** — PNG, ICO, JPG or JPEG; square, ideally 80×80 pixels or
  larger so it survives the render (it is drawn at 40×40 with rounded
  corners, and a non-square logo is centre-cropped, not squashed). It sits
  on a light `#f3f3f3` plate, so a white-on-transparent logo will vanish —
  supply one with its own background.
- **Speaker photo** — PNG, JPG or JPEG, any size, **roughly square**: the
  longer side at most 1.25 × the shorter, so a selfie cropped by eye on a
  phone is fine. The pipeline trims the longer side evenly on both sides to
  make an exact square (a plain centre crop — no face detection, no
  guessing) and resizes it to the slot; it never pads or stretches. Further
  from square than that is a stop with the measured width and height and one
  fix: crop it roughly square on your phone and resend.

## 4. Geometry

The eight numbers that place the two elements on the backdrop were measured
once, when the template was built, and stored as constants in section 6 of the
[intake form](../../_internal/core-templates-please-dont-touch/intake-template.md).
No run re-measures the picture, and a hand edit to those numbers is reverted
before anything renders. One check runs every time: the purple block's
proportions must match the card as it actually renders, within **±2 pixels**
at the card's own CSS scale — the check divides the block's height by the
1.9975 enlargement first, so on the finished 1200×1200 image that is about
4 pixels of slack. Card height moves in fixed steps, set by how many lines the blurb
wraps to — so a blurb that shrinks to one line changes the shape, and this
check catches it.

## 5. Halt, don't degrade

Fail a gate and the run stops with a plain reason. An over-budget blurb is rejected
with its character count, never trimmed; the one-line fields are counted as
you answer and, on the card, end in an ellipsis rather than wrap or shrink. A missing image stops with a choice:
resubmit, or render now with a dashed placeholder outline that the real image
will cover exactly. A shape mismatch reports the height the block should be.
Nothing is ever overwritten — a repeated name gets `-01`, `-02` and so on.

A silent fix is the worst outcome for a brand asset: it ships looking almost
right, and nobody notices until it is public. A stop takes a minute to fix and
costs nothing.

Back to the [index](INDEX.md).
