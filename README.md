# Înmulțiri — two browser games

Two self-contained multiplication games for roughly 6–11 year olds, plus a picker
homepage. Romanian interface throughout. No build step, no dependencies, no network
at runtime.

```
index.html                 game picker (homepage)
pasii-lui-rex/index.html   Pașii lui Rex — times tables to 10 × 10
teren-de-creta/index.html  Terenul de Cretă — multiplication as area
```

Open `index.html` in a browser, or serve the folder. GitHub Pages deploys the repo
root as-is via `.github/workflows/pages.yml`.

---

## Pașii lui Rex

**Where the idea came from.** Four concepts were pitched on the unmerged
`psticea-multiplication-game-pitches` branch (`GAME_IDEAS.md`). This is built on
**#3, Gearling Express** — the only one where the number the player sets is a real
machine parameter rather than a quiz answer, and the only one that drills both
directions of a fact. The train skin was dropped for dinosaurs; the reveal-by-
skip-counting scaffold is borrowed from **#4, Echo Grove**.

**The change that made it work.** Gearling's track is a straight line, which cannot
show a distance of 100 on a phone. Here the trail is a **hundred-square** — ten rows
of ten stones, boustrophedon, so consecutive stones are always exactly one cell apart
and a step of 8 is always the same visible length. Multiples of one number then fall
into the same standing pattern every round: 9s make a staircase, 5s make two columns.

**One input, three questions.** The child always does the same thing — touch a stone.
The number under the finger floats in a bubble well above it, so the hand never covers
the answer, and the choice commits on release.

| Question | Plaque reads | Touch | Practises |
| --- | --- | --- | --- |
| `produs` | `7 × 8 = ?` | where Rex lands | recall of the product |
| `multipli` | `pași de 8` + three eggs | the egg he reaches exactly | multiples / divisibility |
| `factor` | `7 pași → 56` | where his first step falls | the missing factor |

Rex then walks it. Each stomp lights its stone, pops the running total in gold and
climbs a pentatonic scale, so 8 · 16 · 24 · 32 is heard as well as seen. A wrong
answer is never punished: he still walks the true path, the chosen stone gets a
dashed `?`, the real multiples light up in sequence, and the fact is re-asked once,
three rounds later. Ten eggs per expedition, one baby dinosaur per egg.

Three correct in a row rolls fog over the valley: every stone number fades except the
every-ten ruler, so the second half of a run has to be done from memory. A miss
lifts it again.

### Tuning constants

All named in `FEEL` and the generator, so the game can be re-tuned without reading it.

| Constant | Value | Controls |
| --- | --- | --- |
| `ROUNDS` | 10 | eggs per expedition (≈3–4 min) |
| `TYPES` | 5 / 3 / 2 | produs / multipli / factor per run, `produs` always first |
| `runBudgetS` | 1.55 s | a whole walk aims to land inside this, whatever the step count |
| `stepMinS` – `stepMaxS` | 0.17 – 0.40 s | per-step animation floor and ceiling |
| `markPopS` | 1.10 s | how long a landing number stays popped in gold |
| `resultS` | 1.85 s | hatch beat before the next question |
| `revealStepS` / `revealTailS` | 0.13 / 1.15 s | pace of the "here is the real path" replay |
| `shakePx` / `shakeS` | 2 px / 0.12 s | screenshake, correct answers only, never on a miss |
| `hitstopS` | 0.045 s | frame hold on a hatch |
| `fogAtStreak` | 3 | streak at which the numbers fade |
| `DT` | 1/120 s | fixed simulation step; accumulator clamped at 0.25 s |
| touch target | 1.00 cell | stones are *drawn* at 0.86 cell, so the hitbox is deliberately larger than the sprite |
| `devicePixelRatio` | capped at 2 | a 3× phone costs frames for no visible gain |

### Level generation guarantees

- Factors 2–10 on both sides; every product ≤ 100.
- At least **three products above half the chosen group's maximum**, so a run is never
  all easy ones. Slots are reserved for them rather than hoped for.
- No `(steps, size)` pair repeats inside one expedition.
- A `multipli` round is only generated when two decoy eggs exist that are within ±5 of
  the answer, are *not* multiples of the step, are at least 2 apart from each other,
  and are still reachable inside 100 stones.
- A re-queued miss that lands in a slot it cannot support falls back to `produs`.
- Everything draws from a seeded `mulberry32`; there is no bare `Math.random()` in the
  simulation, so a run is reproducible from its seed.

---

## Terenul de Cretă

Unchanged apart from its new folder and a link back to the picker. Its own design
notes live in `PRODUCT.md`. It teaches multiplication as **area** — pull a rectangle
around a painted number until the squares match — which is why the two games sit next
to each other rather than one replacing the other: one makes a product a *shape*, the
other makes it a *place*.

---

## Known deviations from the game brief

Expected to be non-empty, per the brief.

1. **No Vite / TypeScript / vitest build.** The brief calls the bundler
   non-negotiable. `PRODUCT.md` calls a single self-contained `index.html` with no
   build step non-negotiable, and the Pages workflow uploads the repo root as-is.
   The repo's own constraint won. The cost is real: there is no pure simulation layer
   under unit test, so the §9 simulation checks were done in a browser instead (see
   below), which is weaker evidence.
2. **Not a one-input arcade loop with a decision several times a second.** The brief's
   template targets ages 5–8 and a twitch mechanic. A product has to be *thought*, not
   reacted to, so the decision here is roughly one per ten seconds and there are ten of
   them. The input is still a single gesture that never changes.
3. **No timer and no session countdown.** A clock would punish exactly the slow,
   careful arithmetic the game exists to build, and `PRODUCT.md` principle 5 ("wrong is
   cheap") rules it out. The run is bounded by ten eggs instead, with progress pips
   always visible.
4. **Deployed to GitHub Pages, not Vercel**, following the workflow already in the repo.
5. **`og.png` is a real frame**, generated by screenshotting an actual 7 × 4 run at
   1200 × 630 rather than authored as a logo.

## What was verified, and what was not

Verified in Chromium against the served files: a full ten-round playthrough covering
all three question types and the end screen; a deliberate wrong answer and its reveal;
keyboard-only start, arrow-key selection, `Enter` commit, `M` menu and `Escape` close;
portrait 390 × 844 and landscape 844 × 390 and 1200 × 630 with nothing clipped; zero
console errors and zero third-party requests.

Not verified, and not claimed: Lighthouse scores, frame rate under 4× CPU throttling,
behaviour on real touch hardware, audio output, and — the one that matters most — an
actual child playing it cold.
