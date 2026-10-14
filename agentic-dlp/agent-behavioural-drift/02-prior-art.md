# 02 — Prior Art

arXiv IDs below are **[FROM DECK]** unless marked otherwise — taken from the 261008-v2
slides. Re-check each against arXiv before citing in a paper.

---

## The three-layer frame

The organising idea: each line of work guards a different integrity layer of the same
artifact, the agent's tool-call log.

| Layer | Work | Threat | Telemetry used | Baseline | Where it stops |
|---|---|---|---|---|---|
| **Memory** | Leong v3 | Delayed memory poisoning, fires sessions later | Passive structure: tool names, order, timestamps | One global rule (recall → send) | Honest recall looks identical: 24.7–57.6% false alarms |
| **Execution** | DriftNet / AgentDrift | Prompt injection inside one session | Passive semantics: embedded step text + arguments | One shared Transformer for all agents | Single session; no memory recall in the benchmark |
| **Identity** | LIDAR | Provider silently swaps the model | Active: 6 probe tasks it issues itself | Enrolled reference per model | Cannot watch live everyday traffic |

---

## Leong — delayed memory poisoning
`arXiv:2606.30566` v2 (May 2026) → v3 (Sept 2026)

**v2** built a detector from tool logs only (names, order, timestamps; no content),
19 trace features including recall-before-send and pre-send gap, random 5-fold CV over
2,520 attack runs. AUC 0.990.

**v3** re-tested it properly and **self-falsified**:
- Grouped CV split by model × defence cell → AUC 0.907
- Honest traffic added: 13 models, 4,160 benign sessions that legitimately use memory
- Retrieval visibility separated: tool-routed recall vs implicit RAG
- **24.7–57.6% false alarms on honest traffic; ≤3.85% precision at a 1% attack rate**

v3 states that identifying a configuration that works is open work, not a contribution
there — an explicit invitation.

**What this leaves:** the recall-before-send trace is an *attack precondition*, not a
test of intent. Every logged attack shows it; so do honest agents. A shared rule cannot
separate them.

**Why it matters for Study 2:** v3's failure is evidence the content-free cross-session
cell is *hard*, not that it is empty. Standing on a documented failure is a stronger
position than standing on an unexplored gap.

---

## AgentDrift + DriftNet — execution integrity
AgentDrift `arXiv:2609.06972`, DriftNet `arXiv:2609.10892` (Pinjari & Saint-Germain, Sept 2026)

**AgentDrift** — benchmark of 12,536 trajectories, 3–11 steps each, one session,
step-level labels: Clean / Hijacked / Resisted / Hard negative (risky text, safe
behaviour). No timestamps — **order only**.

**DriftNet** — all-mpnet-base-v2 text encoder + 4 domain flags → Transformer <2M params,
two heads (session verdict, step labels).

Results: trajectory F1 0.983; injection step found 98.7%; delayed hijacks 93.2%;
**0% false alarms on resisted attacks, 2.9% on hard negatives.**

**Limits:** prompt injection within one session only, no cross-session memory poisoning;
needs step text and arguments; no timing, step index only; one shared model for all
agents, so it cannot tell agents apart.

**Note on the 2.9%:** this partly undercuts a broad "false positives are unsolved"
claim. It survives because DriftNet's hard negatives are *single-session, in-domain
lookalikes*, not memory-grounded cross-session recall. State the scope precisely or the
motivation collapses.

---

## LIDAR — identity integrity
Wang et al., `arXiv:2609.28559` (Sept 2026) — "Who Is Behind the Harness?"

Probes → fingerprint → match. Six paired probe tasks (VERIFY / RECOVER / RESOLVE),
behaviour features, enrolled reference per model, query scoring against a frozen panel.

Results: 95.1% top-1 (mini-swe-agent), 88.4% (OpenCode) across 36 models; 36 probe runs
to enroll each model.

**Limits, and this is the gap Study 1 occupies:**
- **Active** — issues its own probe tasks; no passive mode over everyday logs
- No timing signal: discrete actions and test outcomes only
- Closed panel of 36 known models; no instance-level identification
- "Identity drift" only motivates re-running probes; no baseline over time

LIDAR names passive identification from ambient traffic as open in its own future work.

**Caution:** Study 1 as scoped also runs on a closed set (whatever models are in the
corpora). Do not criticise LIDAR for a limitation shared with the proposed work.

---

## The taint-analysis line — found 2026-10-14, previously missed

The recall-before-send invariant is structurally source → sink reachability. That
connection is already an active subfield.

### NeuroTaint — "Ghost in the Agent" **[VERIFIED]**
`arXiv:2604.23374` (Apr 2026) · https://arxiv.org/html/2604.23374v1

Provenance-oriented offline auditor tracing untrusted sources to privileged sinks.
Covers explicit content propagation, implicit control influence, asynchronous provenance
reuse. Dynamic Context Provenance Graph stores taint labels with memory writes,
**persists across sessions**, rehydrates lineage on retrieval.

- **28 of 30 cross-session / cross-agent positives detected**
- **16 false positives in 200 non-propagating scenarios ≈ 8%**
- Precision 0.921, recall 0.935; zero FP for the canary tier
- Baseline compared against: FIDES, 106 false positives

**Requires content.** Audits full execution logs — user prompts, reasoning traces, tool
arguments, return values, memory operations. Explicit detection compares source text
against sink arguments (canary, LCS, embedding matching); implicit detection re-queries
the model with the source replaced by a neutral placeholder. **Does not evaluate tool
names and order alone.** No per-agent baselining; fixed global thresholds.

**Impact on this project:** 8% on cross-session memory poisoning is the second hit to
the false-positive motivation, and a direct one. Any claim that cross-session poisoning
cannot be detected with low false positives is now false. The surviving claim is that it
cannot be detected **without content**.

### APPA — taint confinement **[VERIFIED]**
`arXiv:2607.24625` (Jul 2026) · https://arxiv.org/html/2607.24625v1

Information-flow control for LLM agents. Label-based dynamic taint tracking confined to
child branches to avoid "label creep" (reading restricted data locking the agent out of
downstream tools). Pre-acquisition checks produce remedy plans. Attack success 31–50% →
0–7% across four models.

**Core enforcement needs no content inspection** — checks labels, declared tool
contracts and an event log at the tool-dispatch layer, arguing content guardrails are
bypassable by paraphrasing. **But single-run only**: labels and event log cover one run.
Names **MemLineage** as a cross-session provenance complement.

### Also mentioned, not yet read
- **MemLineage** — cross-session provenance, cited by APPA. **Look this up.**
- **TaintBench** — benchmark NeuroTaint evaluates on, includes cross-session positives.
  **Possible Study 2 data.**
- **FIDES** — baseline in NeuroTaint.
- **Lugoloobi et al. 2026** — passive model ID from browser UI traces, 96% F1
  **[UNVERIFIED, from deck footnote]**. Closest thing to a scoop of Study 1. Must be
  read and distinguished explicitly, not left in a footnote.

---

## Provenance-based intrusion detection — adjacent, not yet searched

System-provenance IDS does structure-without-content anomaly detection on syscall
graphs. Agent tool-call traces are provenance graphs. The alert-fatigue problem these
systems attack is the same shape as the false-positive problem here.

To check: **UNICORN** (https://arxiv.org/pdf/2001.01525), **Marlin**
(https://arxiv.org/pdf/2403.12541), ProvDetector, NoDoze, ATLAS, HOLMES, SIGL, CamFlow,
and the 2026 industrial evaluation (https://www.alphaxiv.org/abs/2603.22982).

**The question to answer:** is per-entity (per-host) baselining already established
technique in this literature? If so, "per-agent baseline" is novel as an *application to
agents*, not as a *technique* — and the paper must say so before a reviewer does.
Prof. Choi is a security professor and may well know this literature.
