# 6. ISO/IEC 27002:2022 8.12 vs NIST SP 800-137 D.1.8

Built from the two primary texts only.

## Side by side

| Aspect | NIST SP 800-137 (2011), D.1.8 | ISO/IEC 27002:2022, 8.12 |
|---|---|---|
| Status | Guidance; part of continuous monitoring | Guidance for a normative ISO/IEC 27001 Annex A control |
| Umbrella term | Data loss | Data leakage |
| Intent | Split: theft (intentional) vs leakage (inadvertent) | Not distinguished |
| Actors | Espionage, disgruntled or careless employees, lost devices | Individuals or systems; adversary intelligence actions |
| Security properties | C, I and A, framed as information management | Confidentiality only |
| Function | Monitoring and mitigation | Preventive and detective (Protect / Detect) |
| Organizational measures | Inventory and classification, metrics, lifecycle policies | Classification, channel monitoring, export approval by data owners, user accountability |
| Tool functions | Monitor data at rest, in use, in transit; alert, log, block | Identify and monitor, detect disclosure, block actions |
| Channels named | Laptops, internet storage, USB | Email, file transfer, mobile devices, portable storage, cloud uploads, copy/paste, screenshots, backups |
| Distinctive content | Metrics collection; SP 800-53 control mapping | Deception and honeypots; legal limits on monitoring employees |
| Related controls | AC-4, AC-17, CA-3, CA-7, CM-7, SC-9 (now SC-8), SI-12 | 5.12, 5.15; also 5.8, 6.6, 8.16, 8.26, 8.27 |

## The terminology clash

```
NIST "data loss"   ≈   ISO "data leakage"      (the umbrella)
├── NIST "data theft"    → intentional exposure
└── NIST "data leakage"  → inadvertent exposure
```

Same word, different scope: NIST's "leakage" is only the lower branch; ISO's covers the
whole tree. **Any paper using the term must say which definition it adopts.**

## Where they agree

1. **The goal** — preventing exposure of sensitive information; confidentiality is central
   in both.
2. **Classification comes first** — NIST lists inventory and classification in its strategy;
   ISO puts identification and classification first in its measures.
3. **DLP is a program, not a product** — both pair technical tooling with organizational
   measures (NIST: lifecycle policies; ISO: export approval, accountability, training,
   terms of use).
4. **Monitoring is central** — data at rest, in use and in transit (NIST); leakage channels
   (ISO).
5. **Detect, then act** — alerting and blocking in both; NIST adds logging, ISO adds
   quarantine.
6. **Overlapping threats and channels** — insiders in both; espionage/adversary intelligence
   in both; USB and internet/cloud storage in both.

## Where they differ

- **Intent:** NIST classifies by intent, ISO does not.
- **Emphasis:** NIST stresses metrics and lifecycle policy; ISO stresses the breadth of
  channels, organizational accountability, deception, and the legal limits of monitoring.
- **Currency:** SP 800-137's control mapping predates SP 800-53 Rev. 5 (see
  [04-nist-sp-800-137.md](04-nist-sp-800-137.md)).
