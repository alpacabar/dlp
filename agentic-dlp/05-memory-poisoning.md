# 05 — Memory poisoning (T3 in depth)

As of 2026-10-29. Expands T3 in [`01-threats.md`](01-threats.md): something written into an agent's long-term memory in one session fires in a later one. This is also the threat model of Study 2 in [`agent-behavioural-drift/`](agent-behavioural-drift/05-study-2.md).

The field moved quickly in 2026. Attacks no longer need write access to memory; they get in through **content the agent reads**. On the defence side, a 2026 result argues that **gates which judge content cannot be made sound**, which shifts attention to provenance fixed at write time.

**Marks:** [VERIFIED] = checked against the paper's abstract (read through the conference page or a paper mirror; arXiv itself was not reachable from this session). [TITLE ONLY] = the paper exists, but its claims were not checked. See *Corrections* below for where the source list did not match the papers.

---

## Attacks

Ordered by how the attacker gets into memory, from most to least access.

| Work | Entry route | Cross-session | Target | Key result | Mark |
|---|---|---|---|---|---|
| [AgentPoison](https://arxiv.org/abs/2407.12784) (Chen et al., NeurIPS 2024) | Direct: poisoned entries placed in memory / knowledge base | Yes (trigger-activated) | Driving, QA, EHR agents | ≥80% attack success at <0.1% poisoning | [VERIFIED] |
| [MINJA](https://arxiv.org/abs/2503.03704) (Dong et al., NeurIPS 2025) | **Query-only**: attacker just talks to the agent; bridging steps + a progressively shortened indication prompt | Yes, for later victim queries | Diverse agents | No direct memory access needed. Abstract gives no success rates | [VERIFIED] |
| [MemoryGraft](https://arxiv.org/abs/2512.16962) (Dec 2025) | **Content the agent reads**: benign-looking artifacts make the agent store fake "successful experiences" | Yes | MetaGPT DataInterpreter, GPT-4o | A few poisoned records dominate retrieval on benign tasks; persistent behavioural drift. [Code](https://github.com/Jacobhhy/Agent-Memory-Poisoning) | [VERIFIED] |
| [Poison Once, Exploit Forever — eTAMP](https://arxiv.org/abs/2604.02623) (Zou et al., Apr 2026) | **Content the agent reads**: a manipulated web page or comment | Yes, and **cross-site**: poisoned on site A, fires on site B | Web agents, (Visual)WebArena | ASR up to 32.5% (GPT-5-mini), 23.4% (GPT-5.2); up to 8× higher when the agent is "frustrated" by dropped clicks or garbled text. Stronger models are not safer | [VERIFIED] |
| [Trojan Hippo](https://arxiv.org/abs/2605.01970) (Das et al., May 2026) | **Content the agent reads**: one untrusted tool call, e.g. a crafted email | Yes, dormant until the user discusses finance, health or identity | Frontier models from OpenAI and Google | **Exfiltrates personal data**; 85–100% ASR; still fires after 100 benign sessions | [VERIFIED] |
| [Hidden in Memory — Sleeper Memory Poisoning](https://arxiv.org/abs/2605.15338) (Pulipaka et al., ICML 2026 workshop) | **Content the agent reads**: document, web page or repo makes the assistant store a fabricated memory about the user | Yes, dormant then resurfaces | Stateful assistants: GPT-5.5, Kimi-K2.6 | Memory writes up to 99.8% (GPT-5.5) / 95% (Kimi); 60–89% of successful injections lead to the attacker's intended action. [Code](https://github.com/ivaxi0s/LLM-agent-memory-poisoning) | [VERIFIED] |
| [When Agents Remember Too Much — GhostWriter](https://arxiv.org/abs/2607.06595) (Jul 2026) | **Content the agent reads**: emails and calendar invites | Yes (two-phase) | A-Mem, Mem0, ExpeL, Letta, MemoryOS on four LLMs | ~98% injection, ~60% average activation | [VERIFIED] |

**Pattern:** five of the seven need nothing more than getting the agent to read something. For DLP this matters because the planting step looks like ordinary work (reading an email), and the leak happens sessions later.

## Systematisation and benchmark

| Work | What it gives | Key finding | Mark |
|---|---|---|---|
| [From Untrusted Input to Trusted Memory](https://arxiv.org/abs/2606.04329) (Dash et al., ICML 2026 workshop) | Taxonomy of **six** memory-poisoning attack classes; **MPBench** | Agents that write and retrieve memory more aggressively are easier to exploit; prompt-injection defences do not cover memory poisoning | [VERIFIED] |

## Defences

The "Reads" column says what each defence has to inspect. It uses the content / metadata / telemetry split discussed for the proposed drift idea.

| Work | Mechanism | Reads | Key result | Mark |
|---|---|---|---|---|
| [Memory Poisoning Attack and Defense on Memory Based LLM-Agents](https://arxiv.org/abs/2601.05504) (Jan 2026) | Composite trust scoring on input/output; trust-aware retrieval with temporal decay and pattern filtering. Tested against MINJA on EHR agents (MIMIC-III) | Content + time | Pre-existing legitimate memories already weaken MINJA. Sanitisation depends on threshold calibration: too strict blocks everything, too lax misses subtle attacks | [VERIFIED] |
| [A-MemGuard](https://arxiv.org/abs/2510.02373) (Wei et al., Sep 2025) | Consensus across reasoning paths built from related memories; detected failures stored as separate "lessons" | Content (reasoning) | ASR cut by over 95% across benchmarks at minimal utility cost (abstract gives no exact figures) | [VERIFIED] |
| AM-Sentry (in GhostWriter, Jul 2026) | Memory-saving policy plus a screen on retrieval | Content | Greatly reduces GhostWriter while keeping utility | [VERIFIED] |
| Trojan Hippo's four defences (May 2026) | User-prompt-only indexing; **no memory writes after an inbox-reading tool call**; cap memory entries at 80 characters; IFC labels at write time that block sends from tainted sessions | Role labels; **tool sequence only**; length; write-time labels | ASR down to 0–5%; IFC reaches 0% but breaks workflows that need to send | [VERIFIED] |
| [TMA-NM — Non-Malleable, Origin-Bound Authority](https://arxiv.org/abs/2606.24322) (Louck, Jun 2026) | Origin bound at write time by a trusted channel; checked with TLA+/TLC | Write-time origin labels | **Any gate deciding on content, or on lineage derivable from content, can be laundered** (self-summarisation, trusted-tool echo, fake corroboration): trust scoring laundered 68%, lineage 47%. TMA-NM blocks MemoryGraft, Trojan Hippo and two other published attacks (0/1,152) at 95.9% utility. Proof is over a bounded model and assumes origin labels are correct | [VERIFIED] |
| [When Does Belief-Based Agent Memory Help?](https://arxiv.org/abs/2606.22030) (Jun 2026) | Reliability-conditional updating; provenance-capped poisoning defence (from title) | Provenance | Not checked | [TITLE ONLY] |

**Pattern:** defences are converging on **provenance fixed at write time** (TMA-NM, Trojan Hippo's IFC). Content screening (A-MemGuard, AM-Sentry, trust scoring) is what TMA-NM's result says can be laundered.

## Relevance to the proposed drift idea

1. **A content-free control already exists.** Trojan Hippo's "no-untrusted-write" rule blocks memory writes after an inbox-reading tool call. It looks only at tool order — the same kind of signal as Leong's recall-before-send precondition. It is prevention by a fixed rule, not detection against a per-agent baseline, and it is the closest published comparison point. Its utility cost should be read in the full paper.
2. **TMA-NM's impossibility result does not cover telemetry-only detection.** It is about gates that read content or content-derived lineage; a detector that reads only tool names, order and timing is outside it. That does not make the drift idea sound, and a detector offers no guarantee. A reviewer may still ask whether laundering through self-summarisation makes a poisoned recall → send look normal.
3. **The Study 2 threat model now has verified attacks to cite and replay.** Sleeper, Trojan Hippo and eTAMP all have a trigger session free of attack content, which is the assumption in [`05-study-2.md`](agent-behavioural-drift/05-study-2.md). Trojan Hippo and MemoryGraft have code or reproductions (TMA-NM re-ran both as drop-in attacks).
4. **Dormancy and the clean-enrollment assumption.** Trojan Hippo still fired after 100 benign sessions, so an enrollment window can contain planted-but-dormant memory. Whether that contaminates a baseline depends on whether dormant poison changes tool-call behaviour — worth adding to the open questions.
5. **Candidate data for Study 2.** MPBench, alongside TaintBench, already listed. Apply the same Stage 0 checks: raw traces? per-session tool calls? timestamp resolution?

## Corrections to the source list

The reading list these entries came from did not match the papers in places:

- **Sleeper Memory Poisoning:** the abstract names **GPT-5.5 and Kimi-K2.6**, not "ChatGPT, Claude and Gemini". The claim that prompt-based mitigations were inconsistent is not in the abstract; unchecked.
- **eTAMP:** the paper's title is *Poison Once, Exploit Forever*. Cross-session, cross-site and no direct memory access are confirmed; the claim that it is the *first* such attack is not checked.
- **MemoryGraft:** "single-shot" is not in the abstract, which says a small number of poisoned records. Authors not obtained.
- **GhostWriter:** the list's only source was a news summary. The figures are confirmed from the abstract.
- **Belief-based memory:** title only. The claims about content-inferred versus provenance-derived trust are unchecked.

## Also found, not yet checked

Surfaced while verifying the list above: [MemSecBench — Tracking Agent Memory Poisoning from Persistence to Consequence and Repair](https://arxiv.org/abs/2607.27080) · [Memory Provenance Laundering in LLM Agents: A Non-Amplification Firewall](https://arxiv.org/abs/2607.29167) · [Utility Under Attack: Agent Memory Poisoning and the Limits of Content Screening and Provenance Ranking](https://arxiv.org/abs/2608.21230) · [An Empirical Study of Memory Poisoning Defenses for LLM Agents](https://icml.cc/virtual/2026/poster/61006) (ICML 2026) · MemMorph (one of TMA-NM's reproduced attacks).
