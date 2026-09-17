# Three questions this folder provokes

**Can I point this at my own brand's component or template?**

That is the approach, not just the demo. The pipeline is a versioned pair —
one visual template image and one intake form that share a version number
(both v4 today) — plus a coded card. Changing the look is a deliberate,
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

Back to the [index](INDEX.md).
