# DLP Research

Collected notes, related work and references for Data Loss Prevention (DLP) research at S3Lab, Pusan National University.

## Research strands

| Strand | Where | Status |
| --- | --- | --- |
| Agent behavioural drift from content-free telemetry | [`agent-behavioural-drift/`](agent-behavioural-drift/) on `main` | Pre-experiment; Stage 0 data audit next |
| RAG for DLP — related work map | [`rag-for-dlp/`](https://github.com/alpacabar/dlp/tree/rag-for-dlp/rag-for-dlp) on the `rag-for-dlp` branch | Literature map, 2026-10-14 |

## Standards and policies

[`standards-policies/`](standards-policies/) — ISO/IEC 27001/27002 control 8.12, NIST SP 800-137, SP 800-53 Rev. 5 and CSF 2.0, Korean PIPA and ISMS-P, and how this project's components map onto them. Paraphrase with citations only, plus [`references.bib`](standards-policies/references.bib). Standard PDFs stay out of the repo (copyright).

## Competitor analysis

[`existing-tools/`](existing-tools/) — 7 Korean DLP vendors scored on a 60-item, six-dimension checklist with a focus on generative and agentic AI controls; international vendors in progress. Includes the full deck and the gaps no vendor covers (MCP mediation, agent action control, exact data matching, AI response inspection).

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
├── standards-policies/       # Notes 01–08 on ISO, NIST, CSF 2.0, PIPA/ISMS-P, project mapping; references.bib
├── existing-tools/           # Competitor analysis: domestic scores and CSVs, international (WIP), slides
├── papers/                   # One note per paper, from papers/_template.md
├── notes/                    # Meeting notes, ideas, drafts
└── reading-list.csv          # Master list of general DLP papers and their status
```

Each research strand keeps its own `reading-list.csv` beside its notes.

## Adding a paper

1. Copy `papers/_template.md` to `papers/<first-author><year>-<short-title>.md`.
2. Fill in the summary and the DLP relevance.
3. Add a row to the matching `reading-list.csv`.
