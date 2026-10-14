# 01 — How data leaves through an agent

Five categories, ordered from the most studied to the least. Each lists the works that establish it. Verification marks follow the convention in [`agent-behavioural-drift/`](agent-behavioural-drift/): **[VERIFIED]** = checked against the paper's abstract or the primary source page; **[FROM DECK]** = carried over from the drift notes, not re-checked here.

---

## T1 — Injection-driven exfiltration

An attacker places instructions in content the agent will read (web page, email, document, tool output). The agent follows them and sends private data to the attacker, usually through a tool it legitimately holds.

| Work | What it shows | Mark |
|---|---|---|
| [Greshake et al., *Not what you've signed up for*](https://arxiv.org/abs/2302.12173) (2023) | Introduces **indirect prompt injection**: instructions hidden in retrieved data blur data and code. Taxonomy of impacts includes data theft; demonstrated on Bing Chat (GPT-4) | [VERIFIED] |
| [InjecAgent](https://arxiv.org/abs/2403.02691) (Zhan et al., 2024) | 1,054 cases, 17 user tools, 62 attacker tools. Two attack classes: direct harm and **exfiltration of private data** (typically emailed to the attacker). ReAct GPT-4 vulnerable 24% of the time; 47% with a reinforcing "hacking prompt" | [VERIFIED] |
| [EchoLeak, CVE-2025-32711](https://thehackernews.com/2025/06/zero-click-ai-vulnerability-exposes.html) (Aim Security, June 2025) | First widely reported **zero-click** agent exfiltration: a crafted email's hidden prompt is pulled into Microsoft 365 Copilot's RAG context with the user's data, which then leaks via Teams and SharePoint URLs. CVSS 9.3 | [VERIFIED] |

**DLP reading:** the exfiltration channel is a legitimate tool (email, URL fetch). Channel-blocking DLP sees an authorised action by an authorised identity.

## T2 — Tool and supply-chain poisoning

The malicious instruction lives in the tool itself — its description, metadata or a later update — rather than in data.

| Work | What it shows | Mark |
|---|---|---|
| [Invariant Labs, MCP tool poisoning](https://invariantlabs.ai/blog/mcp-security-notification) (1 Apr 2025) | Instructions hidden in MCP tool descriptions, invisible to users but read by the model. Demo: Cursor agent read and sent `~/.cursor/mcp.json` (other servers' credentials) and `~/.ssh/id_rsa`. Mitigations: show full descriptions, pin tool versions by hash, control data flow between MCP servers | [VERIFIED] |
| [OWASP ASI04 Agentic Supply Chain](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) (2026) | Tampered agent cards / registries redirecting data | [VERIFIED] (list) |

**DLP reading:** a "rug pull" (tool changes after approval) means one-time approval is not enough; the competitor gap "MCP / tool-call mediation" is exactly this.

## T3 — Memory and knowledge poisoning

Something planted earlier — in long-term memory or a retrieval store — fires in a later session. The triggering session can be free of attack content.

| Work | What it shows | Mark |
|---|---|---|
| [AgentPoison](https://arxiv.org/abs/2407.12784) (Chen et al., 2024) | Backdoor via poisoned memory / knowledge-base entries with an optimised trigger. ≥80% average attack success on three agents (driving, QA, EHRAgent) at <0.1% poisoning rate, ≤1% benign drop. No model training needed | [VERIFIED] |
| Leong, delayed memory poisoning (v2/v3, 2026) | Content-free detector over tool logs; v3 shows 24.7–57.6% false alarms on honest traffic. Details in [`agent-behavioural-drift/02-prior-art.md`](agent-behavioural-drift/02-prior-art.md) | [FROM DECK] |

**DLP reading:** the leak and its cause are sessions apart, so per-event inspection cannot connect them. Cross-session provenance or per-agent baselines are needed. RAG-store risks are mapped in detail on the `rag-for-dlp` branch.

## T4 — Contextual over-sharing (no attacker)

The agent discloses data it holds because it misjudges what is appropriate for the task or recipient. Framed as **contextual integrity** or **data minimisation**.

| Work | What it shows | Mark |
|---|---|---|
| [PrivacyLens](https://arxiv.org/abs/2409.00138) (Shao et al., NeurIPS 2024 D&B) | 493 privacy-sensitive seeds turned into vignettes and agent trajectories. In action, GPT-4 leaks in **25.68%** and Llama-3-70B in **38.69%** of cases, even with privacy-enhancing prompts | [VERIFIED] |
| [AgentDAM](https://arxiv.org/abs/2503.09780) (Zharmagambetov et al., Meta, 2025) | Web agents (GPT-4, Llama-3, Claude) use unnecessary sensitive data. Probing a model about privacy looks more optimistic than end-to-end testing; prompting defences help but cost task performance | [VERIFIED] |
| [AirGapAgent](https://arxiv.org/abs/2405.05175) (Bagdasarian et al., Google, 2024) | **Context hijacking** — a third party talks the agent into revealing task-irrelevant data. One query cut a Gemini Ultra agent's protection from 94% to 45% | [VERIFIED] |

**DLP reading:** this is closest to ISO 27002 8.12, which covers disclosure by systems as well as individuals, with no adversary in the loop. Injection defences do not address it.

## T5 — Multi-agent propagation

Data crosses agent boundaries through intermediate outputs, delegation or covert channels.

| Work | What it shows | Mark |
|---|---|---|
| [Open Challenges in Multi-Agent Security](https://arxiv.org/abs/2505.02077) (Schroeder de Witt et al.) | Intermediate outputs can carry sensitive information beyond intended scope **without any malicious actor**; steganographic channels between agents could hide exfiltration | [VERIFIED] |
| [OWASP ASI07 Insecure Inter-Agent Communication](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) (2026) | Interception and spoofing of agent-to-agent messages | [VERIFIED] (list) |

**DLP reading:** least mature category; a problem statement exists, measurement mostly does not.
