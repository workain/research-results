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

## 2. Cross-model evidence boundary

The requested full benchmark — all 20 deployed questions × 3 models × 3 independent runs — **was not completed**. Several workflow attempts failed or were cancelled before producing a complete answer matrix. The earlier draft's all-20 percentages were extrapolations and are not reported as measured results here.

The exact completed targeted observations, including the full question text used for the two main probes, are in [`cross_model_evidence.md`](cross_model_evidence.md). The 138-run matrix in section 1 is a separate set of earlier pilot, structural and channel experiments and must not be confused with the unfinished 180-cell cross-model benchmark.

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
