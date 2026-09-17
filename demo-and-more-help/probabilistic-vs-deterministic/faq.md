# Three questions this folder provokes

**Can I point this at my own brand's component or template?**

That is the approach, not just the demo. The pipeline is a versioned pair —
one visual template image (v4 today) and one intake form (v5 today; the
two were numbered together until a form-only change to the photo rule
bumped the form alone) — plus a coded card. Changing the look is a deliberate,
versioned event: the template image gets a new `_v<N>` suffix, its blocks
are measured once and the eight placement numbers are written into the
form's section 6, the form's version bumps, and one card is rendered to
confirm. For *this* demo, v4 is final and the core templates are read-only;
the recipe for the next version is in the
[core-templates README](../../_internal/core-templates-please-dont-touch/README.md).
Swap in your own coded component and your own backdrop and the same shape
holds: code the exact parts, gate the generated parts.

**What does a card cost?**

Nothing beyond your assistant's tokens and a browser you already have. The
repo is self-contained: fonts, card code, template and instructions ship in
the folder; there are no build scripts, no packages, no services and no
network calls at any point. The only external requirement is a
Chromium-based browser (Chrome, or the Edge preinstalled on Windows), driven
invisibly to take one screenshot. The plain-English inventory of every piece
of code is in
[scripts-and-security.md](../about-this-project/scripts-and-security.md).

**Is this official Apify?**

No. This is an unofficial, non-commercial demo built for a community
meetup. The card is a from-scratch reproduction of apify.com's public
actor-card design, made as an homage; Apify's name, logo and visual design
belong to Apify, and nothing here is affiliated with or endorsed by them.

**Which parts are AI and which are code?**

The assistant is the engine — there are no scripts — but what it produces
is fixed by code: the card's HTML and CSS, the fonts, the backdrop image and
the render step never change from card to card. The words are yours, or
the assistant drafts them from what you say in chat; either way they pass
through the same gates (character budgets, fixed vocabularies, the photo
rule) before they touch the layout. So: AI writes and checks, code draws.

**Can I post my own card on LinkedIn?**

Yes — that is what it is for. Two honest notes. The design is an
unofficial homage to Apify's public actor card, so present it as a
community-event teaser, not as Apify material. And the example cards in
this folder carry their own credits: some portraits are Creative Commons
with an attribution obligation, and the real-people set is internal test
material — your own photo and logo carry no such strings.

Back to the [index](INDEX.md).
