# D. RAG for policy and regulatory compliance reasoning

This strand maps onto the policy layer of DLP: retrieving the relevant rule (GDPR article, internal policy) and reasoning over whether an action complies. None of these works apply it to live data-flow events.

| Work | What it does | Relevance |
| --- | --- | --- |
| [PrivComp-KG](https://www.arxiv.org/pdf/2404.19744) (Garza et al., 2024) | Embeds GDPR articles in a vector DB, uses RAG to map privacy-policy sections to articles, stores results in a knowledge graph | Retrieval correctness 0.9; found 40 GDPR articles missing from one retailer's policy. Template for policy-to-rule retrieval |
| [Teaching LLMs to reason about compliance](https://ceur-ws.org/Vol-3816/paper63.pdf) (CEUR 2024) | Structure-preserving RAG over legal texts plus generate–judge–refine examples | Chunking that respects legal structure matters for retrieval quality |
| [RAG chatbot for regulatory compliance](https://publica.fraunhofer.de/handle/publica/482992) (Fraunhofer IAIS, 2024) | Hybrid search + relevance boosting for risk and quality queries; deployed | Evaluated on 124 expert-annotated real queries; useful evaluation design |
