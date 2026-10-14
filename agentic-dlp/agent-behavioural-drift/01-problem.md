# 01 — The Problem

## The question

Can one agent be told apart from another using only the fields a trace collector keeps
by default — and if so, can the same measurement tell an agent apart from its own
past self?

The second half is what detecting behavioural drift means. The first half has to hold
for the second to be possible.

## Why the default fields

Under the OpenTelemetry GenAI semantic conventions:

**On by default** — `gen_ai.conversation.id` (session identity), `execute_tool` spans
(tool name, call order, span timestamps), `gen_ai.response.finish_reasons`.

**Opt-In, flagged as likely to carry PII** — `gen_ai.input.messages`,
`gen_ai.output.messages`, `gen_ai.system_instructions`, `gen_ai.tool.definitions`.

So in a deployment that takes the defaults, content is not there. Any detector that
needs message text, tool arguments or return values is asking for data the operator
has deliberately not collected.

This is a **constraint argument, not a privacy argument** — see the framing note below.

## What "content-free" means here

Included: tool identity, order of calls within a session, timing gaps between calls.

Excluded: messages, tool arguments, tool results, recipients, recall keys, retrieved
document text — anything that would tell you *what* was recalled or *what* was sent.

## Lineage

The five-dataset re-identification work (CERT r4.2, HDFS, Google Cluster, Azure VM,
Alibaba GPU) established that ordered behaviour alone separates actors, with timing
raising it further. **[FROM DECK]** Figures quoted in the deck as ~50–90% for order and
~95–100% for order + timing; earlier materials said 16–89% and 94–100%. *These two sets
disagree and must be reconciled before presenting.* See `06-open-questions.md`.

Study 1 is the same measurement carried from human users in system logs to agents in
agent telemetry.

---

## Framings tried and rejected

Recorded so they don't come back by accident.

### "Identity leaks from telemetry the standard treats as safe"
**Rejected.** Frames it as a privacy harm, but agents are not data subjects. A reviewer
asks "leaks whose identity, to what harm?" and there is no answer. The OpenTelemetry
analysis survives in a different role — as a statement of what is actually available at
detection time, not as a claim about a hole in the spec's privacy model.

### "Order + timing re-identifies"
**Rejected as a headline.** It is true and already demonstrated by the five-dataset
work. Repeating it on agent data is not a contribution — "everyone knows it
re-identifies." Timing survives as an ablation inside each study, not as the claim.

### "Shared detectors fail, we fix them"
**Weakened twice, by evidence.** The false-positive wall is narrower than it first
looked. See `02-prior-art.md` — DriftNet reports 2.9% on hard negatives, NeuroTaint 8%
on cross-session poisoning. The wall stands only for **content-free, cross-session**
detection, which is a much more specific claim and must be stated that precisely.

### Model swap, shadow agents
**Out of scope.** Different threat model; LIDAR's territory.

### Instance-level identity
**Parked.** Telling two deployments of the same agent apart needs data neither available
corpus has.
