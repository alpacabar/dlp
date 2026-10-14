# C. DLP-style defences and controls for RAG

The consistent finding is that model-side fixes (prompting, fine-tuning) are not enough; reliable controls act on what the model is allowed to see.

| Work | Family | Mechanism | Reported result / note |
| --- | --- | --- | --- |
| [Secure RAG with RBAC](https://sol.sbc.org.br/index.php/bracis/article/view/40906) (Martinelli & Ponti, BRACIS 2025) | Access control at retrieval | Filters retrieved documents by role before they reach the LLM | Prompting, model choice and fine-tuning all failed to stop leaks; only controlling the context did |
| [RAG classification and access control](https://www.tdcommons.org/dpubs_series/7382) (Google TD Commons, 2024) | Access control + classification | Records permissions at upload, scores prompt risk against data classification levels, exposes hidden permissions | Directly a DLP clearance model applied to RAG |
| [VAGUE-Gate](https://preview.aclanthology.org/credits/2025.ijcnlp-long.194) (IJCNLP-AACL 2025) | Local DP gate before generation | Plug-in local-DP rewriting of passages in front of any RAG | At ε = 0.3: 70% less lexical leakage, 91% of plain-RAG faithfulness, +240 ms |
| [DP-SynRAG](https://arxiv.org/html/2510.06719v1) (Mori et al., ACL Findings 2026) | DP synthetic knowledge base | Builds a reusable DP synthetic RAG database instead of adding noise per query | Fixed privacy budget; beats query-time DP RAG baselines |
| [DP token generation for RAG](https://arxiv.org/html/2412.19291v2) (2024) | DP at decoding | Differentially private token generation over retrieved documents | Shows DP decoding is viable for private RAG |
| [TextCrafter](https://arxiv.org/pdf/2509.17302v5) (2025) | Embedding perturbation | Adversarial, PII-guided perturbation of embeddings before release | Blocks Vec2Text inversion while keeping downstream utility |
| [Protecting Sensitive Data in RAG — guardrail talk](https://www.fh-salzburg.ac.at/fhs/aktuelles/veranstaltungen/2026/05/it-talk-protecting-sensitive-data-in-retrieval-augmented-generation-systems-for-large-language-models) (FH Salzburg, May 2026) | Guardrail | Guardrail against dynamic attacks and unintended exposure | Abstract only; worth tracking |

## Industry guidance

- Classify and redact at ingestion, carry lineage and authorisation metadata on every chunk, and place a mandatory policy check between retrieval and generation ([Petronella](https://petronellatech.com/blog/secure-rag-enterprise-architecture-patterns-for-accurate-leak-free-ai/); [Thales](https://cpl.thalesgroup.com/sites/default/files/content/solution_briefs/secure-rag-ai-data-protection-sb.pdf)).
- PII enters an LLM pipeline at four points: user input, retrieved context, tool results and model output ([TrueFoundry](https://www.truefoundry.com/ar/blog/pii-redaction-llm-gateway-vs-application)).
