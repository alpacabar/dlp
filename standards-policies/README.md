# DLP industry standards and policies

Notes on the standards, frameworks and regulations behind data loss prevention,
collected for the DLP project (detection pipeline + audit dashboard) and for the
related paper.

Everything here is **paraphrase with citations**, not reproduced standard text.
ISO/IEC standards are copyrighted; for normative wording, read the clause itself
in a licensed copy.

## Contents

| File | What it covers |
|---|---|
| [docs/01-landscape.md](01-landscape.md) | The whole field in one page: which standards, frameworks and laws say anything about DLP |
| [docs/02-iso-27001-vs-27002.md](02-iso-27001-vs-27002.md) | How the requirements standard and the guidance standard differ, and which to cite |
| [docs/03-iso-27002-8-12.md](03-iso-27002-8-12.md) | Control 8.12 (data leakage prevention): attributes, purpose, guidance, cross-references, legal caveat |
| [docs/04-nist-sp-800-137.md](04-nist-sp-800-137.md) | Appendix D.1.8: the loss / theft / leakage taxonomy and the DLP strategy, with the control mapping brought up to SP 800-53 Rev. 5 |
| [docs/05-nist-csf-2.md](05-nist-csf-2.md) | CSF 2.0 has no DLP control: where the same outcomes live, and what happened to CSF 1.1's PR.DS-5 |
| [docs/06-iso-vs-nist.md](06-iso-vs-nist.md) | Side-by-side comparison, the terminology clash around "leakage", where the two agree |
| [docs/07-monev-2023.md](07-monev-2023.md) | Notes on Monev (2023), "Data Leakage Prevention in ISO 27001", and which of its constructs are the author's rather than ISO's |
| [docs/08-project-mapping.md](08-project-mapping.md) | Mapping this project's components to ISO 27001 Annex A and NIST CSF 2.0 |
| [references.bib](references.bib) | BibTeX for everything cited here |

## How to cite, in short

- **The existence of a DLP requirement** → ISO/IEC 27001:2022, Annex A, control 8.12.
- **What implementing it involves** → ISO/IEC 27002:2022, 8.12.
- **The loss / theft / leakage taxonomy** → NIST SP 800-137 (2011), Appendix D.1.8 — cite the
  primary text, not someone's summary of it.
- **Current US control mappings** → NIST SP 800-53 Rev. 5 (SP 800-137's mapping is Rev. 3).
- **Outcome-level framing** → NIST CSF 2.0 (NIST CSWP 29).

## Status

Working notes, updated as the project goes. Checked against the primary documents
(ISO/IEC 27002:2022, ISO/IEC 27001:2022, NIST SP 800-137, NIST CSWP 29, Monev 2023)
in September 2026. Verify anything time-sensitive — domain lists, framework versions,
control numbering — before relying on it in a publication.
