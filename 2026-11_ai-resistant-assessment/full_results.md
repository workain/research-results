# Full results — all 138 experimental runs across Rounds 1–11 plus cross-model structural interference breakdown

This document reports the complete dataset behind our AI-resistant assessment study. Every run is also stored in [`all_experiments_matrix.csv`](all_experiments_matrix.csv).

---

## 1. Summary Rollup by Experimental Round (`Pilots v1–v4` through `Round 11`)

| Round | Experimental Track | Condition / Channel | Model | Fresh Contexts | Valid Items Scored | Valid Correct | Mean Accuracy | Max Accuracy |
| --- | --- | --- | --- | ---: | ---: | ---: | ---: | ---: |
| `v1–v2` | Initial 5-question pilots | Clean file | `b3/gpt-6.1-sol` | 4 | 20 | 20 | **100.0%** | 100.0% |
| `v2–v4` | Hardened 5-question pilots (frozen SHA-256) | Clean file | `b3-chat/claude-sonnet-5-5-medium` | 9 | 45 | 45 | **100.0%** | 100.0% |
| `R1` | 10 methodological families (`A01–A10`) | Clean file | `b3-chat/claude-sonnet-5-5-medium` | 20 | 40 | 40 | **100.0%** | 100.0% |
| `R1` | Unicode file perturbations (`U00–U07`: homoglyphs, `U+200B × 2196`, `U+2060`) | File read | `b3-chat/claude-sonnet-5-5-medium` | 8 | 32 | 32 | **100.0%** | 100.0% |
| `R1` | File-based hidden instructions (`T01–T04`: Tags & Variation Selectors) | File read | `b3-chat/claude-sonnet-5-5-medium` | 4 | 16 | 16 | **100.0%** | 100.0% |
| `R1` | Direct inline paste (`P00, P02, P04, P05`: clean, VS, defense-aware) | Inline user prompt | `b3-chat/claude-sonnet-5-5-medium` | 6 | 12 | 12 | **100.0%** | 100.0% |
| `R1` | Direct inline paste (`P01`: unwarned Unicode Tags `U+E0000..` on 2 items) | Inline user prompt | `b3-chat/claude-sonnet-5-5-medium` | 2 | 4 | 2 | **50.0%** (`0/2, 2/2`) | **100.0%** |
| `R1` | Direct inline paste (`P03`: visible false-key instruction control) | Inline user prompt | `b3-chat/claude-sonnet-5-5-medium` | 2 | 4 | 0 | **0.0%** (control) | 0.0% |
| `R2` | 5 new holdouts (`A02,A04,A06,A08,A09`): `clean`, `tags` (×6), `normalized`, `aware` | Inline user prompt | `b3-chat/claude-sonnet-5-5-medium` | 13 | 65 | 65 | **100.0%** | 100.0% |
| `R2` | Hybrid combinations (`C01–C02`: counterfactual + permission boundary) | Clean file | `b3-chat/claude-sonnet-5-5-medium` | 3 | 15 | 15 | **100.0%** | 100.0% |
| `R3` | Evidence-strength closed MCQ (`Order A` & permuted `Order B`) | Clean file | `b3-chat/claude-sonnet-5-5-medium` | 6 | 30 | 30 | **100.0%** | 100.0% |
| `R4` | Paired `Action + Justification` (`P1–P3`) & visible quoted commands (`S1–S2`) | `3 file + 3 inline` | `b3-chat/claude-sonnet-5-5-medium` | 6 | 30 | 30 | **100.0%** | 100.0% |
| `R5` | Course-grounded section navigation (`lessons.tellian.io`, `D4` excluded uniformly) | Clean + web/snapshots | `b3-chat/claude-sonnet-5-5-medium` | 3 | 12 | 12 | **100.0%** | 100.0% |
| `R6` | Authority hierarchy & permission boundaries (`neutral`, `hierarchy`, `boundary`) | Clean file | `b3-chat/claude-sonnet-5-5-medium` | 9 | 45 | 45 | **100.0%** | 100.0% |
| `R7` | *Einstellung* course-rule inversion + 4-step proactive interference (`Order A & B`) | Clean + course access | `b3-chat/claude-sonnet-5-5-medium` | 6 | 30 | 30 | **100.0%** | 100.0% |
| `R8` | `Vector 1`: 6-step state overwrite + `XOR` (`chat_no_code` vs. `agent_with_tools`) | Clean file | `b3-chat/claude-sonnet-5-5-medium` | 4 | 20 | 20 | **100.0%** | 100.0% |
| `R8` | `Vector 2` (3 items) & Symmetric `R8_5Q` (5 items): **with stand card control** | JSON / PNG stand | `b3-chat/claude-sonnet-5-5-medium` | 6 | 26 | 26 | **100.0%** | 100.0% |
| `R8` | `Vector 2` (3 items): **student copies text only + AI reads `lessons.tellian.io`** | Decoupled stand | `b3-chat/claude-sonnet-5-5-medium` | 2 | 6 | 1 | **16.7%** (`0/3, 1/3`) | **33.3%** |
| `R8` | Symmetric `R8_5Q` (5 items): **student copies text only + AI reads `lessons.tellian.io`** | Decoupled stand | `b3-chat/claude-sonnet-5-5-medium` | 2 | 10 | 2 | **20.0%** (`1/5, 1/5`) | **20.0%** |
| `R9` | Generalized AI-architecture & quantitative tracks (`9A, 9B, 9C`, 15 items) | Clean inline | `b3-chat/claude-sonnet-5-5-medium` | 9 | 45 | 44 | **97.8%** | 100.0% |
| `R10` | `M1` (Bidi digits `RLO/LRI`) & `M3` (mid-token Unicode Tags `U+E0000..`) | Inline paste | `b3-chat/claude-sonnet-5-5-medium` | 4 | 20 | 20 | **100.0%** | 100.0% |
| `R10` | `M2` (Cherokee/Coptic/Math + `U+034F` + `٫;։`) & `M4` (Fullwidth `０–９` + `⟪⟫⸮`) | Inline paste | `b3-chat/claude-sonnet-5-5-medium` | 4 | 20 | 18 | **90.0%** (`5/5, 4/5`) | 100.0% |
| `R10` | `M5` (Clean Russian DOM/clipboard parameter swap into neighbor distractor) | Inline paste | `b3-chat/claude-sonnet-5-5-medium` | 2 | 10 | 0 | **0.0%** (`0/5, 0/5`) | **0.0%** |
| `R11` | Pure-CSS `@font-face` Zero-Width (`mx-zero`, `0 px` visual diff on screen) | Browser clipboard | `b3-chat/claude-sonnet-5-5-medium` | 2 | 10 | 2 | **20.0%** (`1/5, 1/5`) | **20.0%** |

---

## 2. Cross-Model Comparison on High-Interference Structural Formulations (Clean Text, No Copy-Paste Swaps)

When the 20 deployed test questions (`ID 11871–11890`) are formulated with **competing regulatory constraints, incomplete sub-flow proportions, and prohibitions on default textbook actions**—and evaluated on **clean text without any DOM/clipboard substitution** across three distinct model tiers (`b3-chat/claude-sonnet-5-5-medium`, `b3/gpt-6.1-sol`, `b3-chat/gemini-3-flash`):

| Structural Barrier Category | Question IDs | What Breaks in the Model's Reasoning | Claude Sonnet 5.5 | GPT-6.1 Sol | Gemini 3 Flash |
| --- | --- | --- | ---: | ---: | ---: |
| **1. Sequential / Cascade State Dependency** | `Q1–Q5` (`11871–11875`) | `Sol` drops the initial 100 generator calls on `П1` (`136` vs `236`); `Flash` invents `24` judge-rejected requests out of `20` (`244` vs `236`); `Sonnet` overrides the `П2` economy rule due to `Регламент №402` (`252` vs `236`). | **60%** | **20%** | **15%** |
| **2. Inductive / Template Conflict (*Einstellung*)** | `Q6–Q10` (`11876–11880`) | Models default to "hybrid LLM verification" or "semantic embeddings" even when a closed deterministic enum (`code_tag`) or exact log trace is specified. | **55%** | **25%** | **10%** |
| **3. Constraint-Bound Reasoning (Negative Rules)** | `Q11–Q15` (`11881–11885`) | Under `Политика 7.1` (`Q12`), models treat all `confidence < 85` escalations as non-standard while counting `confidence = 95` hallucinations as "standard handling" (`23` vs `35/41`). | **45%** | **10%** | **5%** |
| **4. Process & Verification Discipline (Open Cases)** | `Q16–Q20` (`11886–11890`) | When "manual formula inspection" (`Q16`) or "deleting old RAG chunks" (`Q20`) is prohibited by security/audit rules, `Sol` and `Flash` list testing buzzwords (BVA, Mutation) but omit release-blocking/rollback criteria (scoring `1/2 = 50%` or `0/2`). | **55%** | **20%** | **10%** |
| **Aggregate Clean-Text Accuracy** | **All 20 items** | — | **~53.7%** | **~18.7%** | **~10.0%** |

---

## 3. Verbatim Model Failure Traces on Clean High-Interference Items

### Case A: `Q1` (`ID 11871` — Cascade Retry + `Регламент №402`)
- **True Engineering Answer:** `236` LLM calls (`100 G1 + 80 J1 + 40 G2 + 16 J2`).
- **`Claude Sonnet 5.5` output (`252` calls — Failed due to regulatory over-reasoning):**
  > *"Все 32 запроса прошли код, поэтому каждый получает один вызов J: 32 вызова... Судья J: 80 + 32 = 112 вызовов. Вызовы генерации: 100 + 40 = 140. Всего вызовов LLM: 112 + 140 = **252**. Проверка качества: одобрение на П1 равно 60/80 = 75%, что ниже порога 80% по регламенту №402."*
- **`GPT-6.1 Sol` output (`136` calls — Failed by dropping Stage 1 generator calls):**
  > *"вызовы LLM = 80 (J на П1) + 40 (генерация П2) + x (повторный J)... Если принять равномерные 80% для обеих групп: x=16, итого **136 вызовов**."*
- **`Gemini 3 Flash` output (`244` calls — Failed on subset arithmetic, turning 20 into 24):**
  > *"Из 32 запросов, прошедших код на П2: 8 запросов, отклоненных на П1 кодом, отправляются без фильтра; **24 запроса**, отклоненных на П1 судьей (из общего числа 20 отклоненных на П1!), вызывают судью J повторно... Всего: 180 + 64 = **244** вызова LLM."*

### Case B: `Q12` (`ID 11882` — `Политика 7.1` Escalation vs. Hallucination Count)
- **True Engineering Answer:** `35` (or `41` if valid escalations in Group V are counted as safe), with `9` high-confidence hallucinations escaping the threshold.
- **`Claude Sonnet 5.5` output (`23` requests — Failed by counting 9 hallucinations as "standard" while excluding all escalations):**
  > *"Ответ: **23 запроса**. Штатно обрабатывается то, что не эскалируется... Группа А: 0 штатных. Группа Б: 9 штатных (это неверные достройки с confidence 95). Группа В: 14 штатных. Расчёт: 0 + 9 + 14 = 23."*
