# 04 — Study 1: Can agents be told apart from content-free telemetry?

**Status:** unblocked. No new collection, no attack data required.
**Blocked only by:** the Stage 0 audit in `06-open-questions.md`.

---

## Claim under test

Tool identity, call order and timing gaps — the fields an OpenTelemetry collector keeps
by default — are sufficient to separate one agent from another, without probe tasks and
without reading any content.

## Why it is not already answered

LIDAR separates models at 95.1%, but **actively**: six designed probe tasks per check,
reading execution content, against a closed panel of 36. Passive separation from ambient
traffic with content stripped is named as open in LIDAR's own future work.

**Caveat to resolve first:** Lugoloobi et al. 2026 reportedly achieves passive model
identification at 96% F1 from browser UI traces **[UNVERIFIED]**. Read it and establish
the distinction — different signal source, different deployment assumption, no OTel field
constraint, whichever is actually true — before building on this gap.

## Data

Two independently generated corpora, which is worth more than either alone.

| Corpus | Size | Signal | Notes |
|---|---|---|---|
| **AgentDrift** | 12,536 trajectories | Order only, no timestamps | 3–11 steps; step-level labels available but not needed here |
| **Leong v3** | 2,520 sessions | Order + timing | Also 4,160 benign sessions across 13 models — *see audit* |

The absence of timestamps in AgentDrift is a feature. If separation holds there on
ordering alone, timing is not load-bearing — which is exactly the result wanted, given
that leading with timing makes the work look like a repeat of the five-dataset result.

**Audit first:** v3's 4,160 benign sessions across 13 models may be a better Study 1
corpus than the 2,520 attack runs. If those are in the release, they are honest traffic
across many models, which is precisely what this study needs.

## Measure

**Separation ratio** = D_between / D_within — mean distance between agents over mean
distance within an agent. Unbounded; want > 1.

> **Terminology collision:** the CERT work used `separation_ratio = 0.9530`, which reads
> bounded [0,1]. This is a different quantity. Rename one of them in writing, or the
> confusion is guaranteed.

**Against a permutation null.** Shuffle agent labels, recompute, repeat; report the
Z-score of the observed ratio against that distribution. Without a null, any positive
number looks like a result.

## Controls — the part that decides whether this is believable

The failure mode is not "no separation." It is separation for uninteresting reasons.

1. **Fix the defence cell.** Compare only within one defence configuration. Across
   cells, differences in defence behaviour masquerade as agent differences.
2. **Stratify by behavioural phenotype.** Ground-then-act vs act-without-grounding.
   Agents in different phenotypes are trivially separable and will inflate the ratio.
3. **Stratify by attack outcome** where labels exist (clean / hijacked / resisted).
4. **Report sequence length on its own line.** Length alone separates agents. If the
   ratio tracks length, that is the finding and it is a boring one.
5. **Watch bigram sparsity.** On 3–12 step sessions, tool bigrams are sparse; a
   representation that overfits sparse counts will look impressive and generalise badly.
6. **Grouped splits.** StratifiedGroupKFold with the agent as the group — v3's AUC fell
   0.990 → 0.907 precisely because random CV leaked.

## Ablations

- Order only vs order + timing (timing is an ablation here, never the headline)
- Tool identity only vs identity + position
- Session length held constant vs free

## Success criteria — write these down before running anything

- Separation ratio significantly above the permutation null **within a fixed defence
  cell and a single phenotype**
- Not explained by sequence length
- Holds on both corpora, or fails on one with a stated reason

## Either way

**Separates** → the premise Study 2 rests on is established rather than assumed, and the
measurement is the first published one of its kind.

**Does not separate** → a negative result worth reporting, and Study 2 closes before it
costs anything. This is the whole reason for running the studies in this order.

## What is missing from the deck

The 261008-v2 deck reaches the proposal on slide 12 and then gives two slides to Study 2
(threat model, architecture) and **none to Study 1's method**. Add a Study 1 method slide
between 12 and 13, and label 13–14 as Study 2, or the deck reads as though Study 1 has
already been skipped.
