# 02 — Benchmarks for agent data leakage

Which benchmark to use depends on whether the leak needs an attacker. Most security benchmarks assume one (T1–T3); the privacy benchmarks do not (T4).

| Benchmark | Threat | Size | What it measures | Leakage-relevant result | Mark |
|---|---|---|---|---|---|
| [AgentDojo](https://arxiv.org/abs/2406.13352) (Debenedetti et al., NeurIPS 2024 D&B) | T1 | 97 tasks, 629 security test cases (email, e-banking, travel) | Utility and security under prompt injection, dynamic and extensible | Standard testbed for defences: CaMeL, FIDES and Progent all report on it | [VERIFIED] |
| [InjecAgent](https://arxiv.org/abs/2403.02691) (Zhan et al., 2024) | T1 | 1,054 cases, 17 user + 62 attacker tools | Indirect injection, split into direct harm vs **data stealing** | GPT-4 ReAct: 24% → 47% with hacking prompt | [VERIFIED] |
| [PrivacyLens](https://arxiv.org/abs/2409.00138) (Shao et al., NeurIPS 2024 D&B) | T4 | 493 seeds → vignettes + trajectories; 1,479 probing questions | Privacy-norm awareness **in action**, not just in Q&A | GPT-4 leaks 25.68%, Llama-3-70B 38.69% | [VERIFIED] |
| [AgentDAM](https://arxiv.org/abs/2503.09780) (Zharmagambetov et al., 2025) | T4 | Web-navigation tasks | Data minimisation: is sensitive data used only when necessary? | Probing overstates privacy vs end-to-end runs | [VERIFIED] |
| AgentDrift (Pinjari & Saint-Germain, 2026) | T1, single session | 12,536 trajectories, step labels incl. hard negatives | Execution drift under injection; order only, no timestamps | Study 1 corpus — see [`agent-behavioural-drift/04-study-1.md`](agent-behavioural-drift/04-study-1.md) | [FROM DECK] |
| Agent Security Bench (ASB) | T1–T2 | — | Used alongside AgentDojo by Progent | Not yet read | to-read |
| TaintBench | T1, T3 cross-session | — | Benchmark NeuroTaint evaluates on, with cross-session positives | Possible Study 2 data | to-read |

## Choosing for our work

- **Detection of exfiltration attacks** → AgentDojo (defence comparability) + InjecAgent (data-stealing split).
- **"Agent shares too much" without an attacker** → PrivacyLens or AgentDAM. Neither is used in the DLP-product literature yet.
- **Content-free / telemetry-only detection** → AgentDrift and Leong v3 traces (drift strand). None of the benchmarks above strip content, so a content-free variant would have to be derived.
