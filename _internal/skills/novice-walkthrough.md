---
name: novice-walkthrough
description: >
  Hand-holds one complete first run for a person who has never used GitHub,
  a terminal, or an AI coding tool: orient them in the folder, scaffold the
  speaker, interview them for the form, get the two images in and checked,
  render, and show them the finished PNG. A wrapper around `new-speaker`
  and `apify-speaker-card` — it never re-implements either. Use when the
  operator says "Walk me through making my first speaker card", "walk me
  through", "I'm new to this", "I've never done this before", "hold my
  hand", "guide me step by step", "first time", "start the walkthrough",
  or otherwise sounds like they have never seen this folder before.
---

# novice-walkthrough — one guided run, start to finish

The other two skills assume an operator who already knows the repo. This
one assumes nothing: no GitHub, no terminal, no markdown, no idea what a
folder called `_internal` is for. You do the reading, the typing and the
commands; the human supplies answers and two image files.

**This skill is a wrapper.** Scaffolding is `new-speaker.md`. Generation
is `apify-speaker-card.md`. At the two moments below you read the matching
skill **in full** and follow it exactly as written. You never re-implement
their steps here, never simplify them, and never relax a single ground
rule to keep a beginner moving — a halt is still a halt; you just
translate it into plain words and help them fix it.

## Folder names — read this before quoting a path

The operator-facing folders are numbered:

| the skills say | the folder is actually called |
|---|---|
| `to-process/` | `01 To Process/` |
| `processed/` | `02 Processed/` |
| `generated-images/` | `03 Generated Images/` |
| `demo-and-more-help/` | `04 Demo and More Help/` |

`_internal/` and the speaker slug folders (`alex-alderman`) are unchanged.
When `new-speaker.md` or `apify-speaker-card.md` names an old path, map it
onto the numbered folder. **Every path has spaces in it — quote it in every
shell command** (`"01 To Process/alex-alderman"`).

## Tone

- **Short messages. One question per turn.** Ask, stop, wait for the reply.
  Never stack three questions and never move to the next step on an
  assumption about what they would have said.
- **Define jargon in the same sentence you first use it**, or don't use it.
  Never assume they know what a terminal, a repo, a command, markdown,
  YAML, frontmatter, kebab-case or a headless browser is. Say "the form",
  "the folder", "a picture that is exactly as wide as it is tall".
- **Narrate what you just did and where it landed.** "I made a folder
  called `alex-alderman` inside `01 To Process`." They cannot see your
  tool calls.
- **Offer to do the typing.** The default assumption is that they would
  rather answer a question than edit a file.
- Warm, plain, unhurried. No emoji. No "simply" or "just".
- If they answer something you didn't ask, take the answer, thank them,
  and ask your question again.

## Procedure

### 0 · Preflight — silent, before you greet them

Do all of this without narrating it. If everything passes, the human's
first message from you is step 1.

1. **Read `_internal/skills/new-speaker.md` in full.**
2. **Read `_internal/skills/apify-speaker-card.md` in full.**
3. **Run the template-integrity check those skills prescribe**, in their
   order of preference:
   - If the folder is a git checkout:
     `git status --porcelain -- _internal/core-templates-please-dont-touch/`
     — any modification or deletion → `git restore` that path immediately,
     and tag `MISMATCH` in the subject of any commit made this session.
     Also run `git log --oneline -20 --grep=MISMATCH` and, on a hit, tell
     the operator a drift event happened previously.
   - **If there is no git history — the normal case here, because a
     first-timer downloaded a ZIP** — `git status` will fail with
     something like "not a git repository". That is expected, not an
     error to report. Fall back to the hash comparison the skills define:
     compare `_internal/core-templates-please-dont-touch/intake-template.md`
     against `_internal/fonts/jic/BU_intake-template.md`, and
     `..._internal/core-templates-please-dont-touch/speaker-teaser-linkedin_v3.png`
     against `_internal/fonts/jic/BU_speaker-teaser-linkedin_v3.png`. On a
     mismatch, copy the `BU_` reference over the drifted live file and say
     so plainly in step 1 ("one of the locked source files had been
     changed; I put the original back").
   - Never skip, weaken or postpone this check, and never edit or commit
     anything in `core-templates-please-dont-touch/`.
4. **Detect the operating system** (Windows vs macOS) — you need it for
   every command in steps 4 and 6.
5. **Find a Chromium browser**, in the order `apify-speaker-card.md` lists,
   plus the macOS locations of the same two browsers:
   - the `CHROME` environment variable
   - Windows: `C:\Program Files\Google\Chrome\Application\chrome.exe`, then
     `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`
   - macOS: `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`,
     then `/Applications/Microsoft Edge.app/Contents/MacOS/Microsoft Edge`
   - `google-chrome` / `chromium` / `msedge` on PATH

   **HALT if none is found.** Do not download a browser, ever, and do not
   start the walkthrough hoping one turns up later. Say exactly this much:
   *"Before we start, this needs Google Chrome (or Microsoft Edge) on your
   computer — it's used invisibly to draw the picture, and no window will
   ever open. I couldn't find either one. Install Chrome from
   google.com/chrome, then come back and say 'walk me through making my
   first speaker card' again."* Then stop. Nothing else in this procedure
   runs.

### 1 · Orient — where they are, what happens, how long

One message, then one question.

Tell them, in this order and in about this many words:

- What this makes: one square picture announcing a speaker, ready to post
  on LinkedIn.
- What it costs them: about ten minutes, and a couple of questions.
- **They will be asked to click "Allow" once or twice** when you run a
  command. That is normal, it is how their assistant asks permission, and
  they can read what the command is before allowing it.
- **No browser window will ever appear.** The picture is drawn by a
  browser running invisibly in the background.
- If the preflight restored a drifted template, say so in one sentence.

Then the folder tour — name the five folders and give each one sentence:

- `01 To Process` — where a speaker waits while you fill in their details.
- `02 Processed` — where their folder moves once the picture is made.
- `03 Generated Images` — where the finished pictures land. This is the
  one they will actually use.
- `04 Demo and More Help` — examples and explanations. Optional reading.
- `_internal` — the machinery. Nothing in there is for them.

Close with the one question and **wait**:

> *"OK — first, look at the folder list on the left of your screen. You
> should see four numbered folders, `01 To Process` through
> `04 Demo and More Help`, plus one called `_internal`. Do you see them?"*

If they say no, ask what they *do* see, and sort out whether they opened
the right folder (the unzipped `apify-speaker-linkedin-teasers-demo-main`
folder, or whatever they renamed it to) before going on.

### 2 · The speaker's name, then scaffold

Ask one question:

> *"Great. Who's the speaker? Just their name as they'd write it on
> LinkedIn — and it can be you."*

Wait for the answer. Then **read `_internal/skills/new-speaker.md` in full
and follow it end to end** to create the folder (kebab-case name, lowest
free `-<NN>` suffix if the name is taken, the blank intake copied from the
locked template — never from the completed example — plus the folder
README). Do not hand-roll any of that here.

Then tell them what appeared, concretely:

> *"Done. There's now a folder called `alex-alderman` inside
> `01 To Process`. It has a form in it (`intake.md`) and a short note
> saying what belongs in the folder. Next we fill in the form."*

If `new-speaker.md` suffixed the folder because the name was already
taken, say why in one sentence — it is not an error.

### 3 · The form — ask before you assume

One question first:

> *"Have you ever edited a markdown (`.md`) file before? Markdown is just
> a plain text file. If not, no problem at all: tell me the answers and
> I'll type them into the form for you."*

Wait. Then take one of two branches.

**(a) They have edited markdown before.**

Open `01 To Process/<slug>/intake.md` for them — Windows:
`Invoke-Item "01 To Process/<slug>/intake.md"`; macOS:
`open "01 To Process/<slug>/intake.md"` — and explain in three sentences:

- Everything they type goes **between the marker lines** in the section
  headed "Input presentation details here". Those are the blocks that
  start with three backticks and a label like ` ```speaker-name `.
- Type only between those lines; leave the rest of the file alone
  (the part above is the machine's record, and you fill it in for them at
  processing time).
- The number in a label is a limit: `presentation-description-140-char-max`
  means the blurb can be at most 140 characters. Also mention: name 30,
  position and company together 39 (the ` / ` between them counts),
  topic 33.
- Tell them to delete any `[type here]` placeholders.

Then say *"Tell me when you've saved it and I'll check it over"* and
**wait**. When they say done, read the file and check every budget
yourself before moving on; treat any problem exactly as branch (b) does.

**(b) They have not — interview them.**

Ask for **one field per message**, in this order, waiting each time:

1. **Name** — *"What name should appear on the card? (up to 30
   characters)"*
2. **Position** — *"What's their job title? Something like 'gtm engineer'
   or 'head of growth'."* You convert it to lowercase-with-hyphens
   (`gtm-engineer`) yourself; that is the house style, not their problem.
3. **Company** — same treatment (`alderman-ai`). Then check position and
   company together: they must fit **39 characters including the ` / `
   that joins them**.
4. **Blurb** — *"Now the one-or-two-line description of the talk, the bit
   that appears under their name. Up to 140 characters — roughly one
   long sentence."*
5. **Topic** — *"What's the talk's topic or track, in a few words? Up to
   33 characters. Something like 'AI Content Ops'."*
6. **Level** — *"Who's it pitched at? Pick one: `all` (anyone),
   `easy` (beginners), `int.` (people who already know the basics), or
   `adv.` (deep technical)."* Take a plain-English answer and map it, then
   confirm the mapping in half a sentence.
7. **Minutes** — *"How long is the talk, in minutes? Just the number."*

**Count the characters of every answer as it arrives** and say the count
back for the two tight ones (position/company and the blurb). If an answer
is over budget:

- Say the actual number and the limit, plainly: *"That's 168 characters
  and the limit is 140 — 28 too many."*
- **Offer a shortened version and ask them to approve it.** *"Here's a
  140-character version that keeps the meaning: '…'. Want to use that,
  or would you rather rewrite it yourself?"*
- **Wait for a yes.** Never trim, paraphrase or silently shorten their
  words on your own authority. This is the same rule the generator
  enforces by rejecting the form; you are just catching it earlier and
  more kindly.

When every field is in, write the answers into the fenced blocks of
`01 To Process/<slug>/intake.md` — into the body fences, not the
frontmatter; the generator transfers them itself as step 1 of processing —
then **read the finished answers back** as a short list and ask:

> *"That's the form filled in. Anything you'd like to change before we do
> the pictures?"*

### 4 · The two images

One message explaining both, then one action, then wait.

Explain:

- **The company logo** — the small square badge in the top-left of the
  card. Square, ideally 80×80 pixels or bigger. PNG or JPG.
- **The speaker photo** — the portrait. It must be **square**: exactly as
  wide as it is tall, like an Instagram post rather than a normal phone
  photo. PNG, JPG or JPEG, **no bigger than 800×800 pixels**. 262×262 is
  the perfect size, but any square up to 800×800 works.
- **Why square matters:** this tool never crops or re-frames a photo. It
  will not decide which part of their face to keep. So the photo arrives
  framed the way they want to be seen, or it doesn't arrive.

Then **open the speaker folder in their file manager for them** so they
can drag the two files in:

- Windows: `explorer "01 To Process\<slug>"`
- macOS: `open "01 To Process/<slug>"`

Say: *"I've opened the folder. Drag the two pictures into it, then tell me
when they're in — you can name them anything."* Then **wait**.

When they say done, **check the files yourself with built-in tools only —
install nothing:**

- Windows (PowerShell):
  ```
  Add-Type -AssemblyName System.Drawing
  $i = [System.Drawing.Image]::FromFile("<absolute path>")
  "$($i.Width)x$($i.Height)"; $i.Dispose()
  ```
- macOS: `sips -g pixelWidth -g pixelHeight "<path>"`

Rename what they dropped to `company-logo.<ext>` and `speaker.<ext>` as
the folder README expects, and tell them you did.

**If the photo is square and ≤800×800**, say so and go to step 5.

**If it is not**, do not proceed and do not fix it for them. Report the
exact numbers and offer three ways forward, then wait for a choice:

> *"Your photo is 1080×1350 — taller than it is wide, so it isn't square,
> and this tool never crops a photo for you (it won't guess which part of
> your face to keep). Three ways to go:*
>
> *1. **Crop it to a square yourself** — quickest on a phone: open it in
> Photos, tap Edit, choose the 1:1 or Square crop, save, and drop the new
> file in. On a computer: Paint on Windows or Preview on Mac, crop to
> equal width and height. Then tell me and I'll re-check it.*
> *2. **Finish today without the photo** — I can make the card now with a
> dashed outline where the photo goes. A correctly sized square photo
> pasted over that outline later covers it exactly.*
> *3. **Do a practice run now** with a stand-in picture, so you see the
> finished thing end to end, then run it again with your real photo when
> it's square."*

Details for options 2 and 3:

- **Option 2 (placeholder):** this is the generator's own missing-asset
  behaviour. Answer its "resubmit or placeholder?" question with
  "placeholder" on their behalf in step 5, and say clearly that the
  finished picture will have a dashed box in it.
- **Option 3 (practice run):** use `_internal/sample-assets/top-left-actor.png`
  as the logo. **Check the sample portrait's dimensions before offering
  it** — `_internal/sample-assets/speaker.png` is currently 720×960 and
  would be rejected by the same rule. If it isn't an accepted square, take
  a square stand-in portrait from an archived example instead, e.g.
  `04 Demo and More Help/Example Speakers/Fictional Characters/Processed/baba-yaga/speaker.png`
  (262×262), copy it into the speaker folder as `speaker.png`, and say in
  plain words that it is a stand-in face, not theirs.

If the **logo** is missing or is not square, it is much less strict —
non-square logos are centre-cropped by the card itself. Mention it, don't
block on it.

### 5 · Make the picture

Tell them what is about to happen, in one short message:

> *"Now I'll make the picture. This takes a minute or two. Your computer
> may ask you to allow a command — that's the invisible browser that draws
> the card. No window will open."*

Then **read `_internal/skills/apify-speaker-card.md` in full and follow it
end to end** for this one folder: validate the form, check the assets,
measure the template's coloured blocks, run the card-ratio check, fill the
render shell, screenshot it with the browser you found in preflight,
verify the result with your own eyes, then deliver the PNG and move the
folder. Do not shortcut, reorder or approximate any of it, and do not
substitute your own render.

**If it halts, that is the pipeline working.** Do not work around it, do
not degrade the output, do not retry the same thing hoping for a different
result. Translate the halt into one plain sentence, name the one thing to
fix, and loop back:

| what halted | what you say | go back to |
|---|---|---|
| description over budget | "The blurb is N characters and the limit is 140. Want me to suggest a shorter version?" | step 3 |
| photo not square / too big / wrong format | "The photo is WxH — it needs to be square and no more than 800×800." Re-offer the three options. | step 4 |
| an image missing | "I can't find the logo / photo in the folder. Want to add it, or shall I use a dashed outline?" | step 4 |
| block/card ratio mismatch, geometry disagreement | "Something's off with the background template's layout — that's a maintainer problem, not something you did wrong." Report the numbers and stop. | halt |
| browser fails to run | Name the browser path you tried, tell them what to install. | halt |

Never present a halt as their fault, and never let "let's just get it
done" turn into trimming text, cropping a photo, overwriting a file, or
skipping the verification step.

### 6 · Show them the result

When `apify-speaker-card.md` reports OK:

1. **Open the finished picture for them.**
   - Windows: `Invoke-Item "03 Generated Images\<slug>-final.png"`
   - macOS: `open "03 Generated Images/<slug>-final.png"`
2. **Say exactly where it lives:** `03 Generated Images/<slug>-final.png` —
   a 1200×1200 PNG, the right shape for a LinkedIn post.
3. **Say what moved:** their folder (form plus the two pictures) is now in
   `02 Processed/<slug>/`. `01 To Process` is empty again. Nothing was
   deleted.
4. **How to post it:** on LinkedIn, start a post, click Photo, pick that
   file, write the caption, post. Square images show at full size in the
   feed.
5. **Offer the next one:** *"Want to do another speaker? It's faster the
   second time — say the word and give me a name."* If yes, go to step 2;
   the folder tour is not repeated.

If they ask what to change on the card, the honest answer is: the text and
the two pictures are theirs; everything else — the frame, the starfield,
the `Join me in PRAGUE` line, the layout — is fixed by design.

## Never

- Never skip the preflight, and never start without a browser found.
- Never edit, commit, or "fix" anything in
  `_internal/core-templates-please-dont-touch/`. Never weaken or postpone
  the drift check, in either its git or its hash form.
- Never re-implement `new-speaker.md` or `apify-speaker-card.md` from
  memory or from this file's summaries — read them, in full, at steps 2
  and 5.
- Never crop, pad, rotate or re-frame a photo, and never offer to. The
  human squares it, or takes the placeholder, or takes the stand-in.
- Never trim, paraphrase or shorten operator text without an explicit yes
  to a specific suggested replacement.
- Never overwrite a file or a folder. Duplicate names get `-<NN>`, lowest
  free number, first dupe = 01.
- Never download a browser, a font, a package or anything else, and never
  let a render touch the network.
- Never ask two questions in one message, and never assume the answer to
  the one you asked.
- Never say "just", "simply", or "obviously". Never blame them for a halt.
- Never leave them at a dead end: every halt comes with one concrete next
  action.

## Notes

- This skill adds no code to the repo. It is markdown instructions, like
  the other two.
- It is a first-run wrapper. Someone who has been through it once can use
  the two operator phrases directly — "new speaker <Name>" and "process
  the queue" — and never needs this file again.
- Assume a ZIP download rather than a git clone: no git history, no
  terminal habits, and paths containing spaces. Quote every path.
