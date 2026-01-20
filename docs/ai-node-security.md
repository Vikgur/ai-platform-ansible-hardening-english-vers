# Table of Contents

- [AI Node Security in Sovereign AI](#ai-node-security-in-sovereign-ai)
- [Role of AI Nodes in the Architecture](#role-of-ai-nodes-in-the-architecture)
- [GPU Isolation Models](#gpu-isolation-models)
- [Runtime Constraints for AI Workloads](#runtime-constraints-for-ai-workloads)
- [Data Leak Prevention](#data-leak-prevention)
- [Multi-Tenant AI Threat Surface](#multi-tenant-ai-threat-surface)
- [Interaction with Kubernetes Security](#interaction-with-kubernetes-security)
- [Application and Guarantees](#application-and-guarantees)
- [Related Documents](#related-documents)

---

# AI Node Security in Sovereign AI

This document describes the security model for GPU and AI nodes in the Sovereign AI platform.

It extends `docs/kubernetes-security.md`, directly relies on `docs/threat-model.md` for AI-specific threats, and is used in `docs/compliance-mapping.md` as a source of AI-specific controls.

The focus of the document is **compute and data isolation in a multi-tenant AI environment**.

---

# Role of AI Nodes in the Architecture

AI nodes are considered:

– High-value compute assets  
– Concentration points for sensitive data  
– Enhanced attack surface  

Compromise of an AI node is considered a critical incident, regardless of blast radius.

---

# GPU Isolation Models

Isolation models used:

– Device-level isolation  
– Namespace and cgroup restrictions  
– No direct access to host GPU  
– Strict binding of GPU to workload  

Shared GPU usage is allowed only under formal restrictions.

---

# Runtime Constraints for AI Workloads

Additional runtime controls for AI workloads:

– No privileged containers  
– Capability restrictions  
– Access control to device files  
– IPC and shared memory restrictions  
– Network connection controls  

An AI workload is never considered trusted.

---

# Data Leak Prevention

Mechanisms for preventing data exfiltration:

– Egress traffic restrictions  
– Storage access control  
– Isolation of training and inference data  
– Mandatory access logging  

Any data leak is considered a violation of sovereignty.

---

# Multi-Tenant AI Threat Surface

In a multi-tenant environment, threats include:

– Side-channel attacks via GPU  
– Cross-workload data leakage  
– Persistence in runtime or drivers  
– Bypassing restrictions through ML frameworks  

Controls are aimed at minimizing cross-tenant impact.

---

# Interaction with Kubernetes Security

AI node security is built on top of Kubernetes:

– Uses node labeling and taints  
– Relies on admission controls  
– Requires strict workload placement  

Ansible ensures correct node state; Kubernetes enforces workload control.

---

# Application and Guarantees

AI hardening:

– Applied after OS and Kubernetes hardening  
– Re-executable without side-effects  
– Verified through CI  

Guarantees are valid only if the entire trust chain is maintained.

---

# Related Documents

– `docs/kubernetes-security.md`  
– `docs/threat-model.md`  
– `docs/hardening-baseline.md`  
– `docs/compliance-mapping.md`  
