# Table of Contents

- [Compliance Mapping for Sovereign AI](#compliance-mapping-for-sovereign-ai)
- [Supported Standards](#supported-standards)
- [Control Mapping Principle](#control-mapping-principle)
- [Control Mapping Table](#control-mapping-table)
- [CI Checks](#ci-checks)
- [External Audit Requirements](#external-audit-requirements)
- [Link to Threat Model](#link-to-threat-model)
- [Using Compliance Mapping](#using-compliance-mapping)

---

# Compliance Mapping for Sovereign AI

This document describes how the security controls of the Sovereign AI platform map to regulatory and industry standards.

It aggregates `docs/hardening-baseline.md`, `docs/kubernetes-security.md`, and `docs/ai-node-security.md`, and relies on the threat model from `docs/threat-model.md`.

Compliance is treated as an **executable and verifiable system characteristic**, not merely a declaration of intent.

---

# Supported Standards

The platform targets the following frameworks:

– CIS Benchmarks  
– NIST 800-53 and 800-190  
– ISO 27001 and 27002  

Standards support is implemented at the host, Kubernetes node, and AI infrastructure levels.

---

# Control Mapping Principle

Each standard requirement:

– Maps to a specific security control  
– Implemented by one or more Ansible roles  
– Either fully automated  
– Or explicitly marked as requiring external audit  

Uncovered requirements are considered an architectural defect.

---

# Control Mapping Table

| Security Control | Standard | Implementation | Role | Automation |
|-----------------|---------|----------------|------|------------|
| OS Hardening | CIS / NIST | Ansible | os-hardening | Full |
| SSH Access Control | CIS / ISO | Ansible | ssh-hardening | Full |
| Audit Logging | CIS / NIST | Ansible | auditd | Full |
| Network Filtering | CIS | Ansible | firewall | Full |
| Kubernetes Node Hardening | CIS / NIST | Ansible | kubernetes-node-hardening | Full |
| Container Runtime Hardening | CIS | Ansible | container-runtime-hardening | Full |
| GPU / AI Node Isolation | NIST / ISO | Ansible | gpu-hardening | Partial |
| Break-Glass Access | ISO | Ansible | break-glass | Full |

This table represents a minimal view and can be expanded.

---

# CI Checks

Automatically verified:

– Baseline compliance  
– Correctness of roles and playbooks  
– Presence of forbidden configurations  
– Adherence to policy-as-code rules  
– Generation of compliance reports  

CI is considered the primary mechanism for proving compliance.

---

# External Audit Requirements

External audit is required for:

– Access management procedures  
– Incident response processes  
– Physical security  
– Organizational policies  

These areas are intentionally not automated in Ansible.

---

# Link to Threat Model

Each compliance control:

– Is linked to a specific threat  
– Justified by `docs/threat-model.md`  
– Reduces measurable risk  

Compliance without threat linkage is considered formal only.

---

# Using Compliance Mapping

This document is used for:

– Audit preparation  
– Demonstrating compliance to customers  
– Justifying security architecture  
– Assessing Day-2 changes  

Any baseline change requires updating the compliance mapping.
