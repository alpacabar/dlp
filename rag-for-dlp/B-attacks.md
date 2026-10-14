# B. The RAG pipeline as a data-loss surface (attacks)

A RAG system leaks through three channels: the generated answer (extraction), the answer's behaviour (membership inference) and the stored vectors (embedding inversion). Each is a DLP exfiltration path that classic content inspection does not watch.

| Work | Channel | Finding | DLP takeaway |
| --- | --- | --- | --- |
| [The Good and The Bad](https://arxiv.org/abs/2402.16893) (Zeng et al., 2024) | Output: retrieval-DB extraction | Crafted queries extract private records (e.g. medical dialogues) from the retrieval store; RAG also reduces leakage of the LLM's training data | Foundational threat model. A distance threshold on retrieval lowers leakage but hurts utility |
| [Follow My Instruction and Spill the Beans](https://arxiv.org/abs/2402.17840) (Qi et al., ICLR 2025) | Output: prompt-injected extraction | Instruction-tuned LMs repeat retrieved context verbatim; leakage hit 25 of 25 custom GPTs within 2 queries, and gets worse with model size | Output-side DLP must catch verbatim regurgitation of context |
| [Is My Data in Your Retrieval Database?](https://arxiv.org/abs/2405.20446) (Anderson et al., IBM, 2024) | Behaviour: membership inference | A single crafted prompt reveals whether a document is in the store, in black- and gray-box settings | Even a "yes/no" leaks; proposes a template-instruction defence |
| [Riddle Me This! Interrogation Attack](https://huggingface.co/papers/2502.00306) (2025) | Behaviour: stealthy membership inference | Natural-language questions infer membership; 2x TPR@1%FPR over prior attacks, under $0.02 per document | Query-pattern detectors and query rewriting miss natural queries |
| [Vec2Text — Text Embeddings Reveal (Almost) As Much As Text](https://arxiv.org/pdf/2310.06816) (Morris et al., 2023) | Vectors: embedding inversion | Iterative correction reconstructs text from its embedding | Vectors of sensitive text are sensitive data |
| [ALGEN](https://arxiv.org/pdf/2502.11308) (2025) | Vectors: few-shot inversion | Aligns victim embeddings to an attacker space with few samples; tested defences failed | Covers API abuse, compromised agents and insider misconfiguration of the vector DB |
| [Zero2Text](https://arxiv.org/html/2602.01757) (2026) | Vectors: training-free inversion | Cross-domain inversion without training a decoder | Inversion keeps getting cheaper |
| [Inversion attacks on commercial embedding models](https://escholarship.org/uc/item/92k8b7p9) (UC Irvine thesis) | Vectors: commercial APIs | text-embedding-3-small was the most invertible model tested; 47.4% of medical queries recovered exactly | Vendor embeddings are not a privacy boundary |
