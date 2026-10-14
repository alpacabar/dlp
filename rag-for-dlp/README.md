# RAG for DLP

Related work on retrieval-augmented generation (RAG) and Data Loss Prevention. As of 2026-10-14.

RAG meets DLP in two opposite ways: retrieval can make DLP detection smarter, and a RAG pipeline is itself a new channel for data loss. Most published work sits on the second side; very little uses RAG as the engine of a DLP system.

| Strand | File |
| --- | --- |
| A. RAG and retrieval as a DLP detection technique | [A-detection.md](A-detection.md) |
| B. The RAG pipeline as a data-loss surface (attacks) | [B-attacks.md](B-attacks.md) |
| C. DLP-style defences and controls for RAG | [C-defences.md](C-defences.md) |
| D. RAG for policy and regulatory compliance reasoning | [D-compliance.md](D-compliance.md) |
| Pipeline-stage map: threat and control per stage | [pipeline-map.md](pipeline-map.md) |
| Gaps and possible directions for our pipeline | [gaps-and-directions.md](gaps-and-directions.md) |
| All papers in one list | [reading-list.csv](reading-list.csv) |

Add a paper: write a note in `../papers/` from the template, add a row to the strand file and to `reading-list.csv`.
