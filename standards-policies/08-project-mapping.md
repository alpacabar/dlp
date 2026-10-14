# 8. Mapping the project to the standards

How the components of the DLP baseline engine line up with ISO/IEC 27001:2022 Annex A and
NIST CSF 2.0. Claims are kept to what the implementation actually does.

| Component | What it does | ISO/IEC 27001 Annex A | NIST CSF 2.0 |
|---|---|---|---|
| Presidio PII + custom secret patterns (AWS/SSH keys, JWT) | Identification of sensitive data in files and text | 5.12 classification, 5.34 privacy and protection of PII | ID.AM-07 |
| YARA rules (binary headers, confidential markers) | Signature detection on files | 8.12 | DE.CM-09 |
| Endpoint and folder watchers (local, USB, messenger download folders, cloud sync folders) | Monitoring of files arriving or sitting on monitored paths | 8.12, 7.10 storage media | DE.CM-09 |
| Text redaction | Masking before onward use | 8.11 data masking | PR.DS-01 |
| File quarantine | Containment of critical detections | 8.12, 5.26 response to incidents | RS.MI-01 |
| Dual-direction AI agent guardrail | Inspection of tool-call arguments and responses | 8.12, 8.26 application security requirements | DE.CM-09, PR.DS-02 |
| AI usage sensor (endpoint) | Which endpoints connect to which AI services, by which program (metadata only) | 8.12, 8.16 monitoring activities, 5.7 threat intelligence (contextual) | DE.CM-03, DE.CM-01 |
| syslog receiver + alerts.json + log rotation | Collection and retention of security events | 8.15 logging, 8.16 monitoring | PR.PS-04 |
| React dashboard (Dashboard / Detection / Reporting / AI usage) | Review of events and remediation outcomes | 8.16, 5.28 collection of evidence | DE.AE-06, RS.AN-06 |
| Pseudonymised endpoint and user IDs (HMAC), metadata-only AI telemetry | Minimisation in the monitoring itself | 8.12 "other information" (legal limits), 5.34 | GV.OC-03 |

## Scope statements to keep honest

Write these next to the mapping, in the deck and in any paper:

- **Channel coverage is file-landing, not transmission.** Watching a messenger's download
  folder or a cloud sync folder observes data at rest after arrival; it does not inspect
  outbound sends. ISO 8.12 and NIST both treat outbound channels as central, so this is a
  partial implementation of the control, not a complete one.
- **HTTPS is not decrypted.** No content inspection of web traffic; Shadow IT detection
  beyond the AI usage sensor's metadata is out of scope.
- **AI usage discovery is metadata only**, and IP-based matches are reported as "possible"
  unless the vendor's own application or the endpoint's DNS cache confirms them.
- **Anything scripted or simulated in the UI must be labelled as such** (e.g. the
  remediation/unlearning console) until it is wired to real results. An audit dashboard
  that displays unperformed checks is a correctness problem, not a cosmetic one.
- **Alert levels are project-defined.** The level 12 / 13 / 14 thresholds used for
  PII / critical / quarantine are this project's own convention; verify against the Wazuh
  rule-level documentation before presenting them as the vendor's classification.

## Privacy argument (for the paper)

ISO/IEC 27002:2022 8.12 states that DLP inherently monitors personnel communications and
that privacy, data protection, employment and interception law must be considered before
deployment. CSF 2.0 places personnel-activity monitoring (DE.CM-03) alongside legal and
civil-liberties obligations (GV.OC-03). Korea's PIPA and the ISMS-P criteria apply directly
to a deployment here. The project's pseudonymisation and metadata-only telemetry are a
direct response to that requirement, which is a defensible contribution claim: privacy-
preserving DLP monitoring rather than DLP monitoring alone.
