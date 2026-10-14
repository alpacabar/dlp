# 1. The DLP standards landscape

There is no single dedicated DLP standard. DLP requirements are embedded in broader
security frameworks and in data protection law.

## Core security standards

**ISO/IEC 27001:2022 and ISO/IEC 27002:2022, control 8.12 (data leakage prevention).**
The closest thing to a formal DLP standard. The control is new in the 2022 revision:
it requires DLP measures on systems, networks and devices that process, store or
transmit sensitive information. ISO/IEC 27002 adds the implementation guidance,
including the warning that DLP tooling is intrusive and must be deployed within
privacy and employment law. Related controls: 5.12 (classification), 5.14 (information
transfer), 8.10 (deletion), 8.11 (data masking), 8.16 (monitoring activities).

**NIST SP 800-53 Rev. 5.** No single DLP control. The usual mapping is AC-4
(information flow enforcement), SC-7 (boundary protection, including exfiltration
prevention), SC-8 and SC-28 (data in transit and at rest), SI-4 (system monitoring),
the MP family (media protection), and AC-19/AC-20 (mobile devices and external systems).

**NIST Cybersecurity Framework 2.0 (NIST CSWP 29).** Outcome-based, with no DLP control
and no use of the term. The relevant outcomes sit in PR.DS (data security), ID.AM
(asset and data inventory), DE.CM (monitoring) and RS.MI (incident mitigation).
See [05-nist-csf-2.md](05-nist-csf-2.md).

**NIST SP 800-137 (2011), Appendix D.1.8.** Information security continuous monitoring.
DLP appears as one supporting technology area, with the loss / theft / leakage taxonomy
and a DLP strategy. See [04-nist-sp-800-137.md](04-nist-sp-800-137.md).

**NIST SP 1800-28 and SP 1800-29.** NCCoE practice guides on data confidentiality
(identify, protect, detect, respond). The most implementation-oriented NIST material
on this topic.

**CIS Critical Security Controls v8.1, Control 3 (data protection).** Safeguard 3.13
explicitly calls for a DLP solution; the surrounding safeguards cover inventory,
classification, encryption and retention.

## Regulation

| Regime | What drives DLP |
|---|---|
| GDPR | Art. 32 (security of processing), Arts. 33–34 (breach notification) |
| HIPAA Security Rule | Transmission security, access control, audit controls for ePHI |
| PCI DSS v4.0.x | Req. 3 (stored account data), Req. 4 (data in transit) |
| Korea: PIPA (개인정보 보호법) | Technical and managerial safeguard standards (개인정보의 안전성 확보조치 기준) |
| Korea: ISMS-P (KISA) | Certification criteria covering leakage, removable media, and output/transfer of personal data |

For a Korean deployment, ISMS-P is the certification an assessor will actually ask
about, and it maps closely onto the ISO 27001 controls.

## Practitioner material (not standards)

- SANS security policy templates: acceptable use, data classification, removable media —
  the policy layer around a DLP program.
- Cloud Security Alliance CCM v4: DSP (data security and privacy) controls for cloud.
- Vendor documentation (Microsoft Purview, Forcepoint, Symantec) shows how DLP policies
  are expressed in practice — content inspection rules, channel coverage, response
  actions. Useful as evidence of practice, not as a citable requirement.

## Defensible citation set for a paper

ISO/IEC 27001:2022 Annex A 8.12 + ISO/IEC 27002:2022 8.12, NIST SP 800-53 Rev. 5,
CIS Controls v8.1 Control 3, plus ISMS-P for the Korean context.
