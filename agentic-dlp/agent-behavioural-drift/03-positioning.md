# 03 — Positioning

## The 2×2

Detection of agent misbehaviour, organised by what the detector needs and how far it
reaches:

|  | **Single run** | **Cross-session** |
|---|---|---|
| **Needs content** | DriftNet — 2.9% FP on hard negatives | NeuroTaint — 8% FP, precision 0.921 |
| **Content-free** | APPA — label-based, tool-dispatch layer | **Leong v3 — 24.7–57.6% FP** ← here |

The bottom-right cell is not empty. It contains one documented failure. That is a better
place to stand than an unexplored gap, because the failure is evidence the cell is hard
rather than merely unvisited.

## The motivation, stated correctly

**Wrong:** "Nobody can detect cross-session memory poisoning without unacceptable false
positives." NeuroTaint disproves this at 8%.

**Right:** Cross-session poisoning *can* be detected well — with content. Under the
OpenTelemetry defaults, content is not collected. The question is what remains possible
when the only available fields are tool identity, order and timing.

This is an **availability** argument, not a difficulty argument, and it is what finally
makes the OpenTelemetry analysis load-bearing. It is about what a detector can actually
see at runtime, not about a flaw in the spec.

## The taint-analysis connection

### Where it matches
The recall-before-send invariant is source → sink reachability. Poisoned memory is the
source, an egress tool (`send_email`) is the sink. The precondition filter is a coarse
taint rule. This gives vocabulary a security supervisor already has, and it names the
failure mode precisely.

### Where it breaks 
Taint analysis tracks **data**. This work tracks **order**. There is no propagation:
nothing establishes that the item recalled is the item sent, only that a recall preceded
a send. A systems reviewer will say, correctly, that this is not taint tracking but
reachability on the tool-call graph. Do not claim otherwise.

### What the connection explains
**Leong v3 is taint analysis with maximum over-approximation.** Every recall → send path
is tainted because nothing distinguishes paths carrying real influence. Over-tainting is
the classic failure mode of taint analysis, and the standard remedies — sanitizers,
declassification policies, precise propagation — all require content.

So the standard fix is unavailable by construction.

### The contribution in one sentence

> When you cannot track the data, track the actor.

Replace data-flow precision with a per-agent prior on whether this flow is normal *for
this agent*. Different mechanism, same goal.

## What is novel, and what is not

**Probably novel**
- Passive, content-free separation of agents from ambient tool-call telemetry
  — *conditional on Lugoloobi et al. being genuinely different*
- Per-agent baselining applied to agent drift detection
- The content-free × cross-session cell, approached with a per-agent prior

**Not novel, do not claim**
- That ordered behaviour re-identifies actors (own prior work, and widely known)
- Taint-style source → sink reasoning for agents (NeuroTaint, APPA)
- Cross-session poisoning detection as such (NeuroTaint)
- Possibly per-entity baselining as a *technique* — pending the provenance-IDS search

**Honest risk**
Study 1 alone is a measurement, not a weighty contribution. Its value is that it
licenses Study 2. Plan for a workshop paper or a long section; if it turns out stronger,
good.

## Two things that will be attacked

**1. Separation for boring reasons.** Different models, frameworks and task mixes will
differ whether or not anything interesting is happening. A reviewer says "of course."
The defence is controlling hard inside a cell — which is why the stratification in
`04-study-1.md` matters more than it looks.

**2. The field is moving fast.** Four directly relevant papers appeared in Sept 2026
alone, plus two taint papers earlier in the year that were missed until October. Not
fixable, only outrun. It is an argument for doing Study 1 now, since it needs no new
data collection.
