# 03 — Defences, by where they enforce

Numbers ①–⑤ match the diagram in the [README](README.md). The further left a control sits, the more it prevents; the further right, the more it can see without trusting the model.

| # | Enforcement point | Work | Mechanism | Needs content? | Key result | Mark |
|---|---|---|---|---|---|---|
| ① | Before context | [AirGapAgent](https://arxiv.org/abs/2405.05175) (Google, 2024) | A minimiser sees only the user task and data and passes on the relevant subset; the conversational model never holds the rest. Escalation step for extra disclosures | Yes | Under context hijacking: 97% protection vs 45% undefended | [VERIFIED] |
| ② | Planner (IFC) | [CaMeL](https://arxiv.org/abs/2503.18813) (Google DeepMind / ETH, 2025) | Control and data flow taken from the trusted user query; custom interpreter attaches capabilities to values and checks policies at each tool call | Yes | AgentDojo: 77% tasks solved **with provable security** vs 84% undefended | [VERIFIED] |
| ② | Planner (IFC) | [FIDES](https://arxiv.org/abs/2505.23643) (Microsoft, 2025) | Confidentiality and integrity labels on data, deterministic policy enforcement, primitives to hide and reveal information selectively | Yes | Completes a broad range of AgentDojo tasks with security guarantees | [VERIFIED] |
| ③ | Tool-call gate | [Progent](https://arxiv.org/abs/2504.11703) (Shi et al., 2025) | Symbolic allow/block rules over tool names and arguments; LLM drafts the policy, SMT solver auto-applies narrowings and requires approval for expansions | Arguments | Lowers attack success on AgentDojo and ASB while keeping utility; works in LangChain and OpenAI Agents SDK | [VERIFIED] |
| ③ | Tool-call gate | [Towards Verifiably Safe Tool Use](https://alphaxiv.org/abs/2601.08012) (Doshi et al., Jan 2026) | STPA hazard analysis → formal data-flow specs; MCP extended so tools declare capability, confidentiality and trust labels; external policy engine with block/must/allow/confirm tiers | Labels | Formal (Alloy) model: leak scenarios found without policy, proven absent with it. No empirical deployment study | [VERIFIED] |
| ④ | Inside the tool | [MTGuard — Hybrid Analysis for Secure MCP Tool Use](https://www.alphaxiv.org/abs/2607.25297) (He et al., Jul 2026) | Each MCP tool runs in a Docker sandbox monitored with eBPF; observed process/file/network behaviour compared with the declared call | Behaviour, not payload | 48.3% of 99 unsafe calls caught at 3.7% FPR (baselines 8.3%, 0%); 75% of host-side hijacks reading sensitive files | [VERIFIED] |
| ⑤ | After the fact (taint audit) | NeuroTaint (2026) | Cross-session provenance graph; traces untrusted sources to privileged sinks | Yes | 28/30 cross-session positives; ~8% FP | [VERIFIED] in drift notes |
| ⑤ | After the fact (single run) | APPA (2026) | Label-based taint confined to child branches at the tool-dispatch layer | No (labels) | Attack success 31–50% → 0–7% | [VERIFIED] in drift notes |
| ⑤ | After the fact (telemetry) | Leong v3; DriftNet; **our drift strand** | Tool order / timing (Leong), step text (DriftNet), per-agent baseline (ours) | Leong & ours: no | See [`agent-behavioural-drift/03-positioning.md`](agent-behavioural-drift/03-positioning.md) | [FROM DECK] |

## Patterns across the table

- **Deterministic beats probabilistic for guarantees.** CaMeL, FIDES and Progent enforce policy outside the model; prompt-based defences (AgentDAM's privacy prompt) help but give no guarantee.
- **Guarantees cost utility.** CaMeL gives up 7 points of task success on AgentDojo for provable security.
- **Almost every strong defence needs content or declared labels.** MTGuard (behaviour) and the drift strand (telemetry) are the exceptions, and both have far lower detection rates — the price of not reading payloads.
- **No defence covers T5 (multi-agent).**

## Also to read

[ROPE — Routed Origin Policy Enforcement against Indirect Prompt Injection](https://arxiv.org/pdf/2608.27496) (2026) · [Indirect Prompt Injections: Are Firewalls All You Need, or Stronger Benchmarks?](https://arxiv.org/pdf/2510.05244) (2025) · MELON · LlamaFirewall · Spotlighting · MemLineage (cross-session provenance, cited by APPA).
