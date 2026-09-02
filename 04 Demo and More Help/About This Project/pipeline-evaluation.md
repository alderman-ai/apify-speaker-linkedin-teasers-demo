# Pipeline efficiency evaluation — 2026-09-01

> **Note:** written before the same-day file-tree reorganisation, and
> before the 2026-09-02 folder renaming; every path named below is an old
> one. Current names: `final-output/` → `03 Generated Images/`,
> `to-process/` → `01 To Process/`, `processed/` → `02 Processed/`,
> `demo-and-more-help/` → `04 Demo and More Help/`; `visual-templates/`
> and `skills/` now live under `_internal/`. Findings unchanged; only the
> paths moved.

Scope: the **skills, input contract, and generation pipeline** — scaffold →
intake form → validate → geometry → token fill → render → verify → deliver.
The speaker research and the subagent-as-operator role-play used to drive the
test are simulation overhead and are excluded from every number below.

Evidence base: a ten-speaker batch (five real Czech figures, five folklore
characters) run through the regenerated docs by **two independent executors
reading the docs cold**, in parallel, against the same canon template. All
ten teasers were delivered to `final-output/`; one carries a defect (below).

---

## 1 · What the test showed about efficiency

### The cost structure is almost entirely fixed, not per-speaker

Both executors independently converged on the same shape:

| cost bucket | executor A (fantasy) | executor B (czech) |
|---|---|---|
| fixed batch overhead | ~17 tool calls | ~50 tool calls |
| genuine per-speaker work | **~1 call** (the eyes-on verification) | **~2 calls** |
| total for 5 speakers | ~25 | ~60 |

Executor B's higher overhead was not process — it was **doc defects**: three
iterations on geometry because the colour-mask rule is ambiguous (§2.2), a
ten-call investigation of the description-truncation bug (§2.1), and one
wasted render from an undocumented locale trap (`scale(1,997500)`).

The marginal cost of an eleventh speaker on a warmed-up batch is roughly:
one fence-fill, one token-fill, one screenshot, one verification look.
Everything else — reading four docs, measuring the template, the ratio
check, resolving 13 of the ~20 shell tokens — is identical for every
speaker in a batch, **but the skill frames processing as a per-folder loop**
("for each folder in `to-process/`, in order: … measure both placeholders
…"). Followed literally, that is five identical ~40-second mask passes for
one answer, and 8–10 calls per speaker (~45–60 per batch) instead of ~25.
The skill's own §4 batch-discipline rule (same `base_image`, `card_width`,
`desc_lines` across a batch) already guarantees the loop recomputes
constants.

### Where the wall-clock actually goes

1. **Headless-Chromium cold starts** — each `--screenshot` is a fresh
   browser launch; five launches per batch was executor B's dominant
   wall-clock cost. The render itself is fast.
2. **Visual verification** — the one genuinely irreducible per-speaker step
   is also the most expensive, because each 1200×1200 PNG enters the
   assistant's context whole.
3. **Ceremony with no consumer** — the fence → frontmatter transfer copies
   every value into YAML in the same file, yet the render step reads the
   fences again anyway (fences win by contract). The transfer is a record,
   not an input. Similarly, the mandated photo rescale duplicates what the
   shell's `object-fit: cover` already does; it is the only step that
   mutates an operator asset, and it exists mostly so the archive carries a
   slot-sized copy.
4. **The temp-then-copy screenshot dance** (browser cannot write into
   Desktop folders) doubles the file operations per render — an environment
   tax, unavoidable as long as the repo lives under Desktop on Windows.

### Verdict

The pipeline is **architecturally efficient** — one deterministic
token-fill and one screenshot per speaker, no build system, no network,
no iteration loops — but the **procedure narrates it inefficiently** (per
-folder framing of batch-scoped work) and its **quality gate leaks** (§2.1),
which converts cheap renders into expensive investigations when it fires.

---

## 2 · Correctness findings that tax efficiency

Found independently by both executors; each costs real calls whenever it
triggers.

1. **The 140-character description budget over-promises — the one real
   defect.** Measured two-line capacity at the card's true content width
   (364px, Inter 12/16) is ~115–121 characters depending on word breaks. A
   character count passes text that the renderer then clamps — exactly the
   "silent truncation" the contract promises never happens.
   *Both executors hit it (4 of 10 descriptions). One halted and
   re-authored; one delivered and flagged — the docs do not say which is
   correct, so the two did opposite things. `vaclav-havel-final.png`
   shipped with its description clipped mid-sentence and needs a
   re-render once the budget question is settled.*
2. **The green colour-mask rule is ambiguous on the canon template** — it
   also matches the Apify wordmark's green triangle; a literal bounding-box
   read returns a 3×-wrong speaker block. Both executors had to invent a
   "largest connected component" rule the docs don't state.
3. **The "dilate 3px" instruction harms machine-built templates** (blocks
   are pixel-exact; dilation pushes the ratio check to the edge of its own
   tolerance and inflates the photo slot).
4. **The worked-example geometry (200.5 / 748.028 / 306.949) cannot be
   produced by the documented integer-mask method** — a correct
   implementation looks wrong against the docs' own example.
5. **The photo-archive instruction is self-contradictory** in the default
   case: archive the normalized copy *as* `speaker.png` while leaving the
   operator's `speaker.png` untouched. Each executor invented a different
   filename for the derived copy.
6. Smaller frictions: the verification checklist demands "untouched
   template areas identical" while the hover ring necessarily repaints a
   band outside the block; "no purple or green anywhere" is unachievable
   (the wordmark is green); the `[type here]` decoy inside the operator
   callout; no invariant-culture note for `CARD_SCALE`; the stale
   `processed/jana-novakova` archive contradicts the live contract while
   CLAUDE.md points at `processed/` as worked examples.

---

## 3 · Portability (fresh clone on a new machine)

- **Tier 1 — generate teasers from a bare clone: ✅ works today.**
  Everything rendering needs is committed (template, fonts, shell, icon,
  particles.svg, skills); paths are repo-relative; nothing is fetched; the
  only requirement is a preinstalled Chromium, and the docs say so.
- **Tier 2 — evolve the template from a bare clone: ❌ blocked.** The
  pristine pre-starfield template source lives only on the operator's
  Desktop, and the starfield/block-stamping recipe exists as prose plus
  scripts in a session scratchpad that dies with the machine. Fix: commit
  the source PNG into `visual-templates/`; decide separately on the recipe
  (next point).
- **The `.ps1` question (evaluated, per operator request — nothing removed).**
  The repo currently ships **zero executable scripts**; that is a feature,
  not an accident — "the assistant is the engine" is the design, and it
  keeps the clone-trust story clean: many people are rightly wary of
  cloning a repo that contains PowerShell scripts, and corporate policy
  often blocks them outright. Committing the stamping/preflight helpers
  would buy reproducibility but spend that trust, and would quietly turn a
  docs-driven pipeline into a scripts-driven one that then needs
  maintenance, review, and cross-platform care. The middle path, if
  reproducibility is wanted: keep operational logic as **prose recipes the
  assistant re-derives** (today's model), and put any committed automation
  in **inert, in-browser form** — HTML+JS pages like the shell itself,
  which no one double-clicks into a shell session. The self-verification
  proposal below is deliberately shaped this way: it adds JavaScript to a
  render page, not scripts to the repo.

---

## 4 · Top recommendation

**Make the batch the unit of work, and make the render page verify
itself.** (Both executors proposed halves of this independently; together
they remove the biggest fixed cost and the only correctness defect at
once, and neither half adds a shell script to the repo.)

1. **Batch preamble in the skill**: measure the template once, ratio-check
   once, resolve the batch-constant tokens once; then a thin per-speaker
   body (fence-fill → per-speaker tokens → render → look). Same outputs,
   ~threefold fewer steps on a five-speaker batch, and one Chromium
   session can serve all N screenshots instead of N cold starts.
2. **Self-verifying shell**: a few lines of JS in `shell.html` (enabled by
   a `{{DEBUG}}`-style token) that measure the description's rendered line
   count, the card's real height, and a canvas sweep for surviving
   placeholder colour, and paint the verdict as a tiny strip in an unused
   corner of the page. The screenshot the assistant already inspects then
   carries machine-checkable facts alongside the picture. This replaces
   the drift-prone character budget with the real gate — *rendered lines ≤
   desc_lines* — and the Havel truncation becomes impossible to ship: it
   would have been caught on the first render at zero extra cost, instead
   of costing one executor ten calls to diagnose and the other a defective
   deliverable.

Runner-up (cheap, worth doing regardless): fix the five §2 doc defects —
budget number or line-gate, largest-component mask rule, drop the dilation
for machine-built templates, integer worked example, name the derived
photo file — and define explicitly whether a rendered-line overflow halts
the folder (it should; both executors should have behaved identically).

---

*Status at time of writing: the ten test teasers are in `final-output/`
(`vaclav-havel` needs a re-render once the budget decision lands; three
fantasy descriptions were legitimately re-authored shorter by the executor
after a halt). The regenerated docs and this file are uncommitted. Three
fantasy portraits and the Czech-outline logo are CC BY-SA — publishing
those four teasers carries an attribution obligation; exact credit lines
are in the prep manifests.*
