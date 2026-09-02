# 04 Demo and More Help/

Everything that isn't the pipeline itself: what to read when you're stuck,
what the output actually looks like, and what's really inside the repo you
just cloned.

None of it is needed to make a teaser. The pipeline lives in the folders
next door — you fill in a form in `01 To Process/`, say "process the queue",
and a finished PNG lands in `03 Generated Images/`; the root `README.md`
walks through that in four steps. This folder is for the moments around
the edges.

## "I've never used GitHub or an AI coding tool"

→ **`Getting Started/getting-started.md`**

Six steps, plain words, no account needed: download the folder from
GitHub, install an AI assistant (Claude Code is the easy one), open the
folder in it, and paste one sentence into the chat —
**"Walk me through making my first speaker card"**. From there the
assistant asks you one question at a time and does the typing, the
checking and the drawing. About ten minutes end to end, and it tells you
plainly if anything needs fixing.

Start there rather than with the root `README.md` if words like *repo*,
*markdown* or *terminal* are unfamiliar — the README assumes you've used
one of these tools before.

## "I'm filling in the form and I don't know what fits where"

→ **`Filling in the Form/`**

An annotated picture of the card with every field labelled and a line
drawn to where it lands, the character budget for each one, and a complete
filled-in intake form showing exactly what "done" looks like before you
say "process the queue".

## "What does this thing actually produce?"

→ **`Example Speakers/`**

Twelve speakers that have already been through the pipeline, each with
their finished 1200×1200 PNG *and* the exact folder that produced it — so
you can read a real filled-in form next to the image it became. Five
folklore characters in one half; in the other, seven mid-project
snapshots — real Czech public figures used as stress-test material, plus
the two earliest synthetic personas, made before the pipeline was
finished.

## "I cloned a repo off the internet. Is it going to do something to my machine?"

→ **`About This Project/scripts-and-security.md`**

Good instinct. That file is a plain-English inventory of every piece of
code in the project and when it runs. Short version: there are **zero
executable scripts** — the assistant is the engine, and the only code here
is a little HTML and CSS that draws the card so a browser can photograph
it. Nothing is fetched from the internet at any point.

The same folder holds `pipeline-evaluation.md`, the ten-speaker stress
test, if you'd like evidence the thing actually works before you trust it
with a real speaker.

## Still lost?

Ask the assistant in plain words — "what is this repo?", "how do I make a
card?", "is this safe?" Orienting you is exactly what it's for, and every
folder in the repo carries an `INDEX.md` it can read.
