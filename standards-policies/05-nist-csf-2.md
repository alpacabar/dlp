# 5. NIST Cybersecurity Framework 2.0 (NIST CSWP 29)

**CSF 2.0 never uses the terms "data loss prevention", "data leakage" or "data
exfiltration".** That is by design: the CSF states outcomes, not controls or products.
The "how" lives in the online Informative References and Implementation Examples, which
point to SP 800-53, ISO/IEC 27001 and others.

## The dropped subcategory

CSF 1.1 had **PR.DS-5: "Protections against data leaks are implemented."** It does not
exist as a separate outcome in 2.0 — the numbering gaps in PR.DS mark relocated 1.1
subcategories. Its intent appears to be absorbed into the data-state outcomes (PR.DS-01,
PR.DS-02, PR.DS-10). Confirm against NIST's official CSF 1.1 → 2.0 crosswalk before
citing that mapping.

## Where DLP outcomes live in 2.0

| DLP element (per SP 800-137 / ISO 8.12) | CSF 2.0 outcome |
|---|---|
| Data inventory and classification | ID.AM-07 (data inventories, metadata), ID.AM-05 (prioritise by classification) |
| Knowing where data flows | ID.AM-03 (authorized network data flows) |
| Data at rest / in transit / in use | PR.DS-01, PR.DS-02, PR.DS-10 |
| Backup protection | PR.DS-11 |
| Access restriction | PR.AA-05 (least privilege, separation of duties) |
| User awareness | PR.AT-01, PR.AT-02 |
| Logging | PR.PS-04 |
| Monitoring users and channels | DE.CM-03 (personnel activity and technology usage), DE.CM-01 (networks), DE.CM-09 (computing hardware, software and their data) |
| Containing a leak | RS.MI-01 |
| Legal limits on monitoring | GV.OC-03 (legal, regulatory and contractual requirements, including privacy and civil liberties) |

The "at rest, in transit, in use" split in PR.DS matches SP 800-137's wording almost
exactly: NIST kept the model and dropped the DLP label.

## The three sources compared

| | DLP named? | Form |
|---|---|---|
| ISO/IEC 27002 8.12 | yes | dedicated control + implementation guidance |
| NIST SP 800-137 D.1.8 | yes | strategy + tool description, in an appendix |
| NIST CSF 2.0 | no | general data security, asset management and monitoring outcomes |

## Privacy angle

DE.CM-03 (monitoring personnel activity) sits in the same framework as GV.OC-03, which
explicitly covers privacy and civil liberties obligations, and CSF 2.0 treats
cybersecurity risk and privacy risk as overlapping but distinct (Fig. 6). Combined with
the 8.12 legal caveat, this is a solid citation base for the argument that employee-facing
DLP monitoring needs privacy-preserving design. Pair it with the NIST Privacy Framework
if the paper needs a stronger privacy-side reference.
