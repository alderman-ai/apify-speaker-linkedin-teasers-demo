# This project as a worked example

This repo turns a few details about a speaker into a finished 1200×1200
LinkedIn image. It is a small, honest example of the split: the words are
written fresh every time, the picture around them never moves.

## What varies, and what never does

| Varies per speaker (written, then checked) | Never varies (coded once) |
|---|---|
| Speaker name — fits 30 characters | The backdrop image, 1200×1200, the same file for every card |
| Position and company — 39 characters together, lowercase and joined by hyphens | The card's own layout, rebuilt in HTML and CSS (the code behind any web page) from Apify's |
| Topic category — 26 characters | The fonts, Inter and IBM Plex Mono, sitting in the folder so no render reaches the internet |
| Talk length — a number of minutes | The small `?` circle in the card footer, and the word `(mins)` |
| The blurb — 115 characters | The fixed line `Join me in PRAGUE` under the portrait |
| Two pictures: a square speaker photo and a company logo | The eight numbers that place the two elements on the backdrop |

Audience level sits oddly in the middle: it looks like a field, but it was
frozen. Every card reads `For All Levels`, and nobody is asked.

Those eight numbers deserve a sentence. The card block is 799×307 pixels at
(201, 748); the speaker block is 294×336 at (706, 345). They were measured
once, when the backdrop was built, and written into section 6 of
[the intake form](../../_internal/core-templates-please-dont-touch/intake-template.md).
No run measures the picture again.

## Where the gates sit

A gate is a check a computer can answer yes or no to. Written words only
reach the fixed layout through these:

1. **The form is checked.** Every field is counted against its budget as the
   details come in. The blurb is the hard gate: over budget, the form is
   rejected with the count — the text is never quietly shortened. The
   one-line fields end in an ellipsis on the card rather than wrap or shrink.
   Anything typed into the machine-readable part of the form is put back the
   way the template has it. (The budgets are pictured in
   [Actor card text budgets.png](../filling-in-the-form/Actor%20card%20text%20budgets.png).)
2. **Both pictures are checked.** The photo must be exactly square, a PNG or
   JPG, at most 800×800; it is scaled into its 262×262 slot, never cropped.
3. **The eight fixed numbers are loaded** from the form, exactly as written.
4. **One ratio check.** The block is 799 wide and the card is drawn 400 wide,
   so it is enlarged by 1.9975 — which means the block has to match a card
   153.667 tall, within 2 pixels. If it does not, the run stops.
5. **The render.** A headless browser — the Chrome or Edge already on the
   machine, running with no window — photographs the page. Same inputs, same
   pixels, every time.
6. **A look with human eyes**, and then the PNG is delivered.

Six stages, and only the first two concern anything a person wrote. Past
them, the picture is arithmetic.

Back to the [index](INDEX.md).
