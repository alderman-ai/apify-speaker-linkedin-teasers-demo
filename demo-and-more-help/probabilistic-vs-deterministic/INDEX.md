# probabilistic-vs-deterministic/

**Why brand assets need deterministic code and probabilistic prose — and how
this project combines both.** This is where line 2 of the assistant's menu
lands.

## The idea in ten sentences

A language model is probabilistic: ask it the same thing twice and you get
two different, both reasonable, answers. A brand needs the opposite — the
logo, the colours, the spacing and the fonts have to land identically every
single time. Neither tool is wrong; they are good at different halves of the
job. So split any task into the parts that must be exact and the parts that
benefit from variation. Code the exact parts, once, so they never vary
again. Generate the variable parts — the words — with a person or a model.
Then pass each generated piece through a gate: a mechanical rule a computer
can answer yes or no to, such as a character count, a required format, or a
fixed list of allowed values. This repo is a small, honest example: the
speaker's name, role, topic and blurb are prose, held to budgets of 30, 39,
26 and 115 characters; the card's HTML and CSS, the template geometry, the
fonts and the render step are code, fixed to the pixel. Coded front-end
elements — ours or any brand's existing web components — are the best
container for this, because the same code draws every card while gated
prose fills the slots. Different words on every card; the same card, to
the pixel, every time.

## What you supply, and who writes the words

A visitor supplies a few words — name, job title, company, topic, talk
length, a short blurb — and two pictures: a logo and a photo of themselves
(roughly square is fine). The blurb is the visitor's by default; if they
prefer, the assistant drafts it from what they say in chat. Either way it
passes the same gates before it touches the card.

## The pages

Each is one screen long, in plain words. Any term that needs defining is
defined where it first appears.

1. [The concept](the-concept.md) — probabilistic versus deterministic, when
   each is the best tool, and the split-and-gate recipe.
2. [This project as a worked example](this-project-as-an-example.md) —
   which pieces of this pipeline vary, which never do, and the six stages
   where the gates sit.
3. [Coded front-end elements: the unifying layer](coded-elements-as-the-unifying-layer.md)
   — why a verified HTML/CSS reproduction of the actor card beats asking an
   image model to draw one.
4. [The gates](the-gates.md) — the actual checks with the actual numbers:
   character budgets, fixed vocabularies, image rules, geometry, and
   halt-don't-degrade.
5. [Gallery](gallery.md) — the finished example cards, with links; the same
   code with different words, seen side by side.
6. [Try it yourself](try-it-yourself.md) — back to line 1 of the menu to
   make a card and watch the split happen.
7. [Three questions this folder provokes](faq.md) — can I use my own
   brand's template, what does a card cost, and is this official Apify.

Every fact and number on these pages comes from a file in this repo — the
intake form, the skills, the render page, the help graphics and the
stress-test report. Nothing here is read by the pipeline at run time.
