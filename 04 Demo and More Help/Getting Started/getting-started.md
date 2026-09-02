# Start here — never used GitHub or an AI assistant before

This page is for you if none of this is familiar. It is six steps, and the
last one is "if it stops". You do not need to know how to code, and you do
not need an account anywhere.

**What you are making:** one square picture announcing a speaker at your
event, ready to post on LinkedIn.

**How long it takes:** about ten minutes, most of it answering questions.

**How it works:** you talk to an AI assistant in a chat box. It asks you
one question at a time and does the rest. You never type a command.

---

## Before you start, have these handy

- The speaker's **name**, as they write it publicly
- Their **job title** and **company**
- A **two-line blurb** about the talk (one long sentence is plenty)
- The **topic** of the talk, in a few words
- How **long** the talk is, in minutes
- A **square photo** of the speaker — as wide as it is tall
- The **company logo** as a picture file

Don't worry if the photo isn't square yet. Step 6 covers that.

---

## Step 1 — Get the folder

1. Go to
   **https://github.com/alderman-ai/apify-speaker-linkedin-teasers-demo**
2. Click the green **Code** button near the top right.
3. In the small menu that opens, click **Download ZIP**.
4. Find the downloaded file (usually in your Downloads folder) and
   **unzip** it — double-click it on a Mac, or right-click and choose
   "Extract All" on Windows.

You now have a folder called **`apify-speaker-linkedin-teasers-demo-main`**.
The `-main` on the end is normal. Rename it to something friendlier if you
like, or leave it — nothing depends on the name. Move it somewhere you can
find again, like your Desktop or Documents.

**You do not need a GitHub account.** You are only downloading a folder.

---

## Step 2 — Get an AI coding assistant

An "AI coding assistant" is a chat app that can also read and write files
in a folder on your computer. That second part is why an ordinary chat
window isn't enough here.

**The recommended one: Claude Code.** There is a desktop app for both Mac
and Windows. Download it here: **https://claude.com/claude-code**

Install it the way you'd install any other app, and sign in when it asks.

Other options, if you already have one of these:

- **Cursor** — a code editor with an AI chat panel built in:
  **https://cursor.com**
- **VS Code with GitHub Copilot** — if your workplace already gave you
  these, they work too.

Any of the three is fine. They all read the same instruction file that
comes inside the folder you just downloaded, so they all behave the same
way here.

---

## Step 3 — Open the folder in the assistant

In the app, choose to open a folder, and pick the folder you unzipped in
step 1 (`apify-speaker-linkedin-teasers-demo-main`, or whatever you renamed
it to).

- In the Claude Code desktop app, this is the "open a folder" or "add a
  project" choice on the first screen.
- In Cursor or VS Code, it is **File → Open Folder**.

Once it opens, you should see a list of folders down the left-hand side:

```
01 To Process
02 Processed
03 Generated Images
04 Demo and More Help
_internal
```

Plus a few files. If you see that list, you are in the right place. If you
see something else, you probably opened the folder *containing* the one you
want — go one level down and try again.

Somewhere on the screen there is a chat box. That is the only thing you
will use.

---

## Step 4 — Paste this sentence into the chat

Copy this exactly, paste it into the chat box, and press Enter:

```
Walk me through making my first speaker card
```

That's it. That is the whole job.

From here the assistant leads. It will ask you **one question at a time**
and wait for your answer each time — what the speaker is called, what they
do, what the talk is about, how long it runs. Answer in normal words. It
does all the typing into the form for you, and it tells you what it just
did after each step.

If you don't understand a question, say so. "I don't know what you mean"
is a perfectly good answer, and it will explain.

*(One line of housekeeping: your assistant reads a file called `CLAUDE.md`
in this folder on its own, to learn how everything here works. You never
need to open it.)*

---

## Step 5 — What to expect while it runs

- **It will ask permission to run a command a few times, especially near the start.** A box will
  pop up asking you to **Allow** it. This is normal and it's how the app
  checks with you before doing anything. You can read what the command is
  before you allow it.
- **No browser window will open.** The picture is drawn by Chrome or Edge
  running invisibly in the background. If you see nothing happen, that is
  correct.
- **You will be asked for two pictures**, and the assistant will open the
  right folder for you so you can drag them in.
- **The whole thing takes about ten minutes.**
- **The finished picture lands in `03 Generated Images`**, named after the
  speaker, ending in `-final.png`. That is the file you post. The
  assistant will open it for you when it's done.

---

## Step 6 — If it stops

It sometimes stops on purpose. That is the design: it would rather stop and
tell you than quietly ruin the picture. When it stops, it says exactly what
is wrong in plain words. Fix that one thing, tell it you're done, and it
carries on.

The three things that stop it most often:

| What it says | What to do |
|---|---|
| The blurb is too long (over 100 characters) | Shorten it, or say yes to the shorter version it offers you. It will never cut your words without asking. |
| The photo isn't square | Crop it to a square. On a phone: open the photo, tap Edit, choose the **Square** or **1:1** crop, save. On a computer: Paint (Windows) or Preview (Mac). Then drop the new file in. |
| It can't find a picture in the folder | Drag the missing one in, or let it draw a dashed box where the picture goes and add the real one later. |

It will never crop your photo for you, shorten your text without your
say-so, or overwrite a file you already have. If it stops for a reason not
in that table, just paste what it said back to it and ask "what should I do
about this?"

---

## When you've done it once

You don't need this page again. Next time, open the folder in your
assistant and say:

```
new speaker Jane Smith
```

fill in the form, drop in the two pictures, and say:

```
process the queue
```

Same result, about two minutes.
