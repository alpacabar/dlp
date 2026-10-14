# 06 — Open Questions

## Stage 0 — the data audit

Roughly a day of work. The whole plan rests on assumptions about data nobody has opened.
Any of these can change it.

- [ ] **Does v3's release contain raw session logs, or only aggregate results?**
      Decides whether Study 2 has data at all.
- [ ] **Are the S1–S3 pre-trigger sessions in the release?** v3 §2.2 retains them and
      discards them — potentially ~7,500 unused clean baseline sessions already public.
- [ ] **Which session index carries the payload write, and does the poison persist after
      it?** If S2 is the ingest session and the poison persists, the clean baseline is
      **n = 1 per run**, and partial pooling becomes mandatory.
- [ ] **Timestamp resolution — float or integer seconds?** Integer seconds voids every
      timing feature and the whole timing ablation.
- [ ] **Are token counts present?**
- [ ] **Task diversity in each corpus.** A uniform task distribution inflates separation
      and a reviewer will find it before you do.
- [ ] **Does v3's release include the 4,160 benign sessions across 13 models?** If so,
      that is probably a better Study 1 corpus than the 2,520 attack runs.

## Literature still to read

- [ ] **Lugoloobi et al. 2026** — passive model ID from browser UI traces, 96% F1.
      Closest thing to a scoop of Study 1. Must be read and distinguished explicitly.
- [ ] **MemLineage** — cross-session provenance, cited by APPA.
- [ ] **TaintBench** — possible Study 2 data with a published baseline.
- [ ] **Provenance-based IDS** — UNICORN, Marlin, ProvDetector, NoDoze, ATLAS, HOLMES,
      SIGL. **Key question: is per-entity baselining already established technique
      there?** If yes, "per-agent baseline" is novel as an application, not a technique.
- [ ] **FIDES** — NeuroTaint's baseline.

## Claims that need checking before presenting

Several numbers in circulation around this project came from AI-generated analyses and
did not survive checking. These are the ones still outstanding.

| Claim | Status | Note |
|---|---|---|
| Five-dataset figures: ~50–90% order, ~95–100% order+timing | **CONFLICT** | Earlier materials said 16–89% and 94–100%. Two decks disagree. Reconcile against the actual results. |
| "One case went unseen for 7 months" | **UNVERIFIED** | Check against the Anthropic report. |
| "Finding the rest meant scanning 141,006 runs" | **UNVERIFIED** | Same. |
| "Nobody noticed from behaviour" | **UNVERIFIED** | This is a claim about *detection method*. The report may not support it. |
| Lugoloobi et al., 96% F1 | **UNVERIFIED** | From a deck footnote. |
| arXiv IDs for Leong / AgentDrift / DriftNet / LIDAR | **FROM DECK** | Re-check each. |
| "Microsecond timestamps" | **WITHDRAWN** | Resolution was never checked. Do not repeat. |
| Model names attached to the phenotype split | **WITHDRAWN** | Did not match the models in v3. |

**Rule of thumb that keeps proving itself:** every gap that looked open in this project
has been wrong at least once. Verify on arXiv full-text search and Semantic Scholar
citation graphs before building on any "nobody has done this."

## Deck fixes — 261008-v2

- [ ] Add a **Study 1 method slide** between 12 and 13. Currently Study 1 has no method
      slide and Study 2 has two.
- [ ] Label slides 13–14 **"Study 2"** so the gate on slide 12 stays visible.
- [ ] Fill the **Study 2 box on slide 12** — it has two sentences and white space.
- [ ] Move **Lugoloobi** out of the slide-11 footnote and into the roadmap, with the
      distinction stated.
- [ ] Slide 4: "slide 4" self-reference should read **slide 3**.
- [ ] Slide 3: reconcile the **re-identification percentages**.
- [ ] Slide 3: verify the **Anthropic incident claims** before presenting.
- [ ] Slide 7: title **collides with the header band**.
- [ ] Slide 9: soften the **"closed set of known models"** criticism of LIDAR, or be
      ready to explain why it does not apply to Study 1.
- [ ] Slides 11 and 12 still say **"normalised timing"** in the proposal sentence —
      decide whether timing is in the headline or is an ablation. It cannot be both.
- [ ] Slide 2: **"Behavioral"** appears under both Identifier and Quasi-Identifier, and
      the colour legend is inherited from the DLP framing. Does it still earn slide 2?
- [ ] **Add a one-slide summary at the front**: the question, why nobody answered it,
      what happens first. Thirty seconds of reading. Everything else becomes backup.

## Risks

**Study 1 separates for boring reasons.** Different models, frameworks and task mixes
differ regardless. Defence: hard control inside a cell. See `04-study-1.md`.

**Study 1 alone is thin.** It is a measurement; its value is licensing Study 2. Plan for
a workshop paper or a long section.

**The field moves faster than the work.** Four relevant papers in Sept 2026, two taint
papers earlier that were missed until October. Not fixable, only outrun — an argument
for running Study 1 now, since it needs no new collection.

**The motivation keeps eroding.** The false-positive wall has been narrowed twice
(DriftNet 2.9%, NeuroTaint 8%). It now holds only for content-free cross-session
detection. State it at exactly that scope; a broader claim is false and checkable.
