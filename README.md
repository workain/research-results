# workain/research-results

Public write-ups and the underlying data behind workain's agent/harness research —
the sanitized numbers and methodology notes behind our published articles, kept
reproducible and separate from the private working repos they come out of.

Each dataset lives in its own dated folder. A folder is added when the article it
backs ships; it is not updated retroactively except to fix an error.

## Datasets

| Folder | Backs the article | Scope |
| --- | --- | --- |
| [`2026-11_harness-agents/`](2026-11_harness-agents/) | "Харнес по частям", part 1 | 3 coding agents (Claude Code, Hermes, DeepSeek Harness) × 9 tasks × with/without a harness: tokens, rounds, cost, quality |
| [`2026-11_ai-resistant-assessment/`](2026-11_ai-resistant-assessment/) | AI-resistant assessment experiments | 138 runs across 11 rounds: clean-text reasoning, structural interference, Unicode/DOM clipboard channels, methodology, raw matrix and Russian blog draft |

## Related

- [`agent-runtime-instruments`](https://github.com/workain/agent-runtime-instruments) — the open instrumentation used to measure the numbers published here (Apache-2.0).
- [`agent-harness-registry`](https://github.com/workain/agent-harness-registry) — a taxonomy of harness components (memory, tools/skills, subagents, access/MCP, instructions), relevant to later entries in this series.

## License

Dataset content and write-ups (`*/`, this README): **CC BY 4.0** — copy and adapt with attribution.
Any scripts added later: MIT.
