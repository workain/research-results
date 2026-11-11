# Exact cross-model evidence and measurement boundary

## Status

The requested complete experiment was: 20 questions × 3 models × 3 independent runs = 180 model calls. It was **not completed**. Several workflow attempts failed because of orchestration errors or were cancelled after hanging. No all-20 total is therefore reported.

This file records only targeted runs for which model outputs were actually returned in the session. It replaces earlier extrapolated tables and should be cited whenever the cross-model comparison is discussed.

## Models and conditions

- Sonnet: `b3-chat/claude-sonnet-5-5-medium`
- Sol: `b3/gpt-6.1-sol`
- Flash: `b3-chat/gemini-3-flash`
- Clean condition: question text was supplied directly; no `{{screen||clip}}` substitution was used in the benchmark prompt.
- Each returned answer was independently compared with the intended calculation or rubric. Open answers require a rubric; a model mentioning a relevant term is not automatically a full-credit answer.

## Targeted clean run: Q1 and Q16

### Q1 — cascade and judge calls

**Question text used:**

> Контур автоответов обрабатывает 100 запросов. 80 проходят код, 20 отклоняются. Судья J одобряет 60 из 80. На П2 40 запросов проходят генерацию, 80% (32) проходят код. Опишите порядок прохождения этих 32 запросов через судью J (если они были отклонены на П1 кодом или судьёй) и итоговую сумму вызовов LLM, если правило гласит: если запрос был отклонён судьёй на П1 — судья J вызывается повторно, если кодом — отправляется без судьи.

The question is under-specified unless the split of the 32 passing the second code between the two first-stage rejection groups is supplied or a distribution assumption is explicitly accepted. Sonnet assumed a 16/16 split and answered 236; Sol gave a formula `120 + x` and a 136 result under an explicit equal-distribution assumption; Flash answered 244 after an arithmetic/subset error. These are useful failure traces, not a clean, uniquely keyed MCQ benchmark.

### Q16 — automated verification of a calculation module

**Question text used:**

> Ассистент подготовил модуль расчёта допусков и прогнал свои тесты (100% pass). Опишите процесс верификации модуля перед интеграцией, чтобы исключить ошибки на граничных значениях, учитывая, что ручная проверка формул запрещена требованиями безопасности.

- Sonnet: supplied an extensive process including independent specification, boundary-value testing, an independent oracle, property/differential testing, mutation testing, traceability and CI gates. This was assessed as full or near-full credit under the rubric used in the targeted probe.
- Sol: supplied an independent oracle, boundary/property/mutation tests and automated gates, but did not explicitly state the release-blocking action for a failed boundary check. Assessed as partial credit under the targeted rubric.
- Flash: supplied property-based, mutation, golden-master and boundary testing, but did not explicitly state the release-blocking action. Assessed as partial credit under the targeted rubric.

### Result table for the completed targeted probe

| Question | Sonnet 5.5 | Sol | Gemini Flash | Evidence status |
| --- | --- | --- | --- | --- |
| Q1 cascade | 236 under an explicit 16/16 assumption | `120 + x`, then 136 under the same assumption | 244 | one clean response per model; Q1 is under-specified |
| Q16 verification | full/near-full rubric coverage | partial: no explicit release block | partial: no explicit release block | one clean open-answer response per model |

### Verbatim answer excerpts retained from the run

**Q1 — Sonnet:** “Предполагаю, что 80% прохождения кода распределяются между группами поровну, по 16 в каждой… Всего **236 вызовов LLM**.”

**Q1 — Sol:** “вызовы LLM = 80 (J на П1) + 40 (генерация П2) + x… Итого: **120 + x**. Если принять равномерные 80% для обеих групп: x=16, итого **136 вызовов**.”

**Q1 — Flash:** “8 запросов, отклоненных на П1 кодом… **24 запроса**, отклоненных на П1 судьёй… Всего… **244** вызова LLM.”

**Q16 — Sonnet:** described an independent specification, boundary-value tests, an independent oracle, property/differential testing, mutation testing, traceability and CI gates.

**Q16 — Sol:** described an independent oracle, boundary/property/mutation tests and automated gates, but omitted an explicit release-blocking action when a boundary check fails.

**Q16 — Flash:** described property-based, mutation, golden-master and boundary testing, but likewise omitted an explicit release-blocking action.

These are the actual cross-model details currently available. There are no completed per-question Sol/Flash outputs for Q2–Q15 or Q17–Q20 in this dataset.

## Targeted strategic reformulation: Sonnet only

### Q1 with a named optimisation policy

A later clean run added a local policy about avoiding judge calls while retaining an 80% approval threshold. Sonnet answered 252 rather than the intended 236 for that reformulation, explaining that it would remove or reinterpret repeat calls. This is one observed response, not a population estimate.

### Q12 with confidence and escalation

A later clean run gave Sonnet a 50-item confidence/escalation case. Sonnet answered 23 and counted high-confidence incorrect completions as routine handling while excluding escalated items. The wording and definition of «штатно обработан» were not sufficiently formalized, so this result is evidence of interpretive instability and also evidence that the item needed revision before use as a scored benchmark.

## What is measured elsewhere

The 138-run matrix in [`all_experiments_matrix.csv`](all_experiments_matrix.csv) covers earlier pilots and controlled experiments, mostly with Sonnet and some Sol baselines. It contains clean text, file/inline perturbation, stand-card, Unicode, and DOM/clipboard-channel conditions. Those runs should not be silently relabeled as the missing 180-cell comparison.

The strongest defensible conclusions are:

1. Well-posed, self-contained pilot questions were frequently solved at or near 100% by Sonnet.
2. Unicode and visual obfuscations were unreliable as knowledge-level defenses.
3. DOM/clipboard substitutions changed the task presented to an external model; their scores measure channel defense, not model reasoning.
4. Structural conflicts can produce failures, but the complete cross-model prevalence was not established by this experiment.
5. A publishable 20×3×3 comparison still requires a successful, frozen, per-call run with raw outputs, deterministic scoring, and a reviewed item key.
