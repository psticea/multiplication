# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Children roughly 6–11, across a wide skill spread — some are meeting the times
tables for the first time, others are drilling fluency and factor pairs. Play is
unsupervised as often as not: a kid on a phone or tablet, sometimes a classroom
laptop, frequently handed the device with no adult reading the screen aloud.
Parents and teachers are a secondary audience who look at the game once, to
decide whether it is worth the child's time.

## Product Purpose

Teach multiplication as **area** rather than as a memorised table. The player
sees a number, then physically builds a rectangle whose square count equals that
number — so 12 stops being a symbol to recall and becomes a shape that can be
2×6, 3×4, or 6×2. Success is a child who reaches for a shape when they meet an
unfamiliar product, and who notices without being told that the same number has
several rectangles.

## Positioning

Most multiplication apps are quiz software wearing a costume: a question, a text
field or four choices, a score. This is a spatial puzzle first — the maths is
the mechanic, not a layer of questions bolted onto an unrelated game. The
Shikaku-derived twist is that cleared rectangles *vanish*, so the board is also
a planning problem: small numbers must often be cleared first to open room for
the large ones.

## Operating Context

- Played in short sessions, one board at a time, most often on a phone in
  portrait with a thumb or index finger covering part of the screen.
- No login, no network, no persistence expectations. Opening the file is the
  whole install.
- Touch is the primary input; mouse and trackpad must work identically.
- Frequently played in noisy or public settings, so sound is a bonus and never
  load-bearing.

## Capabilities and Constraints

- Interface language is **Romanian** throughout — every visible string, screen
  reader announcement, and document metadata. Digits, `×` and `=` are drawn as
  a hand-built stencil glyph set on canvas and are language-neutral.
- Single self-contained `index.html`. No build step, no server, no API keys, no
  hotlinked assets. All art generated programmatically; all sound synthesised
  via Web Audio.
- Board is always **10 rows tall**; the player picks the width (3–10) before
  playing.
- Exactly **5 numbered targets** per board.
- Level generation must guarantee solvability, and must guarantee large products
  appear: two targets larger than half the board, one at least a third of it,
  and no multiples of ten (×10 is too easy to be worth a turn).
- A rectangle is anchored on its numbered square, may not contain any other
  *remaining* number, and clears only on an exact area match.
- Cleared rectangles vanish completely, freeing their squares.
- Must run correctly in portrait and landscape, and must not zoom, scroll, or
  pan the mobile viewport during a drag.

## Brand Commitments

None. The previous name and look are explicitly retired by the owner and carry
no authority over the replacement.

## Evidence on Hand

The prior implementation (`index.html` at commit 90dde0e) is the working
reference for game rules, level generation, and solvability checking. Its visual
language is an anti-reference, not a source. No real users, testimonials,
classroom data, or endorsements exist; none may be invented.

## Product Principles

1. **The number is a shape.** Every affordance should push the child toward
   seeing area, not toward recalling a fact.
2. **Never hide the answer behind the hand.** On touch, the live readout must
   stay clear of the finger at all times — an occluded readout breaks the one
   feedback loop the game has.
3. **Big products must appear.** A board of easy small numbers defeats the
   purpose; the generator's size guarantee is a rule, not a tuning knob.
4. **No reading required to play.** A child who cannot yet read the menu must
   still be able to start, understand feedback, and win.
5. **Wrong is cheap.** Failure costs nothing but a moment — no lives, no timer,
   no score to protect. Exploration is the learning.

## Accessibility & Inclusion

Numbers must stay legible at the smallest supported cell size. Feedback must
never be carried by colour alone, since red/green is the exact pair that fails
most commonly for colour-blind players. Controls must be reachable and operable
with a thumb on a phone held one-handed.
