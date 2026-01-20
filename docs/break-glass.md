# Table of Contents

- [Break-Glass Access in Sovereign AI](#break-glass-access-in-sovereign-ai)
- [Break-Glass Principles](#break-glass-principles)
- [Conditions for Emergency Access](#conditions-for-emergency-access)
- [Roles and Access Model](#roles-and-access-model)
- [Approval and Time Windows](#approval-and-time-windows)
- [Logging and Auditing](#logging-and-auditing)
- [Revocation and Post-Incident Review](#revocation-and-post-incident-review)
- [Incident Lifecycle](#incident-lifecycle)
- [Implementation and Control](#implementation-and-control)
- [Related Documents](#related-documents)

---

# Break-Glass Access in Sovereign AI

This document describes the **emergency access (break-glass)** model in the Sovereign AI platform.

Break-glass is considered a **controlled violation of the normal trust model**, allowed only during incidents and never as a permanent administrative mechanism.

The document is linked to `docs/threat-model.md` for insider threats and is used in `docs/compliance-mapping.md`.

---

# Break-Glass Principles

Core principles:

– Access is denied by default  
– No persistent privileges  
– Mandatory justification  
– Limited duration  
– Full audit  

Break-glass is an exception, not an alternative to CI/CD.

---

# Conditions for Emergency Access

Emergency access is allowed only under:

– Security incident  
– CI/CD unavailability  
– Failure of automated mechanisms  
– Threat of data loss or service disruption  

Any other scenario is considered a violation.

---

# Roles and Access Model

Access model is strictly limited:

– Separate break-glass role  
– Minimal set of commands  
– No access to secrets  
– No baseline modifications  

Access is granted to a specific individual, not a group.

---

# Approval and Time Windows

Access rules:

– Mandatory justification  
– Explicit approval  
– Strictly limited time window  
– Automatic revocation after expiration  

Lack of justification results in automatic denial.

---

# Logging and Auditing

All actions in break-glass mode:

– Logged at the OS level  
– Recorded via auditd  
– Sent to a centralized repository  
– Tagged as break-glass events  

Unlogged actions are not permitted.

---

# Revocation and Post-Incident Review

After the incident:

– Access is automatically revoked  
– Hardening is reapplied  
– Drift absence is verified  
– Post-incident review is conducted  

Any changes outside the baseline must be reverted.

---

# Incident Lifecycle

Incident lifecycle:

– Detection  
– Break-glass activation  
– Limited intervention  
– Access revocation  
– Baseline restoration  
– Analysis and lessons learned  

Skipping any stage is not allowed.

---

# Implementation and Control

Break-glass is implemented via the role:

– break-glass  

Control is ensured by:

– Ansible enforcement  
– auditd  
– CI checks  
– Compliance mapping  

---

# Related Documents

– `docs/threat-model.md`  
– `docs/compliance-mapping.md`  
– `docs/hardening-baseline.md`  
