# DLP Research

Collected notes, related work and references for Data Loss Prevention (DLP) research at S3Lab, Pusan National University.

## Branches

| Branch | Purpose |
| --- | --- |
| `main` | General DLP material: standards and policies, existing tools, papers, notes |
| `rag-for-dlp` | Retrieval-augmented generation (RAG) as it relates to DLP: RAG as a detection technique, RAG pipelines as a leakage surface, defences, compliance reasoning |

Topic branches start from `main`. Merge material back into `main` once it is stable.

## Layout

```
.
├── standards-policies/   # Regulations, frameworks, policy templates (GDPR, PIPA, ISO 27001, NIST ...)
├── existing-tools/       # Commercial and open-source DLP tools, feature notes
├── papers/               # One note per paper, from papers/_template.md
├── notes/                # Meeting notes, ideas, drafts
└── reading-list.csv      # Master list of papers and their status
```

## Adding a paper

1. Copy `papers/_template.md` to `papers/<first-author><year>-<short-title>.md`.
2. Fill in the summary and the DLP relevance.
3. Add a row to `reading-list.csv`.
