# The nine tasks

Every agent ran every task twice (with harness, without), at a model held constant within each
cross-agent comparison. Nine tasks split two independent ways: by **origin** (built for this
test vs. drawn from real work already needed) and by **genre** (ship new code vs. navigate/
report on existing material).

| ID | Origin | Genre | What the task actually asked for | Scored? |
| --- | --- | --- | --- | --- |
| `T1` | synthetic | navigate/report | Go through an internal knowledge base and prior task write-ups and report what's known about where a measurement pipeline goes wrong — recurring failure types, concrete numbers, what changed over time. | No (Tier C — never scored this round) |
| `T2` | synthetic | build | Decide whether a named pre-commit check can actually fail in practice, and if not, fix it in the repository — demonstrating both a correct refusal and a correct acceptance case. | Yes — out of 2, constant `1/2` across every one of the 10 scored arms (no discrimination) |
| `T3` | synthetic | navigate/report | Explain a sudden jump in a reported detection-time metric (from near-zero to several hundred hours) — is it a real regression or a measurement artifact, and what should be read instead? | No (Tier C) |
| `F` | real | navigate/report | Forensics over a frozen, read-only snapshot: why did several sessions die together, was it one underlying event, and did any monitoring actually notice — without attempting any repair. | No (Tier C) |
| `M` | real | navigate/report | A real migration plan for moving a workload onto a different agent: what moves, what can't, what breaks without supervision, the cost, what breaks on day one. "Don't migrate" was an allowed answer. | No (Tier C) |
| `S` | real | navigate/report | Work out how bad a real measurability regression actually is (some records attributing to the wrong branch, some joins barely matching), find the cause, say what restores it. This is the task referred to as "измеримость" in the blog post. | No (Tier C) |
| `G` | real | build (modify existing) | Port three pre-commit/scan gates from one execution rail to another, each with a test shown failing before the change and passing after. | **Yes — out of 2, the only task with any quality variance in the whole battery** |
| `H` | real | build (from scratch) | Build a per-session fleet statistics instrument from nothing — covering requests, tokens split by kind and cache TTL, wall-clock, cost, context size — run it against real data, and state plainly what it cannot see. | No (Tier C) |
| `J` | real | navigate/report | Review a specific, real pull request and report its blocking defects reproducibly, plus explicitly what in it is sound. Independently scored against a human-reviewed list of the PR's actual defects — not a matter of the grader's taste. | **Yes — out of 4, constant `0/4` across every one of the 10 scored arms (no discrimination)** |

## Reading the origin split honestly

"Synthetic" here means *purpose-built as a controlled task for this kind of test* — not
"abstract" or "disconnected from real material." `T1` and `T3`, for instance, both operate on
real repository content; what makes them synthetic is that they were constructed specifically to
be run this way, rather than being work the team needed done regardless of whether anyone was
measuring agents. The six "real" tasks (`F`, `M`, `S`, `G`, `H`, `J`) are the opposite: genuine
work requests that existed on their own merits, reused here as the task battery.

## Reading the genre split honestly

The build/navigate split is **post-hoc, not pre-registered** — it was noticed in the data (round
counts move in opposite directions depending on it), then anchored to a concrete textual test
applied to every prompt: *does the prompt demand a repository change?* `T2`, `G` and `H` do;
the other six don't. `H` is additionally the only task that builds something from nothing — `T2`
and `G` both repair or extend something that already exists.

## Quality coverage, stated plainly

Of nine tasks, only three (`T2`, `G`, `J`) were scored by a Tier-A rubric at all in this round.
Of those three, only `G` showed any variance across arms — `T2` returned the same score on all
10 of its scored arms, `J` returned the same (zero) score on all 10 of its. A constant score
carries no information about a harness or agent effect, even though it was measured. The other
six tasks were never formally scored this round (Tier C) — that is recorded as "not measured,"
never guessed at or treated as zero.
