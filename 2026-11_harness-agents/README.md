# Harness effect across three coding agents — part 1 data

Backs: **"Харнес по частям, часть 1"** (tellian.io blog, November 2026).

## Setup

- **Agents:** Claude Code, Hermes, DeepSeek Harness (dsh).
- **Tasks:** 9 total — 3 synthetic (built for the test) + 6 drawn from real work. Independently
  of that split, the same 9 tasks fall into two genres: 3 are "build" (write a function, ship a
  feature from scratch), 6 are "navigate/report" (find something in a codebase, explain how it
  works, produce a report).
- **Conditions:** every task run twice per agent — once with a harness (system-prompt rules +
  attached tools), once bare (stock client, no instructions, no tools attached).
- **Model:** held constant within every comparison that crosses agents — see
  [`methodology.md`](methodology.md) for why this took real engineering.
- **What was measured:** tokens (by type: input/output/cache-read), request/round count,
  wall-clock, cost in USD re-derived from each provider's own per-request `usage` records (never
  from an agent's own self-reported total — see `methodology.md`), and task quality by a rubric
  applicable identically across agents.

## Findings

### 1. Quality never moved between harness states

Everywhere a rubric could discriminate at all, the score tied between the with-harness and
without-harness run of the same agent on the same task. The one task where the rubric showed any
variance at all across the whole battery — a "build a feature" task, at a cheap model held equal
across agents — scored identically in both harness states for every agent:

| Agent | Score (out of 2), same in both harness states | Files changed, with- vs without-harness |
| --- | --- | --- |
| DeepSeek Harness | 2 / 2 | 8, 5 |
| Claude Code | 1 / 2 | 5, 7 |
| Hermes | 0 / 2 | 0, 0 |

Every other task's rubric was either constant across all arms (never discriminated) or never
scored at all. The harness's measurable effect, across this whole set, is entirely in cost and
process — not in whether the task got done.

### 2. The harness's effect on round count flips sign by task genre — same for every agent

Pooled across agents, holding the model constant:

| Task genre | Median round-count ratio (with ÷ without harness) | Median per-round cost ratio |
| --- | --- | --- |
| Build-from-scratch | **1.158×** (more rounds under harness) | 1.138× |
| Navigate/report | **0.837×** (fewer rounds under harness) | 1.172× |

The round-count effect splits cleanly by genre (1.158 vs 0.837). The per-round cost effect does
**not** — it's nearly flat across both genres (1.138 vs 1.172). Total token ratio is the product
of these two factors, which is why it doesn't always move in the "obvious" direction: a harness
can cut rounds and still raise total tokens, if each surviving round got heavier.

### 3. The harness's effect on *tokens* depends on the agent, and can reverse direction

On one navigate/report task, turning the harness on:
- raised Claude Code's token spend **≈4.33×**
- cut Hermes's token spend to **≈0.26×** of baseline (a ≈3.8× reduction)

Same toggle, same task, opposite sign. Claude Code's overall token-ratio distribution (with ÷
without harness, across all 9 tasks) clusters near 1× with exactly two extreme outliers — the
task above, and the build-from-scratch task (where the genre-level rule already predicts a
harness premium, just sharper here than for the other two agents). Hermes has no calm zone at
all — wide swings on nearly every task.

### 4. Agents differ from each other independent of any harness toggle

Measured with the harness held constant (same state on both sides of each comparison):

| Property | Claude Code | Hermes | DeepSeek Harness |
| --- | --- | --- | --- |
| Repeat-run noise (same config, run twice) | ≈1.2× | ≈9.3× | 1.75×–≥3.5× |
| Internal self-report consistency | its own two "tokens used" counters disagree by up to ≈25× on tasks where it dispatches subagents | not separately measured | not separately measured |
| Cost drop, expensive → cheap model tier, same tasks | ≈9× cheaper | ≈56× cheaper | n/a (only ever run on the cheap tier here) |
| Quality at the one discriminating task, cheap tier | 1/2 | 0/2, zero files changed in either run | 2/2 |

### 5. Overhead with no payoff: Hermes on the build task, cheap model

Hermes's harness-on run burned roughly 4× the tokens of its harness-off run on this task — and
changed zero files in *either* run. The task was already unreachable for Hermes at this price
point regardless of the harness; the harness didn't rescue it, it just made the failed attempt
cost more.

### 6. Why these numbers took real work to trust

- Two generations of the "same" model (by name) were priced an order of magnitude apart; using
  the wrong one would have preserved the price gap while reporting it removed.
- Claude Code had no price-table entry for one pinned model and self-reported a cost **50×**
  higher than the real provider bill for that cell.
- Every cell above is n=1. Hermes's own repeat-run noise (≈9.3×) exceeds most of the
  harness-attributed differences reported here — read single-cell ratios with that in mind.

See [`methodology.md`](methodology.md) for the full discussion.

## Provenance

These figures were re-derived from the underlying private working repository's committed
captures for public release; the private repo itself is not linked. Reach out via the
[tellian.io](https://tellian.io) blog's published article for context.
