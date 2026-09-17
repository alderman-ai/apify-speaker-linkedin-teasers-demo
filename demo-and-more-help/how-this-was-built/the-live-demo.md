# What the on-stage live demo added

On 2026-09-17 this pipeline ran in front of roughly 200 marketers at an
AI-marketing meetup hosted by Apify. The stage version added two things: a
page that advances on cue, and a generator that cannot stall.

## The page: pre-built states, promoted on a tick

The public page `alderman.ai/apify-live-demo` is not built during the show.
Four page states are deployed to Vercel ahead of time as checkpoints -- title
plus QR code, a paper-card state, the crowd-sourced card, then a gallery.
Going live is an instant promote of an already-built deployment, in either
direction, so any checkpoint can be re-shown if a beat goes wrong.

A detached background loop promotes the sequence on a fixed 20-second tick.
Every state also polls a small static feed (every 5 seconds, per the
author's description of the page), so a phone that
scanned the QR during the first state reloads itself when a later one goes
live, and a newly generated PNG landing in `generated-images/` swaps the
placeholder card for the real one without a rebuild.

## The generation: one dictated name, four parallel attempts

The on-stage skill takes exactly one argument -- a real person's name, dictated
and often mangled by speech-to-text. It resolves the name by web search, pushes
it to the feed, researches once, then fans out to four parallel agents. Each
builds a complete card on its own -- portrait, logo, copy, render -- using a
different portrait-sourcing strategy, so the four fail in different ways and
one snag never sinks the demo. The parent publishes the first good card (a
monogram-only card after 150 seconds if no photo card has landed), never asks a
question, and finishes in about three minutes. A fallback card is rendered in
advance in case everything fails.

## Why it is split that way

A live demo must be deterministic in its stage cues and tolerant in its
generation: fixed timing over pre-built artifacts on one side, probabilistic
work made safe by redundancy and a fallback on the other -- the same split the
rest of the repo uses.

Both maintainer-only skills (`start-the-demo` and `live-demo-speaker`) live in
the author's harness skill folder, outside this repo, and the `live-demo/` show
machinery is gitignored. Nothing in the pipeline depends on either. Rehearsal
outputs are archived at
[../example-speakers/live-demo-rehearsals/](../example-speakers/live-demo-rehearsals/);
the four parallel Božena Němcová builds there are one fan-out, kept as it ran.

Back to the [index](INDEX.md).
