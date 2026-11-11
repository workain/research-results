# Methodology notes

## 1. Experimental Hygiene and Pre-Registration Protocol

Evaluating whether an assessment item is "AI-resistant" is notoriously prone to experimenter self-deception: if an author tweaks answer keys after seeing the model's output, counts API/transport errors as zero points, or mistakes a defective/ambiguous question for a "model failure," the reported accuracy drop is an artifact.

Every round in this dataset enforced five rules:

1. **Pre-Target SHA-256 Freeze:** For every experimental batch (`parallel10_round1` through `parallel10_round11_b3_ui`), the exact student-facing markdown packet, the structured answer key (`key.json`), and the workflow input payload were written to disk and hashed with SHA-256 (`freeze.json` with `before_target: true`) *before* any target solver context was spawned. Post-run scoring scripts (`scores.json`) assert byte-for-byte hash equality against `freeze.json`.
2. **Automated Benchmark Auditing (ABA) Preflight:** Following [Wang et al. (2026), *Automated Benchmark Auditing for AI Agents and Large Language Models* (`arXiv:2605.26079`)](https://arxiv.org/abs/2605.26079)—which found that over 25.7% of tasks in frontier benchmarks contain implicit assumptions or defective ground truths—we introduced a two-agent preflight before freezing:
   - **Key-Aware Option-by-Option Auditor:** Independently evaluates the boolean truth value (`True`/`False`) of each option `A, B, C, D` against the problem statement and primary course snapshots (`sem-08.txt`, `sem-09.txt`, `sem-10.txt`), rejecting any item where a distractor is accidentally true.
   - **Blind Readability Editor:** Reads only the student packet (without keys) to flag ambiguous wording or unstated assumptions.
3. **Uniform Exclusion of Defective Items (No Post-Hoc Key Editing):** In Round 5 (`parallel10_round5_source`), all three fresh `claude-sonnet-5-5-medium` contexts answered `ABCD` on multi-select item `D4`, whereas our pre-frozen key was `ABC`. Inspection of [`sem-09.txt:321`]() confirmed that the model was right and our key was defective (option `D` stated that passing five formal regex rules does not prove semantic correctness, which is explicitly affirmed in Seminar 9). Rather than counting `4/5` as a model error or silently changing the key, `D4` was flagged as an invalid item and excluded uniformly across all runs (`4/4` valid score).
4. **Separation of Three Distinct Constructs:**
   - **Semantic / Cognitive Accuracy:** Does the model solve the underlying engineering problem when given the true problem parameters?
   - **Channel / Prompt-Injection Steering:** When an invisible or DOM-level modifier alters the copied parameters or injects an override note, does the model follow the altered parameters? (A `0/5` score under parameter substitution `M5` proves that the *copy-paste defense* works—not that the model lacks domain knowledge).
   - **Transport / Modality Capability Failure:** When an API endpoint rejects `read_image` because vision input is not declared on that route (`Round 3 visual preflight`), or when a child agent starts in a directory without the target file (`Round 10 file-read run`), the run is recorded as `unreadable / transport failure`, never as `0 points`.
5. **No Artificial Handicapping of Target Solvers:** In all core rounds, the target solver (`b3-chat/claude-sonnet-5-5-medium`) ran with full reasoning capability, access to standard analysis tools, and permission to fetch live course pages at `https://lessons.tellian.io`. In Round 8 (`Vector 1`), we additionally ran an explicit ablation comparing `chat_no_code` (pure mental reasoning, no Python/bash allowed) against `agent_with_tools`—both scored `5/5` (`100%`).

---

## 2. Verified Literature Base (2021–2026)

Each experimental round operationalized specific mechanisms from peer-reviewed and preprint research on LLM reasoning failures, calibration, and adversarial NLP:

1. **Imperceptible Encoding Perturbations & Unicode Tags:**
   - Boucher, Shumailov, Anderson, Papernot — *Bad Characters: Imperceptible NLP Attacks* ([IEEE S&P / `arXiv:2106.09898`, 2021](https://arxiv.org/abs/2106.09898)).
   - Rehberger — *Hiding and Finding Text with Unicode Tags (ASCII Smuggler)* ([Embrace The Red, 2024](https://embracethered.com/blog/posts/2024/hiding-and-finding-text-with-unicode-tags/)).
   - *Our empirical finding on frontier models (2026):* Classical homoglyphs, zero-width spaces (`U+200B`, `U+2060`), Bidi number reversals (`U+202E`), and `U+E0000` Tag injections that broke 2021–2024 systems are now largely neutralized in `claude-sonnet-5-5-medium`, which explicitly detects and reports `U+202E` and `U+E0000` sequences in its reasoning trace.
2. **Instruction Hierarchy & Indirect Prompt Injection in Agents:**
   - Wallace, Xiao, Leike, Weng, Heidecke, Beutel — *The Instruction Hierarchy: Training LLMs to Prioritize Privileged Instructions* ([`arXiv:2404.13208`, 2024](https://arxiv.org/abs/2404.13208)).
   - Zhan, Liang, Ying, Kang — *InjecAgent: Benchmarking Indirect Prompt Injections in Tool-Integrated LLM Agents* ([ACL Findings 2024 / `arXiv:2403.02691`](https://arxiv.org/abs/2403.02691)).
   - Debenedetti, Zhang, Balunović, Beurer-Kellner, Fischer, Tramèr — *AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents* ([`arXiv:2406.13352v3`, Nov 2024](https://arxiv.org/abs/2406.13352)).
3. **Einstellung Effect (Cognitive Fixation on Learned Patterns):**
   - *Limitations of large language models in clinical problem-solving arising from inflexible reasoning (mARC-QA)* ([Nature Portfolio / *Scientific Reports*, 2025, `doi:10.1038/s41598-025-22940-0`](https://doi.org/10.1038/s41598-025-22940-0)).
   - Yamin, Ghosal, Wilder — *Can LLMs Reconcile Knowledge Conflicts in Counterfactual Reasoning* ([ICML Workshop / `arXiv:2506.15732v4`, Feb 2026](https://arxiv.org/abs/2506.15732)).
4. **Proactive Interference in Working Memory & Inhibitory Control:**
   - Song, Han, Goodman — *Large Language Model Reasoning Failures* ([TMLR Survey, `arXiv:2602.06176`, Feb 2026](https://arxiv.org/abs/2602.06176)).
   - Xie — *Learning to Forget: Sleep-Inspired Memory Consolidation for Resolving Proactive Interference in LLMs* ([`arXiv:2603.14517`, Mar 2026](https://arxiv.org/abs/2603.14517)).
5. **Spurious Safety/Alignment Shortcuts & MCQ Distractor Calibration:**
   - Liu et al. — *DeShortcut-Align: Decoupling Spurious Shortcuts for Robust Safety Alignment in Large Reasoning Models* ([`arXiv:2609.34896`, Sep 2026](https://arxiv.org/abs/2609.34896)).
   - Steinmann et al. — *Navigating Shortcuts, Spurious Correlations, and Confounders* ([`arXiv:2412.05152`, Dec 2024](https://arxiv.org/abs/2412.05152)).
   - Chhikara — *Mind the Confidence Gap: Overconfidence, Calibration, and Distractor Effects in LLMs* ([TMLR, `arXiv:2502.11028v3`, Dec 2025](https://arxiv.org/abs/2502.11028)).
   - *Can LLMs Model Incorrect Student Reasoning? A Case Study on Distractor Generation for Multiple-Choice Questions* ([Findings of EMNLP 2026, `doi:10.18653/2026.findings-emnlp.2382`](https://doi.org/10.18653/2026.findings-emnlp.2382)).
6. **Blind Grading & Adaptive Overfitting in Benchmarks:**
   - Scarfe et al. — *A real-world test of artificial intelligence infiltration of a university examinations system* ([*PLOS ONE*, 2024, `doi:10.1371/journal.pone.0305354`](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0305354)).
   - Mirzadeh et al. — *GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models* ([ICLR 2025 / `arXiv:2410.05229`](https://arxiv.org/abs/2410.05229)).

---

## 3. What This Dataset Does and Does Not Claim

- **Repeated contexts vs. independent items:** Running 6 fresh contexts on a 5-question packet produces 30 item responses across 6 independent trials of the same 5 questions—not 30 independent questions. Both the number of unique question versions (`60+` across all rounds) and the number of fresh model contexts (`138` runs) are reported explicitly.
- **No human student pilot yet:** Clarity and readability were audited by blind LLM editors and instructor review, not yet by a randomized student cohort.
- **Conditionality on agent capabilities:** In Round 8 (`R8-5Q-text-and-png-card`), we discovered that while `read_image` was disabled on the model route, an autonomous coding agent given unrestricted `bash` access bypassed the missing vision tool by writing a Python script to decompress the PNG `IDAT` stream (`zlib`) and bit-match `DejaVuSansMono.ttf` glyph rasters (in Repeat 1) or installing `rapidocr` into `/tmp` (in Repeat 2). By contrast, a standard browser student copying text (`Ctrl+C`) or taking a window-defocusing screenshot (`Win+Shift+S` under `window.onblur`) hits the client-side DOM/clipboard swap directly.
