# 06 — Trajectory and operation-level detection

As of 2026-10-29. Covers detectors that watch **what an agent did** — its sequence of tool calls, or the graph of its run — rather than inspecting a single prompt or response. This is the family the [proposed drift idea](agent-behavioural-drift/) belongs to. It sits at control point ⑤ in the [README](README.md) diagram.

The works are grouped by **what the detector reads**, the split proposed for the drift idea:

- **Content-free** — tool names, call order, timing, categorical fields (status, role, tool type), graph structure. Nothing that says *what* was recalled or sent.
- **Content-based** — also reads arguments, results or step text, usually through a sentence encoder or an LLM.

**Marks:** [VERIFIED] = checked against the paper's abstract (through a conference page or paper mirror; arXiv itself was not reachable from this session). [PARTIAL] = paper confirmed, but some claims from the source list could not be checked. [NOT FOUND] = no paper located. *Corrections* below lists where the source list did not match the papers.

---

## The finding that matters most

**Shared content-free detectors have so far hit a ceiling.** MCPShield reports that metadata-only detection **plateaus around AUROC 0.64 whatever the architecture**, and that adding content embeddings lifts it above 0.89. Leong's tool-order signature catches attacks, but honest memory-grounded sends produce the same signature. Every content-free detector below is a **single model shared across agents**. None measures an agent against its own history — the gap the drift idea targets. That makes the gap real, but also makes MCPShield's plateau the result the drift idea most needs to answer.

---

## Content-free detectors

| Work | What it reads | Method | Key result | Mark |
|---|---|---|---|---|
| [Forensic Trajectory Signatures for Agent Memory Poisoning Detection](https://arxiv.org/abs/2606.30566) (Leong, 2026; **cite v3**) | Tool names, order, timestamps | `recall_before_send` rule; Random Forest over trajectory features | Original result: AUC 0.9904 (Random Forest), 0.9563 (rule). Latest version: 24.7–57.6% benign false positives across 13 models (N = 4,160); precision ≤3.85% at 1% attack prevalence. The signature is "an attack precondition, not a maliciousness predicate" | [VERIFIED] via abstract summaries — see *Corrections* on versions |
| [TraceAegis](https://arxiv.org/abs/2510.11203) (Oct 2025) | Agent execution traces: execution order and semantic consistency of the sequence | Learns "stable execution units" into a hierarchy, then behavioural rules; validates traces against both | Abstract gives no numbers. TraceAegis-Bench: healthcare and procurement, 1,300 benign + 300 abnormal behaviours per scenario | [PARTIAL] |
| [Skynet](https://arxiv.org/abs/2609.06835) (Virginia Tech & WashU, MobiHoc '26) | Workflow graph: actor role, action category, tool/endpoint, status, edge type, structural descriptors | Directed workflow graph per run; semantic + structural encoders trained with contrastive learning on **benign runs only**; score = distance to nearest of 150 benign-mode centroids | 100% recall at ~0.89% FPR on Agent-SafetyBench and AgentErrorBench; 82.7% on ATBench. Weakest on indirect prompt injection, which looks like benign workflows. It is unclear whether the semantic branch reads free text | [VERIFIED] (body) |

## Content-based detectors

| Work | What it reads | Method | Key result | Mark |
|---|---|---|---|---|
| [DriftNet](https://arxiv.org/abs/2609.10892) (Pinjari & Saint-Germain, Sep 2026) | Step text via a **frozen sentence encoder**, plus four world features | Dual-head Transformer, <2M params: one head per trajectory, one per step (benign / injection point / hijacked / failed injection). No access to the agent's model | Trajectory F1 0.983; exact injection point 98.7%; 0 of 218 resisted attacks flagged; 2.9% on hard negatives; 93.2% of delayed executions (surface baseline: 17.1%) | [VERIFIED] |
| [MCPShield / Content-Aware Attack Detection in LLM Agent Tool-Call Traffic](https://arxiv.org/abs/2605.11053) (Zavrak, May 2026) | Tool name, serialised arguments, tool response, response length, parameter hash; SBERT embeddings | Session graph (tool calls as nodes, sequential and data-flow edges); GAT, GCN, GraphSAGE vs MLP and tree ensembles | **Metadata-only plateaus at AUROC ≈ 0.64**; content pushes it above 0.89; best 0.975 (tree ensembles on pooled SBERT). **Random splits inflate AUROC by up to 26 points** against task-disjoint splits | [VERIFIED] |
| [SentinelAgent](https://arxiv.org/abs/2505.24201) (May 2025) | Multi-agent interactions as dynamic execution graphs | Node, edge and path-level anomaly detection; an **LLM-powered oversight agent** observes and intervenes | Two case studies (email assistant, Magentic-One) with root-cause attribution; no figures in the abstract | [VERIFIED] |

## Benchmark

| Work | Contents | Mark |
|---|---|---|
| [AgentDrift: A Step-Labeled Benchmark of Injection-Hijacked LLM Agent Trajectories](https://arxiv.org/abs/2609.06972) (Sep 2026) | 12,536 trajectories, 71,024 labelled steps; benign, successful, resisted and hard-negative cases. The data behind DriftNet and the planned Study 1 corpus. **Name clash:** a different paper, [AgentDrift: Unsafe Recommendation Drift Under Tool Corruption](https://arxiv.org/abs/2603.12564), shares the name — cite by arXiv ID | [VERIFIED] |

## Which layer to defend: Leong's companion study

[Injection–Execution Dissociation: A Mechanistic Evaluation of Persistent Memory Attacks and Defences in Stateful LLM Agents](https://arxiv.org/abs/2605.08442) (Leong, 2026; v6, Oct 2026). The source list calls it *Defense Effectiveness Across Architectural Layers*. [VERIFIED]

- Six defences across four layers (input, retrieval, instruction, tool); 5,040 runs on nine open-weight models, plus 410 frontier runs on 29 endpoints.
- **Only Memory Sandbox — removing the recall tool — stops the attack**: 0% ASR on eight of nine models.
- On one model (qwq:32b) ASR went from 0% to 100% under the sandbox, attributed to the model **falling back to RAG** to rebuild the instruction. That run did not reproduce after a fresh boot, so the paper treats it as an illustration. The RAG-fallback bypass itself was reproduced on three Bedrock-hosted models.
- Input-layer classifiers (Llama Guard, ShieldGemma) are **argued, not tested**, to miss the attack because they only see the user's message and the payload arrives through retrieval.

**Why it matters:** blocking one memory path pushes the attack onto another. A detector that watches one tool transition (recall → send) inherits the same blind spot when the agent switches to RAG.

---

## Relevance to the proposed drift idea

1. **MCPShield's 0.64 plateau is the result to answer.** It holds for shared models across architectures. The drift idea's claim is that a per-agent baseline changes this; the plateau is the bar it has to beat, and the paper should cite it rather than wait for a reviewer to.
2. **Skynet is the closest design.** It is one-class, trained on benign runs only, and scores distance to benign-mode centroids. The difference is that its centroids are behaviour modes shared across agents, not one baseline per agent. A reviewer will ask how the two differ; that answer belongs in `03-positioning.md`.
3. **Evaluation hygiene has independent support.** MCPShield's 26-point inflation from random splits backs the grouped-split control in [`04-study-1.md`](agent-behavioural-drift/04-study-1.md), alongside Leong's AUC drop.
4. **RAG fallback is a threat to the recall-before-send precondition.** If an agent can rebuild the instruction from its retrieval store, the explicit recall tool call never appears.

---

## Corrections to the source list

- **Leong (2606.30566) — the "correction" points to an older version.** v2 reported 24.7–52.6% false positives (N = 4,360) and said gating with recipient metadata **restores** separation. The latest version reports 24.7–57.6% (N = 4,160), precision ≤3.85%, and that a recipient-metadata gate **flags 129 of 129 legitimate external emails**. So the recipient-gating claim does not survive into the latest version. The drift notes already use the latest numbers; cite that version. Source: abstract summaries on [Pith](https://pith.science/paper/2606.30566) and [awesomepapers](https://awesomepapers.io/ai-agents/papers/2606.30566); the PDF could not be opened from this session, so confirm the 129/129 figure before citing it.
- **Leong companion (2605.08442):** the title is *Injection–Execution Dissociation*. The 0% → 100% inversion did not reproduce. Llama Guard and ShieldGemma were not tested.
- **TraceAegis:** "F1 above 0.94", "collected at the gateway, without prompts or reasoning" and "needs labelled per-scenario templates" are not in the abstract or the review I could read. Unchecked.
- **Skynet:** authors are from Virginia Tech and WashU, not WashU alone.
- **SentinelAgent:** an "attack-path library" is not in the abstract.
- **MCPShield:** v2 retitled the paper *Content-Aware Attack Detection in LLM Agent Tool-Call Traffic: An Empirical Study of Features, Architectures, and Evaluation Protocols*. Its related-work table and the two OpenTelemetry papers it cites could not be confirmed.

## Still to find

- **Trajectory Guard** — [NOT FOUND]. Cited only through MCPShield.
- **"Silent Failures"** — possible match: [When Passing Tests Hides Vulnerabilities: An Empirical Study of Silent Failures in Agentic Systems](https://arxiv.org/abs/2609.10548). Not confirmed to be the XGBoost-on-16-features paper.
- **OpenTelemetry temporal-attack-pattern paper (66.7% benign FPR)** — [NOT FOUND]. Worth tracing from MCPShield's references: it is the closest to the drift idea's OpenTelemetry framing.
