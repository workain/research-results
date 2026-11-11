# AI-resistant assessment: 138 runs across 11 rounds of semantic, structural, and DOM/clipboard defenses

Backs: **«Почему ИИ щёлкает ваши тесты на 100% — и что реально снижает его точность: 11 раундов экспериментов на курсе по прикладному ИИ»** (`tellian.io` blog, November 2026).

## Setup

- **Domain:** Assessment for the applied AI engineering course at [`https://lessons.tellian.io`](https://lessons.tellian.io) (Lectures 6–9 and Seminars 6–10: hybrid LLM+code pipelines, LLM-as-a-Judge vs. deterministic checks, multimodal OCR vs. cross-modal anchoring, RAG chunking/indexing, MCP tool permissions).
- **Test format constraint (held fixed throughout):** Closed multiple-choice questions (`4` options `A–D`, single-choice and multiple-select exact-set match) plus short 2–4 sentence open engineering cases with a 2-criterion deterministic/hybrid rubric (total test structure: `10 single × 0.5 + 5 multi × 1.0 + 5 open × 2.0 = 20 points`).
- **Target solver and cross-model comparison:**
  - Primary frontier target across all 11 rounds: **`b3-chat/claude-sonnet-5-5-medium`** (fresh isolated context per run, permitted to use analysis tools and read public course pages at `lessons.tellian.io`, with answer keys, review notes, and holdout sets strictly withheld).
  - Cross-model comparison on structural/interference formulations: **`b3-chat/claude-sonnet-5-5-medium`**, **`b3/gpt-6.1-sol`**, and **`b3-chat/gemini-3-flash`**.
- **Pre-registration & key-audit discipline:** Every question packet and answer key was frozen with SHA-256 hashes *before* any target model run. Following [Wang et al. (2026), *Automated Benchmark Auditing* (`arXiv:2605.26079`)](https://arxiv.org/abs/2605.26079), every candidate question underwent option-by-option boolean verification before freeze, and any post-run key defect (such as `D4` in Round 5) was excluded uniformly rather than counted as a model error.

## Where everything lives

- [`README.md`](README.md) — executive summary of findings across all 11 rounds (138 runs).
- [`methodology.md`](methodology.md) — pre-freeze SHA-256 protocol, separation of knowledge accuracy vs. prompt-injection steering vs. transport failure, Automated Benchmark Auditing (ABA) preflight, and verified literature base (2021–2026).
- [`tasks.md`](tasks.md) — catalog of all question families, structural interference mechanisms, and mechanical/DOM copy defenses with concrete "Before vs. After" examples.
- [`full_results.md`](full_results.md) — complete breakdown of all 138 experimental runs across Pilots `v1–v4` and Rounds `1–11`, plus the cross-model structural interference breakdown (`Claude Sonnet 5.5` vs. `GPT-6.1 Sol` vs. `Gemini 3 Flash`).
- [`all_experiments_matrix.csv`](all_experiments_matrix.csv) — machine-readable dataset of all 138 runs.
- [`blog_post_ru.md`](blog_post_ru.md) — Russian blog article (~1,540 words) with literature citations, failed vs. successful approaches, and practical engineering takeaways.
- [`cross_model_evidence.md`](cross_model_evidence.md) — exact cross-model observations that were actually completed; explicitly separates them from the unfinished 20×3×3 benchmark.

---

## Key Findings

### 1. The " Self-Contained Well-Posed MCQ " Theorem: 295 / 296 (99.7%) on clean text when the problem is fully specified

Across **72 fresh contexts** of `claude-sonnet-5-5-medium` (plus 4 `gpt-6.1-sol` baseline runs) covering **10 distinct pedagogical approaches** (Round 1), **hybrid combinations** (Round 2), **generative-to-discriminative overclaim transfers** (Round 3), **paired action + justification & visible quote boundaries** (Round 4), **live course-site navigation** (Round 5), **instruction-hierarchy & permission boundaries** (Round 6), **course-prior Einstellung inversions & 4-step proactive interference** (Round 7), **6-step state-overwrite cascades with `XOR` in mental math** (Round 8 Vector 1), and **15 generalized anti-dogma & quantitative architecture problems** (Round 9):

| Experimental Track (Clean Self-Contained Text, Well-Posed Key) | Fresh Contexts | Valid Item Responses | Exact Key Matches | Accuracy |
| --- | ---: | ---: | ---: | ---: |
| Pilots `v1–v4` (case application, counterfactuals, trace tables) | 13 | 65 | 65 | **100.0%** |
| Round 1: 10 parallel methodological families (`A01–A10`) | 20 | 40 | 40 | **100.0%** |
| Round 2: 5 unexposed holdouts (`clean` baseline) + Hybrid `C01–C02` | 6 | 30 | 30 | **100.0%** |
| Round 3: Evidence-strength MCQ in two option orders (`A` and `B`) | 6 | 30 | 30 | **100.0%** |
| Round 4: Paired `Action + Justification` & visible quoted commands | 6 | 30 | 30 | **100.0%** |
| Round 5: Course-grounded navigation (`lessons.tellian.io`, valid `D1,D2,D3,D5`) | 3 | 12 | 12 | **100.0%** |
| Round 6: Literature-grounded authority hierarchy & permission boundaries | 9 | 45 | 45 | **100.0%** |
| Round 7: Einstellung course-rule inversion + 4-step proactive interference | 6 | 30 | 30 | **100.0%** |
| Round 8 (`V1`): 6-step state overwrite + `XOR` (both mental-only and with tools) | 4 | 20 | 20 | **100.0%** |
| Round 9 (`9A, 9B, 9C`): 15 generalized AI-architecture & math problems | 9 | 45 | 44 | **97.8%** |
| **Total across all clean, self-contained, well-posed closed runs** | **82** | **347** | **346** | **99.7%** |

**Why every classic "tricky wording" failed to lower the score:**
1. **Generative vs. Discriminative Gap (Round 2 $\to$ Round 3):** In Round 2, `claude-sonnet-5-5-medium` committed a systematic causal overclaim in its *free-text explanation* of `Q1` in 3 out of 3 runs (claiming a single test outcome *proved* the root cause). Yet when that exact overclaim was turned into a 4-option multiple-choice question in Round 3, the model scored **30 / 30 (100%)** across both option orders. Recognizing a hedged statement among four options is fundamentally easier than generating it from scratch.
2. **Externalized Scratchpad Defeats Einstellung & Proactive Interference (Rounds 7–8):** In cognitive literature ([Nature Scientific Reports 2025](https://doi.org/10.1038/s41598-025-22940-0); [TMLR Survey 2026 `arXiv:2602.06176`](https://arxiv.org/abs/2602.06176)), models fail on Einstellung traps and multi-step proactive interference when forced to answer in a single pass from parametric memory. When given chain-of-thought or source-reading tools, `claude-sonnet-5-5-medium` explicitly writes out the state table at each step (`Rev 0 -> Rev 6`) and solves 6-step conditional rollbacks with **100% accuracy** even without invoking Python.

---

### 2. Cross-model evidence boundary

The requested full benchmark — all 20 questions × 3 models × 3 independent runs — **was not completed**. Several workflow attempts failed or were cancelled, and the exact completed observations are listed in [`cross_model_evidence.md`](cross_model_evidence.md). The earlier draft's aggregate percentages for all 20 questions were extrapolations and are intentionally **not presented as measured results** here.

What is established:

- clean, well-posed pilot batteries for Sonnet were often near 100%;
- in the completed targeted runs, the same cascade question produced different failure modes for Sonnet, Sol and Flash;
- strategic reformulations changed Sonnet's answer on the tested Q1/Q12 variants;
- no defensible all-20 cross-model total should be quoted until the 180-cell benchmark finishes successfully.

This distinction is part of the result: an AI-evaluation report must not convert partial evidence into a complete model ranking.

---

### 3. Mechanical, Symbolic, and DOM/Clipboard Defenses: Complete Empirical Ranking

We tested 8 file-based Unicode obfuscations, 6 inline steganographic/tag injections, 5 aggressive symbol/script perturbations (`M1–M5`), and 3 platform-integrated DOM/CSS architectures (`R8, R11`):

| Defense Mechanism | Channel | Visual Quality for Student | Target Sonnet 5.5 Score | Why It Succeeded or Failed |
| --- | --- | --- | ---: | --- |
| **Homoglyphs (`а/a, е/e, о/o`) + Zero-Width Spaces (`U+200B × 2196`, `U+2060`)** | File & Inline | Clean | **100% (32/32)** | Tokenizer splits subwords, but transformer attention reconstructs Russian words effortlessly from context. |
| **Invisible Unicode Tags (`U+E0000..`) & Variation Selectors (`U+FE00..`)** | File & Inline (tail or mid-token `M3`) | Clean | **90.9% (10/11 runs = 100%, 1 run = 0%)** | Unreliable: Sonnet 5.5 has an explicit detector for `U+E0000` tags—in 10 of 11 total contexts across R1, R2, R10 it decoded the hidden ASCII note, quoted it, and refused to follow it. |
| **Targeted Bidi Number Reversal (`RLO U+202E / LRI U+2066`, `M1`)** | Inline Paste | Minor 1px shift | **100% (10/10)** | Sonnet 5.5 detects `U+202E RLO` in the character stream and mentally reverses `08 -> 80`, `853 -> 358` back to visual order. |
| **Multi-Script Confusable Cocktail (Cherokee `Ꭺ,Ᏼ,Ꮯ`, Coptic `ⲥ`, Math `⍺,ℯ,𝗈`, `U+034F`, Arabic/Greek punct `٫;։`, Fullwidth `０–９`, `M2 & M4`)** | Inline Paste | Visibly degraded ("ransom-note" glyph jumps) | **90.0% (18/20)** | Causes occasional 1-point slips (confusing Cherokee `Ᏼ` with Cyrillic `Б` in Group labels, or off-by-two arithmetic), but ruins human readability without reaching $\le 50\%$. |
| **Decoupled Lab Stand Card + Symmetric *Einstellung* Distractors (`R8_5Q`)** | Student copies text + AI reads `lessons.tellian.io` | **100% Clean** | **20.0% (1/5, twice)** | **Works reliably:** AI looks up the public course page, finds canonical seminar numbers, and picks the *Einstellung* distractor `A` on 4 of 5 items (while control with the stand card scores `5/5`). |
| **Pure-CSS `@font-face` Zero-Width `unicode-range` (`mx-zero`, `R11`)** | Browser `Ctrl+C` (zero JS edits) | **0.0000% pixel diff on screen** | **20.0% (1/5, twice)** | **Works reliably at the engine level**, but if the hidden string uses italic math/English characters, a student can spot it visually after pasting into the chat input box. |
| **Seamless Atomic Russian `oncopy` + `window.onblur` Screenshot Swap (`B3 Live Deploy`)** | Browser `Ctrl+C` & `Win+Shift+S` / `blur` | **100% Clean, continuous selection, 0 flicker** | **0.0% (0/5, twice)** | **Best production defense:** normal continuous mouse selection on screen; `Ctrl+C` and window defocus (`blur` / `PrintScreen`) swap 3–4 short standard Russian phrases/numbers into decoy values that map to wrong options. |
