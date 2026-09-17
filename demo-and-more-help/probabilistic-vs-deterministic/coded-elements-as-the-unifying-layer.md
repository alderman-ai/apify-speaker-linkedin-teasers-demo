# Coded front-end elements: the unifying layer

The card in this demo is not drawn. It is **built** — the same way a web page
is built, out of HTML (the text and structure of a page) and CSS (the rules
that say how that structure looks: sizes, colours, spacing).

Specifically, it is Apify's own actor card — the component their site calls
`ActorStoreItem` — rebuilt from scratch in plain HTML and CSS, and **verified
box-for-box against the live site to 0.001px**. Every box on our card sits
where the real one sits, to a thousandth of a pixel. That is the difference
between a likeness and a reproduction.

Here is what that buys a marketing team.

**One card or fifty, the layout is identical.** The same code draws all of
them. There is no "close enough on this one". Only the words and the two
images change.

**Text behaves like text.** The real fonts — Inter and IBM Plex Mono, the two
faces apify.com actually serves — ship inside this folder, so the letterforms
are the letterforms. A long role line ends in a real ellipsis (the "…" a
browser adds when text runs out of room), and the blurb clamps at two lines.
The card's height is even stepped by how many description lines there are:
113.667px with no description at all, 121.667px when the description is
present but empty (it still contributes its 8px top margin), 137.667px at one
line, 153.667px at two, 169.667px at three. Real components have real measurements.

**Rendering is boring, on purpose.** "Render" here means a browser already on
your machine opens the page invisibly ("headless" — no window, nobody
watching) and photographs it. The render page holds no JavaScript at all and
reaches no network; see
[scripts-and-security.md](../about-this-project/scripts-and-security.md) and
the page itself, [shell.html](../../_internal/render/shell.html).

Ask an image-generation model for "a card that looks like Apify's" and you get
a new invention every time: shifted spacing, misspelled words, a logo that
drifts a little further from the real one on each run — and no way to change
one field without redrawing the whole picture.

The same logic governs the speaker element beside the card: its frame, corners
and the words **Join me in PRAGUE** are fixed. Only the photo is yours.

None of this is special to Apify. Any brand that already has web components
has this asset sitting in its codebase today — a pixel-exact, field-editable
version of itself, ready to be filled with words a language model wrote and a
character count checked.

(This is an unofficial, non-commercial homage; Apify's name, logo and visual
design belong to Apify.)

Back to the [index](INDEX.md).
