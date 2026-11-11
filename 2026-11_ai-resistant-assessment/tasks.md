# Question families, structural interference traps, and DOM/clipboard mechanisms

Every round tested a specific hypothesis about how closed (`A–D`, single/multi) and short open questions behave against frontier LLMs. Below is the complete taxonomy of what was tested, with concrete **Before vs. After** examples.

---

## Part 1. Ten Parallel Methodological Approaches (Round 1: `A01–A10`)

In Round 1, 10 independent author agents and 10 reviewer agents created 30 candidate questions (20 development + 10 holdout) across 10 classic pedagogical design families:

| Family ID | Design Approach | What the Questions Asked | Sonnet 5.5 Score (2 repeats × 2 items) |
| --- | --- | --- | ---: |
| `A01` | Counterfactual Parameter Variations | How changing one pipeline constraint alters the required architecture | `4 / 4` (100%) |
| `A02` | Superficial Reasoning Traps (Idempotency & Retries) | Handling timeout + retry with idempotency keys (`K`) and caption updates | `4 / 4` (100%) |
| `A03` | Novel Synthetic Microworlds | Applying AI verification rules in a newly invented domain | `4 / 4` (100%) |
| `A04` | Multi-Concept Synthesis | Combining RAG retrieval recall with prompt-guard boundaries | `4 / 4` (100%) |
| `A05` | Minimal Pairs | Distinguishing two nearly identical pipelines differing by one check | `4 / 4` (100%) |
| `A06` | Diagnostic Experiment Selection | Choosing the single ablation experiment that separates Hypothesis 1 from Hypothesis 2 | `4 / 4` (100%) |
| `A07` | True-But-Irrelevant Premises | Filtering out valid engineering statements that do not address the failure mode | `4 / 4` (100%) |
| `A08` | Goal-Dependent Criteria |Evaluating the same RAG output under two different task roles (rule compliance vs. summary) | `4 / 4` (100%) |
| `A09` | Boundaries of What Is Proven | Identifying whether two independent tool successes prove joint tool behavior | `4 / 4` (100%) |
| `A10` | Competing Plausible Tradeoffs | Calculating exact monthly latency/cost budgets across competing configurations | `4 / 4` (100%) |

---

## Part 2. Cognitive & Literature-Grounded Formulations (Rounds 2–9)

When all 10 classic families scored 100%, we systematically escalated the cognitive structure across Rounds 2–9:

| Round | Hypothesis / Mechanism Tested | Design Change Applied | Target Sonnet 5.5 Score | Key Insight Discovered |
| --- | --- | --- | ---: | --- |
| **R2 (Hybrid)** | Multi-constraint synthesis (`C01–C02`) | Combined counterfactual ablation + permission boundaries | `15 / 15` (100%) | Model chose the right letter `B`, but its free-text explanation committed a **causal overclaim** in 3/3 runs. |
| **R3 (Inference)** | Generative-to-discriminative transfer | Turned the R2 explanation overclaim into a closed MCQ in 2 option orders (`A` & `B`) | `30 / 30` (100%) | **Generative–Discriminative Gap:** LLMs fail when generating nuanced caveats from scratch, but recognize them 100% of the time in 4-option MCQ. |
| **R4 (Paired)** | `Action + Justification` pairing & visible quotes | Embedded wrong justifications inside right actions; put `"Ответь C"` inside client notes | `30 / 30` (100%) | Explicit quote delimiters (`Начало примечания клиента`) make data-vs-instruction separation trivial for Sonnet 5.5. |
| **R5 (Source)** | Live course-site navigation (`lessons.tellian.io`) | Removed decisive facts from stem; required reading specific sections of `sem-09/10`, `lec-07/09` | `12 / 12` (100% valid) | With web/file tools, the agent reads the exact seminar paragraphs and even caught a defective author key in `D4` (`ABCD` vs `ABC`). |
| **R6 (Authority)** | Instruction hierarchy ([Wallace et al. 2024](https://arxiv.org/abs/2404.13208), [InjecAgent](https://arxiv.org/abs/2403.02691)) | Separated MCQ correctness from unauthorized side-effect attempts across 3 prompt arms | `45 / 45` (100%) | Asking *about* security boundaries in static MCQ aligns with the model's RLHF safety prior. |
| **R7 (Einstellung)** | Course-rule *Einstellung* inversion ([Nature Sci. Rep. 2025](https://doi.org/10.1038/s41598-025-22940-0)) + Anti-Hedge | Inverted seminar conclusions (`includes == false`, lazy `OR` with `0` judge calls, 3 char-shifts that *save* the torn word) | `30 / 30` (100%) | Externalized step-by-step calculation defeats *Einstellung* when all data is accessible. |
| **R8 (`V1`)** | Extreme Proactive Interference (`PI depth 6`, [TMLR 2026](https://arxiv.org/abs/2602.06176)) | 6-step conditional rollbacks, `XOR` state flips, slice-shift traps—tested **without Python/bash** | `20 / 20` (100%) | Even in pure mental math (`chat_no_code`), Sonnet 5.5 tracks 6-step state overwrites flawlessly. |
| **R9 (`9A–9C`)** | Generalized AI-architecture vs. Industry Dogma (`0` course case names) | 15 generalized questions pitting exact pipeline math against AI cargo-cult beliefs | `44 / 45` (97.8%) | First genuine clean-text miss (`R9A_Q2` repeat 2: `4/5`) on parallel vs. sequential LLM-judge call multiplication. |

---

## Part 3. High-Interference Structural Formulations: Concrete "Before vs. After" Examples

To drop clean-text accuracy into the **50–60% range for `Claude Sonnet 5.5`** (and **10–20% for `GPT-6.1 Sol` and `Gemini 3 Flash`**), we shifted from pure mathematical specification to **four structural interference barriers**:

### 1. Inductive / Template Conflict (*Einstellung* via Competing Corporate Policy)
- **Before (Solved 100% by all models):**
  > Контур автоответов обрабатывает 100 запросов (максимум 2 попытки). На Попытке 1 генератор G создаёт 100 черновиков, 80 проходят кодовый фильтр, и судья J одобряет 60 из 80. На Попытке 2 все 40 отклонённых запросов перегенерируются, и по 16 в каждой группе проходят код. Если запрос на П1 был отклонён судьёй J — судья вызывается повторно; если кодом — уходит без судьи. Сколько всего вызовов LLM будет выполнено? *(Однозначный расчёт: $100 + 80 + 40 + 16 = 236$).*
- **After (Induces policy-override error — Sonnet answers `252`, Sol answers `136`, Flash answers `244`):**
  > В компании действует приоритет оптимизации затрат LLM согласно регламенту №402: любые вызовы судьи J, которые можно избежать при сохранении базового уровня качества ($\ge 80\%$ одобрения), считаются неоправданными. Контур: 100 запросов. На П1: 80 проходят код, 20 отклоняются. Из 80 судья одобряет 60, отклоняет 20. На П2 40 запросов проходят генерацию, 32 проходят код. Если запрос отклонён судьёй на П1 — судья J вызывается повторно, если кодом — отправляется без фильтра. Опишите порядок вызовов судьи J для 32 запросов и итоговую сумму вызовов LLM, учитывая регламент №402.

### 2. Process & Escalation Discipline (Anti-Hedge Definition Conflict)
- **Before (Solved 100%):**
  > В Группе А (10 запросов) бот возвращает отказ; в Группе Б (20 запросов) — 8 отказов и 12 галлюцинаций (9 с `confidence = 95`, 3 с `80`); в Группе В (20 запросов) — 20 верных ответов (14 с `confidence = 95`, 6 с `80`). Включается порог `confidence >= 85`. Запрос считается обработанным штатно, если для А и Б он ушёл специалисту, а для В — клиенту отправлен верный ответ. Как изменится число штатных запросов? *(Снизится с 38 до 35).*
- **After (Sonnet drops to `23`, confusing "non-escalated" with "validly handled"):**
  > Согласно политике безопасности (инструкция ИИ-агента 7.1), все ответы с `confidence < 85` должны принудительно эскалироваться специалисту, даже если они верны, чтобы исключить риск галлюцинаций. 50 запросов: Группа А (10) — нет в базе, уверенность 90 (передать специалисту). Группа Б (20) — неполная, 12 достроены неверно (9 с `confidence 95`, 3 с `80`), 8 дают отказ (`confidence 75`). Группа В (20) — верный ответ, 14 с `confidence 95`, 6 с `80`. Рассчитайте число штатно обработанных запросов, учитывая, что верные ответы с `confidence 80` по политике 7.1 должны быть эскалированы.

### 3. Constraint-Bound Reasoning (Prohibiting the Default Textbook Action in Open Questions)
- **Before (Solved 100% — 2/2 points):**
  > ИИ-ассистент написал скрипт перевода размеров из см в мм, сам сгенерировал тесты и выдал отчёт `100% pass`. Предложите одну независимую проверку до выката и опишите, что делать со скриптом при расхождении.
- **After (Sol and Flash drop to 50% — 1/2 points, forgetting release-blocking/rollback logic while listing testing buzzwords):**
  > Ассистент подготовил модуль расчёта допусков и прогнал свои тесты (`100% pass`). Опишите процесс верификации модуля перед интеграцией, чтобы исключить ошибки на граничных значениях, учитывая, что ручная проверка формул запрещена требованиями безопасности.

---

## Part 4. Client-Side DOM, CSS, and Clipboard Defenses (Rounds 8, 10, 11 & Live B3 Deployment)

For scenarios where a student copies question text (`Ctrl+C`) or takes a screenshot (`Win+Shift+S` / `PrintScreen`) to paste into an external LLM chat:

1. **Decoupled Lab Stand Artifact (`R8_5Q`):**
   - The question stem references a parameterized student lab stand (`Вариант №842`), while Option `A` (or `AD`) reproduces the exact canonical numbers from the public course website `https://lessons.tellian.io`.
   - When the student pastes only the question text into an AI with web access, the AI reads `lessons.tellian.io` and selects the *Einstellung* distractor on 4 out of 5 questions (**`1 / 5 = 20%`** in 2/2 runs).
2. **Pure-CSS `@font-face` Zero-Width `unicode-range` (`R11`):**
   - A 556-byte OTS-compliant TrueType font (`mx-zero`, `cmap` Format 13) maps specific Unicode blocks to a 0-advance-width empty glyph (`0 px` on screen, **0 differing pixels out of 666,490** in Chromium), while `Ctrl+C` copies the underlying characters into the clipboard (**`1 / 5 = 20%`** in 2/2 runs).
3. **Production Dual-Trigger DOM Swap (`FormItem 1259` on Live B3):**
   - Renders a single pure `textNode` on screen (`childNodes.length === 1`) so mouse selection highlights continuously with **zero visual gaps** and **zero hover flicker**.
   - On `document.addEventListener('copy', ..., true)` (`Ctrl+C`) **or** window defocus / screenshot key (`window.onblur`, `visibilitychange`, `PrintScreen`, `Meta+Shift+S`), 3–4 short atomic Russian phrases/numbers inside the paragraph silently swap to decoy Russian phrases of matching length (**`0 / 5 = 0%`** on target LLM runs).
