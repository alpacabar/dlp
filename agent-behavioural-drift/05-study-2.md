# 05 — Study 2: Can an agent's own baseline catch what shared detectors miss?

**Status:** gated on Study 1 succeeding, and on the data audit.

---

## Claim

Every published detector compares all agents to one pattern. Honest agents that
legitimately consult memory before acting trip that pattern constantly — that is Leong
v3's 24.7–57.6%. If each agent is measured against its own history instead, those false
alarms should fall away.

## Threat model

**In scope:** an attacker plants one rule in agent memory through normal input; sessions
later the agent recalls it and acts on it, for example by emailing data out. The trigger
session is free of attack content. The attacker may instruct the agent to "behave as
usual" (mimicry).

**Out of scope:** model swap, shadow agents.

**Assumed:** the attacker cannot edit or delete logs, and cannot change the model, tools
or framework.

## Architecture

See `figures/architecture.mmd`. Four groups:

1. **Agent runtime** — agent, memory store, tools
2. **Trace** — collector (OpenTelemetry spans) → content filter (drop messages,
   arguments, results)
3. **Detection, content-free** — feature extractor (tool order, timing gaps) →
   precondition filter (recall-before-send) → drift scorer (distance from baseline)
4. **Per-agent baseline** — clean sessions, fed from the precondition filter

Two design points worth keeping:

- The enrollment arrow leaves the **precondition filter**, not the feature extractor, so
  baseline sessions pass the same filter as candidates — matched population rather than
  all prior activity.
- The arrow is labelled **"enrollment window, assumed clean"** and drawn dashed. The
  pipeline does not determine cleanliness; the enrollment window asserts it. Making that
  assumption visible is better than hiding it.

## Measure

**Precision at 1% simulated attack prevalence, with recall matched to v3's 0.98.**
Matching recall is what makes the comparison fair — otherwise precision can be bought
by detecting less.

**Report separately by phenotype.** Agents that already have no false-positive problem
have nothing for the method to fix; pooling them in inflates the result.

Also report TPR at low FPR.

## The hard problem

If the payload-write session is S2 and the poison persists afterwards, **the clean
baseline is n = 1 per run.** One session is not a baseline.

Partial pooling / hierarchical shrinkage toward the cell mean then becomes **mandatory,
not a design choice**: each agent's baseline is shrunk toward the mean of its defence ×
model cell, with the shrinkage weight set by how little data that agent has.

This is why the audit question about which session carries the payload write matters so
much. It decides whether Study 2 is a straightforward per-agent model or a hierarchical
one.

## Data

v3's pre-trigger sessions, **if the release contains them** — potentially ~7,500 clean
sessions that v3 generated and discarded. Otherwise new collection: roughly 20 agents ×
30 sessions × 2 frameworks.

**Also check TaintBench** (the benchmark NeuroTaint evaluates on, with cross-session
positives). It may be better suited than anything in v3's release, and it comes with a
published baseline to compare against.

## Baselines to beat

| Baseline | What it is | Reported |
|---|---|---|
| Leong v3 | One shared signature, content-free, cross-session | 24.7–57.6% FP; ≤3.85% precision at 1% prevalence |
| NeuroTaint | Taint provenance, **needs content**, cross-session | ~8% FP, precision 0.921 |
| DriftNet | One shared learned model, **needs content**, single session | 2.9% FP on hard negatives |

NeuroTaint is the honest upper bound: it is what is achievable *with* content. The
argument is not that this work beats it, but that it approaches useful performance
without the data NeuroTaint requires. Say that plainly rather than hoping nobody
compares.
