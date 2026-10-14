# DLP Research

Collected notes, related work and references for Data Loss Prevention (DLP) research at S3Lab, Pusan National University.

## Research strands

| Strand | Where | Status |
| --- | --- | --- |
| Agent behavioural drift from content-free telemetry | [`agent-behavioural-drift/`](agent-behavioural-drift/) on `main` | Pre-experiment; Stage 0 data audit next |
| RAG for DLP — related work map | [`rag-for-dlp/`](https://github.com/alpacabar/dlp/tree/rag-for-dlp/rag-for-dlp) on the `rag-for-dlp` branch | Literature map, 2026-10-14 |

## Branches

| Branch | Purpose |
| --- | --- |
| `main` | General DLP material and settled research strands |
| `rag-for-dlp` | Retrieval-augmented generation (RAG) as it relates to DLP: RAG as a detection technique, RAG pipelines as a leakage surface, defences, compliance reasoning |

Topic branches start from `main`. Merge material back into `main` once it is stable.

## Layout

```
.
├── agent-behavioural-drift/  # Research strand: notes 01–06, figures, reading list
├── standards-policies/       # Regulations, frameworks, policy templates (GDPR, PIPA, ISO 27001, NIST ...)
├── existing-tools/           # Commercial and open-source DLP tools, feature notes
├── papers/                   # One note per paper, from papers/_template.md
├── notes/                    # Meeting notes, ideas, drafts
└── reading-list.csv          # Master list of general DLP papers and their status
```

Each research strand keeps its own `reading-list.csv` beside its notes.

## Adding a paper

1. Copy `papers/_template.md` to `papers/<first-author><year>-<short-title>.md`.
2. Fill in the summary and the DLP relevance.
3. Add a row to the matching `reading-list.csv`.
