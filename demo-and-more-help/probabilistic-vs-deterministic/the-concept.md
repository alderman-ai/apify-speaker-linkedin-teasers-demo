# The concept: probabilistic prose, deterministic assets

Two words, one line each:

- **Probabilistic** — the answer is drawn from a range of likely answers. Ask a
  language model the same question twice and you get two different, both
  reasonable, replies.
- **Deterministic** — the same input always produces exactly the same output.
  Code is deterministic. A brand needs to be: the logo, the colours, the
  spacing and the fonts have to land identically every single time.

## Each tool where it is strongest

A model writing prose is the best tool you have when you want **variation, tone
and speed** — forty speakers, forty different blurbs, each one sounding human,
in the time it takes to make coffee.

A coded, fixed asset is the best tool when you want **brand consistency, pixel
accuracy, and something you own**. Nobody wants a logo that is 4 pixels to the
left this week.

## The recipe

Split any task into two piles.

1. The parts that must be exact → **code them**. They never vary again.
2. The parts that benefit from variation → **generate them**, then **gate**
   them — run each one through a mechanical check before it is allowed
   through. A gate is a rule a computer can answer yes or no to: a character
   count, a required format, a fixed list of allowed values.

That is the whole idea. Generated words, mechanical gates, coded container.

## This repo, concretely

Every card here is one picture built from both piles.

The **variable pile** is the words on the card, typed by a person or written by
a model: the speaker's name (30 characters), the position and company (39
together, lowercase and hyphenated), the topic (26), and the talk blurb (115).
Each is counted against its budget as the answers come in. The blurb is the
hard gate: over budget, the form is rejected with the count, never quietly
trimmed. The one-line fields end in an ellipsis on the card rather than wrap
or shrink. The audience level is not even a question — every card reads
`For All Levels`.

The **exact pile** is everything else: the 1200×1200 backdrop, the card's
HTML and CSS rebuilt from Apify's own, the fonts bundled in the folder, and
the eight numbers that place the two elements — measured once, then fixed
constants. No run re-measures anything.

Different words on every card. The same card, to the pixel, every time.

Back to the [index](INDEX.md).
