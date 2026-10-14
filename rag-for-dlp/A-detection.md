# A. RAG and retrieval as a DLP detection technique

Retrieval helps DLP where regex and NER fail: context-dependent sensitivity, rare PII types and document categories. The closest direct precedent is PRvL, which tests RAG-guided PII redaction and finds it raises recall but costs precision.

| Work | Type | What it does | Relevance to DLP |
| --- | --- | --- | --- |
| [PRvL — LLMs for PII redaction](https://arxiv.org/abs/2508.05545) (Garza et al., 2025) | Paper + open models | Compares zero-shot, fine-tuned and instruction-tuned LLMs, with plain decoding vs RAG that retrieves top-k annotated redaction exemplars | Only systematic study of RAG for redaction. RAG gave a small recall gain, lower precision, over-masking drift and slight leakage |
| [Cato Networks LLM-based DLP](https://www.catonetworks.com/blog/how-cato-uses-large-language-models-to-improve-data-loss-prevention/) (2024) | Industry system | Embeds documents with a contrastively trained Sentence-BERT, then classifies by k-NN within a fixed radius of a labelled support set | Retrieval-as-classification for document categories (tax forms, medical records, patents) that patterns miss |
| [TorchSight](https://arxiv.org/pdf/2605.20368) (2026) | Paper + open system | Fine-tuned local LLM for security document classification | Argues cloud LLMs defeat DLP's purpose; motivates local models. Not RAG, but a strong baseline |
| [Channel-boosted multi-agent sensitivity classifier](https://arxiv.org/pdf/2609.22212) (2026) | Paper | Encoder-based multi-agent system + post-hoc XAI for long-document sensitivity labels | Targets truncation loss and explainability, two gaps a retrieval design could also address |
| [NerGuard](https://pypi.org/project/nerguard/) | Open-source tool | Pre-ingestion PII redaction for RAG; uncertain spans routed to an LLM (entropy-gated) | Hybrid NER + LLM design close to a Presidio-plus-LLM stack |
| [SANS: LLMs as sensitive data detectors](https://www.sans.edu/cyber-research/your-sensitive-data-has-left-llms-sensitive-data-detectors) (2026) | Practitioner paper | Evaluates large and small LMs for contextual sensitive-data classification at the AI interface | Evidence for LLM detection in GenAI DLP |
| [DLP with distilled LLMs](https://image-ppubs.uspto.gov/dirsearch-public/print/downloadPdf/12711255) (US patent) | Patent | Sensitive/non-sensitive gate, then category and sub-category classification using LLM embeddings + ML classifiers | Shows the industry pattern: embeddings + classifier, compressed for latency |
| [Locale-conditioned few-shot PII substitution](https://arxiv.org/abs/2605.13538) (2026) | Paper | Rotates retrieved few-shot demonstrations for on-device PII substitution | Demonstration regurgitation is itself a leakage risk |
