# Building Reflex

A narrative of how Reflex was built, prompt by prompt.

---

## 1. Endless mode

> Make the game run indefinitely. The only stop conditions are: user presses Esc, and 3 consecutive turns are missed (no user input). In both cases, show an informative banner.

The game had a configurable round count (`numRounds`). That was removed in favor of truly endless play. A `consecutiveMisses` counter was added — any key response (correct or wrong) resets it, but letting a token expire increments it. At 3 consecutive misses the game ends with an "Are you still there?" banner. Pressing Escape shows a "Game Paused" banner instead. The `endGame` function was refactored to accept a reason and display a contextual title and explanation in the game-over overlay.

## 2. Ship it

> push everything and create the pr

Pushed to the feature branch. PR creation via `gh` hit auth issues against the local proxy — left for manual follow-up.

## 3. Sparkline metrics

> Let's add smetric: events per unit of time (minute?) and average reaction time. The player should see a sparkline on the right side of the screen for each of these metrics, showing their magnitude over time as the game progresses. Make it a sliding window, only showing the last minute of gameplay.

Two sparkline cards were added, fixed to the right edge of the screen and vertically centered. The top card tracks **events per minute** (purple line) — every round outcome (correct, wrong, or miss) is timestamped and counted within a 60-second sliding window, then scaled to a per-minute rate. The bottom card tracks **average reaction time** (gold line) — only correct hits contribute, averaged over the same window.

Under the hood: an `eventLog` and `rtLog` record raw timestamps. A 1-second interval samples both, prunes entries older than 60 seconds, computes the two metrics, and pushes them into fixed-length history arrays. A `drawSparkline` function renders each array onto a HiDPI canvas as a filled gradient area with a stroked line and a dot on the latest value. The panel fades in on game start and out on game end. Hidden on narrow viewports.

## 4. This document

> I want to share the steps we took to build Reflex. Write a brief narrative that includes my prompts (verbatim, no modifications) and your responses (brief summaries only, verbatim only if already short). This will be a markdown file in the repo.

You're reading it.
