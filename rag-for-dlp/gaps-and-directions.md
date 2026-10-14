# Gaps and possible directions for our DLP pipeline

No published work uses RAG as the decision engine of an enterprise DLP system over live data-flow events. Our stack (Wazuh rules, Presidio, YARA, dual-direction agent guardrail) sits where strands A, C and D could meet.

1. **Retrieval-adjudicated detection.** Presidio and YARA hits retrieve labelled exemplars (true leaks vs placeholders, test data, instructional text) before a local LLM decides. Targets the false positives TorchSight describes; watch the precision drop PRvL reports.
2. **Policy-grounded alerts.** Retrieve the policy or regulation clause behind each detection (PrivComp-KG style) so the Reporting page shows why an event is a violation.
3. **Correlating content with events.** Retrieve similar past Wazuh events and user history alongside the content hit. No work found combines SIEM context with content inspection through retrieval.
4. **Protecting our own knowledge base.** Exemplar stores of real leaks are themselves sensitive: Vec2Text-class inversion means local embedding models, access control on the vector DB, or perturbation (TextCrafter).
5. **Guardrail evaluation suite.** Use extraction, membership-inference and inversion attacks from strand B as test cases for the dual-direction guardrail; add verbatim-regurgitation checks on output.
6. **Benchmark gap.** No shared benchmark exists for RAG-based DLP on enterprise flows; a leakage-controlled dataset would be a contribution on its own.

**Open question:** frame RAG as the detection method (1–3) or the RAG pipeline as the protected asset (4–5)?
