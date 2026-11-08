# Methodology notes

## Task set

9 tasks total, full list with real one-line descriptions in [`tasks.md`](tasks.md):

- **3 synthetic** (`T1`, `T2`, `T3`) — purpose-built as controlled tasks for this kind of test.
  "Synthetic" does not mean abstract or free of real context — `T1` and `T3` both operate on real
  repository content — it means they were constructed specifically to be run this way, rather
  than being work anyone needed regardless of measurement.
- **6 real** (`F`, `M`, `S`, `G`, `H`, `J`) — genuine work requests that existed on their own
  merits, reused here as the task battery.

Independently of synthetic-vs-real, the same 9 tasks split by **genre**:

- **3 build** tasks (`T2`, `G`, `H` — write/fix/port code in the repository; `H` alone builds
  something from nothing, `T2` and `G` extend or repair an existing artifact).
- **6 navigate/report** tasks (`T1`, `T3`, `F`, `M`, `S`, `J` — locate something, explain how it
  works, produce a report, with no required repository change).

This genre split is the axis the round-count finding (see main README, §2) is organized around.
It was identified after the fact, from the data — not pre-registered — which is why it's
reported as a pattern worth naming rather than a confirmed hypothesis. The discriminator applied
to each prompt was textual and checkable: *does the prompt demand a repository change?*

## Task mix is not a workload

Two of the nine tasks (`G` and `H`) alone account for the majority of several agents'
with-harness token totals in the equalized matrix — up to ~71% for one agent. An aggregate over
all nine tasks is therefore close to a statement about those two specific tasks, not a balanced
read across "typical" work. The median per-task ratio is the more honest single number to quote;
a raw sum (as in `full_results.md` §1) is reported too, but should be read as "what a bill would
look like if this exact nine-task mix were the real workload," which it isn't.

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

## Four reporting asymmetries, fixed before any cross-agent number was trusted

All four are properties of how the three agents report themselves — not defects in any
measurement taken here.

1. **Hermes's "input tokens" field double-counts the cache.** Hermes reports `input_tokens` as
   the *whole* prompt side of a request, cache-read tokens included. Claude Code and dsh report a
   true fresh-only remainder for the equivalent field. Summing Hermes's fields naively therefore
   charges every one of its cached tokens twice. Measured directly: `input_fresh − cache_read −
   cache_write` leaves a near-zero remainder on every Hermes cell checked (worst case 0.0067% of
   the field) — the field *is* the sum of the cached components for Hermes, and is not for the
   other two agents (0 of 54 cross-agent cells showed the same pattern). Within a single agent's
   own with/without ratio this mostly cancels; across agents it does not cancel at all, and this
   is exactly the bug that inflated cost by ~10× on affected cells (see the blog post's "ловушки"
   discussion).
2. **"Rounds" is three different units across three agents.** Claude Code's own round counter is
   agent turns, not observable as raw API requests without an independent relay. Hermes's is its
   own API-call counter. dsh's is raw HTTP requests counted at the wire by a relay. A with/without
   ratio of "agent turns" and one of "wire HTTP requests" are each meaningful within their own
   agent and are **not the same quantity** across agents. Every cross-agent statement in this
   dataset is built on **API requests** (one unit for all three, independently re-derived), never
   on each agent's own `rounds` field — the two disagree on 16 of 54 equalized cells.
3. **A pricing-table gap** (described above) — Claude Code reported a cost ≈50× too high on a
   model it had no price-table row for.
4. **Disagreeing internal token-usage counters** (described above) — up to ≈25× apart on tasks
   where Claude Code dispatched subagents.

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

## Why wall-clock time is not reported

Part of this measurement round ran under a memory-cgroup limit on a shared machine that throttled
one agent's relay mid-run; the affected attempts were discarded and re-run, but the surviving
wall-clock figures still mix real agent latency with incidental infrastructure load in a way that
can't be cleanly separated after the fact. No wall-clock number in this dataset is offered as a
cross-agent or cross-harness latency comparison. Tokens and price were re-derived from each
provider's own per-request accounting, which is independent of what else was happening on the
machine at measurement time — that is why this dataset leans on tokens/price/requests and not on
the clock.

## What this dataset does not claim

- No formal statistical test is run anywhere here — comparisons are reported as observed ratios
  and medians, not as significance-tested effects.
- The genre classification (build vs navigate/report) is post-hoc, not pre-registered.
- Quality is scored on one rubric per task; most of the 9 tasks' rubrics never showed any
  variance across any arm, so they carry no information about a harness or model effect even
  though they were scored.
- This is one battery of 9 tasks on one day with one set of model versions. It is evidence about
  *these* agents on *these* tasks at *this* price point — not a general ranking.
