# Methodology notes

## Task set

9 tasks total:

- **3 synthetic** — built specifically for this test, to allow pinning a known-correct answer.
- **6 real** — drawn from genuine work (find/explain/report on existing code, or extend it).

Independently of synthetic-vs-real, the same 9 tasks split by **genre**:

- **3 build-from-scratch** tasks (write a function / ship a feature with no prior scaffolding).
- **6 navigate/report** tasks (locate something, explain how it works, produce a report).

This genre split is the axis the round-count finding (see main README, §2) is organized around.
It was identified after the fact, from the data — not pre-registered — which is why it's
reported as a pattern worth naming rather than a confirmed hypothesis.

## Why the model had to be held constant

Comparing agents by cost or token count only means something if they're not secretly also being
compared on *which model generation they happened to run*. Two versions of the same model family
differed in price by roughly an order of magnitude; letting that vary silently between agents
would have produced a "cost comparison" that was actually a "who got the expensive model" result.
Every cross-agent number in this dataset is from a comparison where the model was pinned
identically across the agents being compared.

Pinning it was not just a config flag:

- One agent's model-selection CLI flag resolved, by default, to the *more expensive* of two
  model generations with a similar-looking name — the opposite of the cheap tier the comparison
  needed.
- Another agent's model choice lived in a persistent config file shared across all of that
  agent's sessions on the box; changing it had to be done without mutating that shared file
  (snapshot-and-restore, verified by hash before and after every run).

## Why self-reported cost can't be trusted

Every dollar figure in this dataset is re-derived from each provider's own per-request `usage`
payload (input/output/cache-read/cache-write token counts, multiplied by that model's published
per-token price), never from an agent's own self-reported running total.

Two concrete reasons this mattered:

1. **A pricing-table gap.** One agent had no entry in its own internal price table for a model it
   was pinned to run, and fell back to pricing it at a different (far more expensive) model's
   rate — reporting a cost **50× higher** than the real provider bill for that cell. Had totals
   been taken from self-reported cost, this would have shown up as "this agent is expensive,"
   when the actual defect was a missing row in a lookup table.
2. **Disagreeing internal counters.** One agent exposes more than one field that claims to report
   "tokens used" for a session. On tasks where it dispatched its own subagents, these fields
   disagreed with each other by as much as ≈25× — both still labeled identically from the
   outside. Any pipeline that reads "tokens used" from this agent needs to know which of its
   fields it's actually reading.

The repricing pipeline used here was cross-checked against independently-known-correct figures
(the provider's own per-request billing records, and each agent's own cost figure on the cells
where that figure is independently known to be right) before being trusted for cells where no
independent check exists.

## Why every number here is n=1, and what that means for reading it

Every (agent, task, harness-state) cell was run once. Separately, the same exact configuration
was repeated on a few cells purely to measure how much a second run of the *identical* setup
would differ from the first, with nothing changed. That repeat-run spread is the honest noise
floor for anything claimed above it:

| Agent | Repeat-run spread observed |
| --- | --- |
| Claude Code | ≈1.2× |
| DeepSeek Harness | 1.75× to at least ≈3.5× |
| Hermes | up to ≈9.3× |

Read every single-cell ratio in the main findings against this table. A reported harness effect
that is smaller than an agent's own repeat-run noise is not yet distinguishable from that agent
simply being noisy — this applies most to Hermes, whose own noise ceiling (≈9.3×) is close to or
larger than several of the "effects" discussed in the write-up.

## What this dataset does not claim

- No formal statistical test is run anywhere here — comparisons are reported as observed ratios
  and medians, not as significance-tested effects.
- The genre classification (build vs navigate/report) is post-hoc, not pre-registered.
- Quality is scored on one rubric per task; most of the 9 tasks' rubrics never showed any
  variance across any arm, so they carry no information about a harness or model effect even
  though they were scored.
- This is one battery of 9 tasks on one day with one set of model versions. It is evidence about
  *these* agents on *these* tasks at *this* price point — not a general ranking.
