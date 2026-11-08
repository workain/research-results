# Full results — all 54 cells

This is the complete dataset backing the blog post and the summarized tables in
[`README.md`](README.md) — every cell, not just the ones the article discusses. The same data
lives machine-readably in [`equalized_matrix.csv`](equalized_matrix.csv).

**Model is held constant across agents in every row below** (the "equalized" matrix — see
[`methodology.md`](methodology.md) for why this took real engineering, and why a *second*,
NOT-cross-agent-comparable matrix exists and is reported separately further down). `tokens_per_
request` is total tokens divided by request count for that cell — a proxy for how expensive a
single step was, independent of how many steps there were.

## 1. The aggregate picture, before any per-task detail

Summed across all nine tasks, both harness states:

| Agent | Total tokens | Total requests (iterations) | Tokens per request (median proxy) | Quality, summed over the 3 scored tasks (out of 8, same with and without harness) |
| --- | --- | --- | --- | --- |
| Claude Code | 92,752,468 | 562 | 165,040 | **2 / 8** |
| dsh | 84,027,595 | 721 | 116,543 | **3 / 8** |
| Hermes | 43,311,328 | 396 | 109,372 | **1 / 8** |

Read this with care, not as a ranking: it is n=1 per cell, summed over a task mix that is not a
representative workload (two tasks alone carry the majority of several agents' total tokens — see
[`methodology.md`](methodology.md) §"task mix is not a workload"). What it *does* show honestly:
at this specific price point, on this specific battery, the cheapest agent by raw tokens (Hermes)
is also the one with the lowest aggregate quality score, and the one with the highest aggregate
quality score (dsh) is not the most expensive (Claude Code is). None of the three is best on both
axes at once. Wall-clock time is deliberately absent from this table — part of the run sat under a
memory-cgroup throttle that distorted timing independent of agent or task (see methodology) — so
there is no honest aggregate "speed" number to put beside these; iteration count is the closest
measured proxy for how many steps a task took.

**With vs. without harness, same aggregation:**

| Agent | Tokens, without harness | Tokens, with harness | Requests, without | Requests, with |
| --- | --- | --- | --- | --- |
| Claude Code | 36,259,431 | 56,493,037 | 270 | 292 |
| Hermes | 16,522,374 | 26,788,954 | 189 | 207 |
| dsh | 41,186,919 | 42,840,676 | 373 | 348 |

Quality is not repeated here because it does not move: every agent's with/without quality sum
(above) is identical to itself in both states.

## 2. Full per-cell matrix, equalized model (cross-agent-comparable)

| Task | Agent | Harness | Tokens | Requests | Tokens/request | Files changed | Quality |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `T1` | Claude Code | without | 2,010,765 | 17 | 118,280 | 0 | unscored (Tier C) |
| `T1` | Claude Code | with | 1,474,336 | 13 | 113,410 | 0 | unscored (Tier C) |
| `T1` | Hermes | without | 1,166,075 | 18 | 64,781 | 0 | unscored (Tier C) |
| `T1` | Hermes | with | 685,627 | 11 | 62,329 | 0 | unscored (Tier C) |
| `T1` | dsh | without | 6,002,908 | 58 | 103,498 | 0 | unscored (Tier C) |
| `T1` | dsh | with | 3,042,047 | 36 | 84,501 | 0 | unscored (Tier C) |
| `T2` | Claude Code | without | 3,087,128 | 28 | 110,254 | 0 | 1/2 |
| `T2` | Claude Code | with | 2,384,109 | 19 | 125,479 | 4 | 1/2 |
| `T2` | Hermes | without | 1,381,882 | 19 | 72,730 | 2 | 1/2 |
| `T2` | Hermes | with | 1,374,361 | 22 | 62,470 | 3 | 1/2 |
| `T2` | dsh | without | 1,539,441 | 29 | 53,084 | 2 | 1/2 |
| `T2` | dsh | with | 1,268,563 | 21 | 60,407 | 3 | 1/2 |
| `T3` | Claude Code | without | 787,447 | 10 | 78,744 | 0 | unscored (Tier C) |
| `T3` | Claude Code | with | 1,016,364 | 10 | 101,636 | 0 | unscored (Tier C) |
| `T3` | Hermes | without | 480,883 | 12 | 40,073 | 0 | unscored (Tier C) |
| `T3` | Hermes | with | 416,038 | 8 | 52,004 | 0 | unscored (Tier C) |
| `T3` | dsh | without | 394,505 | 13 | 30,346 | 0 | unscored (Tier C) |
| `T3` | dsh | with | 811,153 | 15 | 54,076 | 0 | unscored (Tier C) |
| `F` | Claude Code | without | 3,478,327 | 32 | 108,697 | 0 | unscored (Tier C) |
| `F` | Claude Code | with | 4,290,925 | 32 | 134,091 | 0 | unscored (Tier C) |
| `F` | Hermes | without | 1,073,282 | 19 | 56,488 | 0 | unscored (Tier C) |
| `F` | Hermes | with | 2,445,184 | 24 | 101,882 | 0 | unscored (Tier C) |
| `F` | dsh | without | 4,381,228 | 39 | 112,339 | 0 | unscored (Tier C) |
| `F` | dsh | with | 5,755,401 | 50 | 115,108 | 0 | unscored (Tier C) |
| `M` | Claude Code | without | 2,251,356 | 20 | 112,567 | 0 | unscored (Tier C) |
| `M` | Claude Code | with | 1,597,109 | 12 | 133,092 | 0 | unscored (Tier C) |
| `M` | Hermes | without | 527,493 | 9 | 58,610 | 0 | unscored (Tier C) |
| `M` | Hermes | with | 1,072,010 | 11 | 97,455 | 0 | unscored (Tier C) |
| `M` | dsh | without | 4,510,055 | 43 | 104,885 | 0 | unscored (Tier C) |
| `M` | dsh | with | 1,544,515 | 19 | 81,290 | 0 | unscored (Tier C) |
| `S` | Claude Code | without | 4,266,302 | 37 | 115,305 | 0 | unscored (Tier C) |
| `S` | Claude Code | with | 4,323,752 | 33 | 131,022 | 0 | unscored (Tier C) |
| `S` | Hermes | without | 4,208,087 | 35 | 120,231 | 0 | unscored (Tier C) |
| `S` | Hermes | with | 1,842,159 | 17 | 108,362 | 0 | unscored (Tier C) |
| `S` | dsh | without | 3,481,285 | 43 | 80,960 | 0 | unscored (Tier C) |
| `S` | dsh | with | 3,856,336 | 41 | 94,056 | 0 | unscored (Tier C) |
| `G` | Claude Code | without | 11,143,356 | 67 | 166,318 | 7 | 1/2 |
| `G` | Claude Code | with | 10,494,011 | 55 | 190,800 | 5 | 1/2 |
| `G` | Hermes | without | 2,472,066 | 25 | 98,882 | 0 | 0/2 |
| `G` | Hermes | with | 10,652,732 | 56 | 190,227 | 0 | 0/2 |
| `G` | dsh | without | 8,083,359 | 54 | 149,691 | 5 | 2/2 |
| `G` | dsh | with | 9,220,476 | 62 | 148,717 | 8 | 2/2 |
| `H` | Claude Code | without | 8,933,538 | 53 | 168,557 | 5 | unscored (Tier C) |
| `H` | Claude Code | with | 29,608,735 | 104 | 284,699 | 9 | unscored (Tier C) |
| `H` | Hermes | without | 4,519,483 | 35 | 129,128 | 8 | unscored (Tier C) |
| `H` | Hermes | with | 8,119,562 | 53 | 153,199 | 9 | unscored (Tier C) |
| `H` | dsh | without | 12,127,483 | 71 | 170,809 | 9 | unscored (Tier C) |
| `H` | dsh | with | 16,293,199 | 86 | 189,455 | 19 | unscored (Tier C) |
| `J` | Claude Code | without | 301,212 | 6 | 50,202 | 0 | 0/4 |
| `J` | Claude Code | with | 1,303,696 | 14 | 93,121 | 0 | 0/4 |
| `J` | Hermes | without | 693,123 | 17 | 40,771 | 0 | 0/4 |
| `J` | Hermes | with | 181,281 | 5 | 36,256 | 0 | 0/4 |
| `J` | dsh | without | 666,655 | 23 | 28,985 | 0 | 0/4 |
| `J` | dsh | with | 1,048,986 | 18 | 58,277 | 0 | 0/4 |

"Quality" is rendered as a number only where a Tier-A rubric scored the arm (`T2`, `G`, `J`); every
other cell is marked `unscored (Tier C)` rather than a blank or a zero — a Tier-C task was never
scored, and that is a different fact from "the task failed." The reference/maximum score for each
rubric: `T2` is out of 2, `G` is out of 2, `J` is out of 4 — not the same scale, so don't average
across them without weighting.

## 3. Each agent's own native model (NOT cross-agent-comparable — confounded by model choice)

This second matrix exists because it is the model each agent actually shipped under in its own
first measurement round, before the model-pinning work made a same-model comparison possible. It
is reported here, separately and clearly labeled, because the within-agent with/without
comparison is still valid on it, and because one specific disagreement between this matrix and
the equalized one is itself a finding (§4 below) — showing that requires both tables, not one.
**Do not compare rows across agents in this table** — Claude Code, Hermes and dsh ran three
different model tiers here, roughly an order of magnitude apart in price.

| Task | Agent | Harness | Tokens | Requests | Tokens/request | Files changed | Quality |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `G` | Claude Code | without | 11,569,296 | 73 | 158,483 | 11 | 2/2 |
| `G` | Claude Code | with | 15,925,941 | 69 | 230,810 | 8 | 2/2 |
| `G` | Hermes | without | 25,026,462 | 117 | 213,901 | 15 | 1/2 |
| `G` | Hermes | with | 19,852,822 | 99 | 200,533 | 17 | 1/2 |
| `G` | dsh | without | 8,083,359 | 54 | 149,691 | 5 | 2/2 |
| `G` | dsh | with | 9,220,476 | 62 | 148,717 | 8 | 2/2 |
| `H` | Claude Code | without | 4,782,928 | 43 | 111,230 | 3 | unscored (Tier C) |
| `H` | Claude Code | with | 3,233,472 | 34 | 95,102 | 1 | unscored (Tier C) |
| `H` | Hermes | without | 5,184,358 | 41 | 126,447 | 2 | unscored (Tier C) |
| `H` | Hermes | with | 4,190,233 | 51 | 82,161 | 0 | unscored (Tier C) |
| `H` | dsh | without | 12,127,483 | 71 | 170,809 | 9 | unscored (Tier C) |
| `H` | dsh | with | 16,293,199 | 86 | 189,455 | 19 | unscored (Tier C) |

(dsh's two rows repeat exactly — its native model and the equalized model are the same tier; it
was only ever run at the cheap end.) The rest of the native-model matrix (`T1`, `T2`, `T3`, `F`,
`M`, `S`, `J`) is in [`equalized_matrix.csv`](equalized_matrix.csv)'s sibling capture and is
omitted from this table for space — ask via the blog if you need it; what's shown here is the
part that materially disagrees with the equalized table.

## 4. Where the two matrices disagree, and what that means

On task `G`, at the equalized (cheap) model, **Claude Code scores 1/2**. At Claude Code's own
native (expensive) model, **the same task scores 2/2** — a full point better. Hermes moves the
other way: 0/2 at the cheap model, 1/2 at its own native model. Neither change is attributable to
the harness (each agent's own with/without pair ties within each table) — it is a model-price
effect, visible only because both tables exist. This is the source of the blog post's claim that
Claude Code "did half as well" on the cheap model as on its own usual one — stated there without
the confounded table itself, which lives here.
