# demo-and-more-help/

Everything that isn't the pipeline itself: what to read when you're stuck,
what the output actually looks like, and what's really inside the repo you
just cloned.

None of it is needed to make a teaser. The pipeline lives in the folders
next door — you tell the assistant a speaker's details in chat, and a
finished PNG lands in `generated-images/`; the root `README.md` says how
to start (say "show me the demo"). This folder is for the moments around
the edges.

## "I'm filling in the form and I don't know what fits where"

→ **`filling-in-the-form/`**

An annotated picture of the card with every field labelled and a line
drawn to where it lands, the character budget for each one, and a complete
filled-in intake form showing exactly what "done" looks like before it is
rendered.

## "What does this thing actually produce?"

→ **`example-speakers/`**

Eighteen speakers that have already been through the pipeline, each with
their finished 1200×1200 PNG *and* the exact folder that produced it — so
you can read a real filled-in form next to the image it became. Folklore
characters, real Czech public figures used as stress-test material, and
the rehearsal runs made the day of the live demo.

## "Why code the card instead of just asking an AI to draw one?"

→ **`probabilistic-vs-deterministic/`**

The idea behind the whole project, in six short pages: language models
give a slightly different answer every time, brands need the same result
every time, and the trick is to code the parts that must be exact and
generate the parts that benefit from variation — then check the generated
words with mechanical rules before they touch the layout. Start at its
`INDEX.md`, or pick line 2 of the assistant's menu and it will walk you
through.

## "I'm technical. How does it actually work?"

→ **`how-this-was-built/`**

The internals, one screen per topic: the architecture (a markdown form,
two images, one static HTML page, one headless-browser screenshot), the
verified CSS reproduction of the actor card and why its oddities stay,
the template's eight fixed constants, the intake contract as executable
checks, the exact render command, the git timeline with the stress test's
findings, and what the on-stage live demo added. Start at its `INDEX.md`,
or pick line 3 of the assistant's menu.

## "I cloned a repo off the internet. Is it going to do something to my machine?"

→ **`about-this-project/scripts-and-security.md`**

Good instinct. That file is a plain-English inventory of every piece of
code in the project and when it runs. Short version: there are **zero
executable scripts** — the assistant is the engine, and the only code here
is a little HTML and CSS that draws the card so a browser can photograph
it. Nothing is fetched from the internet at any point.

The same folder holds `pipeline-evaluation.md`, the ten-speaker stress
test, if you'd like evidence the thing actually works before you trust it
with a real speaker.

## For assistants

**Read `INDEX.md` instead of this page.** It sits next to this file and is
the routing table for the whole folder — every subfolder and file with a
one-line description of what it's for and who to send there. This page is
written for the human; the INDEX is written for you.

## Still lost?

Ask the assistant in plain words — "what is this repo?", "how do I make a
card?", "is this safe?" Orienting you is exactly what it's for, and every
folder in the repo carries an `INDEX.md` it can read.
