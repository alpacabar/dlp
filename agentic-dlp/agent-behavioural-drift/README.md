# Agent Behavioural Drift — Research Notes

Working notes for the research strand on detecting agent behavioural drift from
content-free tool-call telemetry. 

**Status:** pre-experiment. Direction settled, data audit not yet done.
**Last updated:** 2026-10-14

---

## The direction in thirty seconds

Two studies, run in order.

**Study 1 — Can agents be told apart from content-free telemetry?**
Tool names, call order and timestamps are what a trace collector keeps by default.
Message content is opt-in. Whether the default fields alone separate one agent from
another has not been measured. Runs today on existing public data; needs no attack data.

**Study 2 — Can an agent's own baseline catch what shared detectors miss?** *(gated)*
Every published detector compares all agents to one pattern, and honest agents that
legitimately consult memory before acting trip that pattern constantly. If each agent is
measured against its own history instead, those false alarms should fall away.

Study 2 only makes sense if Study 1 succeeds. If agents are not separable at all,
per-agent baselining has nothing to stand on and the work stops after weeks, not months.

---

## Files

| File | What's in it |
|---|---|
| [`01-problem.md`](01-problem.md) | The question, how the framing arrived there, framings rejected along the way |
| [`02-prior-art.md`](02-prior-art.md) | Every paper that matters, what each does, where each stops |
| [`03-positioning.md`](03-positioning.md) | The 2×2, what is novel and what is not, the taint-analysis connection |
| [`04-study-1.md`](04-study-1.md) | Distinguishability study: data, measure, controls, success criteria |
| [`05-study-2.md`](05-study-2.md) | Per-agent drift detector: design, gated on Study 1 |
| [`06-open-questions.md`](06-open-questions.md) | Stage 0 data audit, unverified claims, risks |
| [`figures/architecture.md`](figures/architecture.md) | System architecture, rendered ([Mermaid source](figures/architecture.mmd)) |
| [`reading-list.csv`](reading-list.csv) | Papers cited in these notes, with what still needs reading or verifying |

---

## How to read these notes

Claims are marked where they have not been checked:

- **[VERIFIED]** — read in the source paper or dataset
- **[FROM DECK]** — taken from the 261008-v2 slides, not independently re-checked
- **[UNVERIFIED]** — asserted somewhere but not confirmed; do not present without checking

Several numbers in circulation around this project came from AI-generated analyses and
have not survived checking. The marks exist because of that, and should be kept honest.

---

## Immediate next step

The Stage 0 data audit in [`06-open-questions.md`](06-open-questions.md). Four checks,
roughly a day of work. Any of them can change the plan, and the plan currently rests on
assumptions about data nobody has opened.
