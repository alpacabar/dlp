# 04 — Standards and industry

## OWASP Top 10 for Agentic Applications (2026)

[Official list](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/). DLP relevance is our reading of each entry's examples, not an OWASP label.

| ID | Risk | DLP relevance | Our threat category |
|---|---|---|---|
| ASI01 | Agent Goal Hijack | Hidden instructions make the agent leak internal data | T1 |
| ASI02 | Tool Misuse & Exploitation | Legitimate tools used to move sensitive data | T1, T2 |
| ASI03 | Identity & Privilege Abuse | Privileged agent passes credentials to a lower-privilege one | T5 |
| ASI04 | Agentic Supply Chain Vulnerabilities | Tampered tools / agent cards redirect data | T2 |
| ASI05 | Unexpected Code Execution (RCE) | Indirect | — |
| ASI06 | Memory & Context Poisoning | Planted memory fires later | T3 |
| ASI07 | Insecure Inter-Agent Communication | Interception and spoofing between agents | T5 |
| ASI08 | Cascading Failures | Indirect | — |
| ASI09 | Human-Agent Trust Exploitation | Users tricked into sharing credentials | — |
| ASI10 | Rogue Agents | Agent acting outside its intended role | T3, T5 |

**Note:** T4 (contextual over-sharing without an attacker) has no direct OWASP entry — the list is attacker-centred.

## ISO / NIST

No standard has an agent-specific DLP control yet. The general requirements apply, and two clauses fit agents especially well:

- **ISO/IEC 27002:2022 8.12** covers unauthorized disclosure by individuals or by systems — an agent is a system, and the definition is intent-neutral, so T4 is in scope. See [`../standards-policies/03-iso-27002-8-12.md`](../standards-policies/03-iso-27002-8-12.md).
- **ISO 8.26 application security requirements** — where our dual-direction agent guardrail maps. See [`../standards-policies/08-project-mapping.md`](../standards-policies/08-project-mapping.md).
- **OpenTelemetry GenAI semantic conventions** keep tool names, order and timing by default and make message content opt-in — the constraint behind the content-free drift strand. See [`agent-behavioural-drift/01-problem.md`](agent-behavioural-drift/01-problem.md).

## What DLP vendors cover

From our domestic competitor analysis ([`../existing-tools/`](../existing-tools/)), mapped to these categories:

| Control | Category | Korean vendors today |
|---|---|---|
| MCP / tool-call mediation (D1-13) | T1, T2 | Comtrue ◐ only |
| Agent action control (D4-05) | T1, T4 | ◐ at best (5 vendors) |
| AI response inspection (D4-03) | T1, T3 | Fasoo, Jiran ●; 이로운앤컴퍼니 ◐ |
| Prompt injection detection (D4-04) | T1 | 이로운앤컴퍼니, Comtrue ● |

Research prototypes for each gap exist (see [`03-defences.md`](03-defences.md)); none is shipped by a domestic vendor. International vendors are not scored yet.
