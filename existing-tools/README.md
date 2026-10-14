# Existing DLP tools — competitor analysis

Competitor analysis of data loss prevention (DLP) products, done to position our DLP solution against what is already on the market, with a focus on controls for generative and agentic AI.

| Part | Status |
|---|---|
| [Domestic (Korean) vendors](domestic/) | ✅ Complete: 7 vendors scored on a 60-item checklist |
| [International vendors](international/) | 🚧 In progress: vendor list and preliminary coverage only, not yet scored |

> Scores reflect what vendors **publicly document**, not tested capability. A blank or `–` means nothing public was found, which is not the same as confirmed absent.

## Contents

```
existing-tools/
├── README.md
├── slides/
│   ├── DLP_Analysis-compiled.pptx      # full deck, 68 slides (source of truth)
│   └── DLP_Analysis-compiled.pdf       # PDF preview of the same deck
├── domestic/
│   ├── vendor-coverage.csv             # Endpoint / Network / Data-centric / AI at a glance (slide 4)
│   ├── vendor-profiles.csv             # type, main products, positioning (slide 61)
│   ├── scoring-method.csv              # six dimensions, 60 checklist items (slide 62)
│   ├── scores.csv                      # per-dimension and overall scores (slide 63)
│   ├── ai-capability-matrix.csv        # 13 AI-related capabilities per vendor (slide 64)
│   ├── agentic-ai-readiness.csv        # agentic-era strengths and gaps (slide 65)
│   ├── gaps-and-opportunities.csv      # controls no vendor fully covers (slide 66)
│   └── references.md                   # sources from the speaker notes, by vendor
└── international/
    ├── README.md                       # status and next steps
    └── vendor-coverage-preliminary.csv # preliminary coverage table (slide 68)
```

CSV files are UTF-8 with BOM so Korean text opens correctly in Excel. Vendors are named the same way in every file: Korean name, then the English name in brackets.

## Method

Each vendor is checked against 60 items in six dimensions:

| Dimension | Measures | Items |
|---|---|---|
| D1 Channel coverage | Exit routes the product can see (endpoint, network, AI channels) | 13 |
| D2 Detection technique | How it recognises sensitive content | 12 |
| D3 Enforcement | What it does once a violation is found | 9 |
| D4 AI-specific | Controls built for generative AI use | 10 |
| D5 Operations and deployment | Where it runs and how it is managed | 9 |
| D6 Compliance and certification | Korean public-sector and certification fit | 7 |

**Score** = (● × 1 + ◐ × 0.5) ÷ items in the dimension. `●` documented, `◐` partial or conditional, `?`/blank = 0. The overall score uses all 60 items together, not an average of D1–D6.

## Domestic results

| Dimension | 이로운앤컴퍼니 | 소만사 Somansa | 파수 Fasoo | 컴트루 Comtrue | 마크애니 MarkAny | 지란지교데이터 Jiran | 세이퍼존 SaferZone |
|---|---|---|---|---|---|---|---|
| D1 Channel coverage | 12% | 69% | 54% | 58% | 62% | **73%** | 54% |
| D2 Detection technique | 42% | 42% | 42% | **58%** | 25% | 54% | 13% |
| D3 Enforcement | 33% | 78% | 83% | **89%** | 83% | 83% | 72% |
| D4 AI-specific | **65%** | 50% | 55% | 60% | 45% | 55% | 40% |
| D5 Ops & deployment | 33% | 67% | 61% | 56% | 61% | 44% | **72%** |
| D6 Certification | 29% | 36% | 36% | 64% | **71%** | 43% | 43% |
| **Overall (60 items)** | 35% | 58% | 55% | **63%** | 56% | 60% | 48% |

### Key findings

- **Enforcement is mature.** Every vendor scores 72–89%; blocking, approval, encryption and remediation are table stakes.
- **Detection is the weak spot** (13–58%). No vendor documents exact data matching (EDM / DB fingerprinting); only Comtrue documents document-similarity matching.
- **Breadth and AI depth do not coincide.** 이로운앤컴퍼니 leads AI-specific controls (65%) but covers almost no traditional channel (12%); Jiran leads channels (73%) but documents no LLM API or MCP coverage.
- **Agentic era:** Comtrue is best placed today (LLM-context detection, masking, LLM API coverage, on-prem AI), but no domestic vendor fully controls agent actions.

### Gaps no domestic vendor fully covers (our opportunities)

| Uncovered control | Why it matters for agents | Opportunity |
|---|---|---|
| MCP / tool-call mediation (D1-13) | Agents move data through tool calls, not file uploads | Inline MCP proxy inspecting each tool call's arguments and results |
| Agent action control (D4-05) | Agents act without a user approving each step | Per-agent allow / deny / require-approval policy per action |
| Exact data matching (D2-03) | Agents query databases and can return real customer records | DB fingerprinting across prompts, outputs and tool calls |
| AI response inspection (D4-03) | Model output can carry sensitive data or injected instructions | Check model output and tool results before they reach users or tools |

Also worth checking (outside the checklist, not scored): agent identity and delegated permissions, indirect prompt injection, RAG / vector store governance, agent-to-agent traffic.

Related in this repo: how our own components map to ISO 27001 and NIST CSF 2.0 is in [`../standards-policies/08-project-mapping.md`](../standards-policies/08-project-mapping.md); RAG / vector store risks are mapped on the [`rag-for-dlp`](https://github.com/alpacabar/dlp/tree/rag-for-dlp/rag-for-dlp) branch.

## Open items

- [ ] Add sources for 지란지교데이터 (Jiran) and 세이퍼존 (SaferZone): slides 32–44 have none in the speaker notes.
- [ ] International vendors: see [`international/README.md`](international/README.md).

## Contributors

Domestic vendor analysis by Zira (이로운앤컴퍼니), 윤석 (Somansa, Fasoo), Sercan (Jiran Data, SaferZone) and Alta (Comtrue, MarkAny), S3Lab, Pusan National University.

## Sources

Vendor documentation, brochures and press coverage; see [`domestic/references.md`](domestic/references.md) and the speaker notes in the deck.
