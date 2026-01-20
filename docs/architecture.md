# Table of Contents

- [Ansible Hardening Architecture in Sovereign AI](#ansible-hardening-architecture-in-sovereign-ai)
- [Place of Ansible Hardening in Sovereign AI](#place-of-ansible-hardening-in-sovereign-ai)
- [Control and Execution Chain](#control-and-execution-chain)
- [Trust Boundaries](#trust-boundaries)
- [Control Plane and Data Plane](#control-plane-and-data-plane)
- [Enforcement and Declaration](#enforcement-and-declaration)
- [Security Assumptions and Guarantees](#security-assumptions-and-guarantees)
- [Day-0 / Day-1 / Day-2 Model](#day-0--day-1--day-2-model)
- [Related Documents](#related-documents)

---

# Ansible Hardening Architecture in Sovereign AI

This document describes the architectural role of Ansible hardening in the Sovereign AI platform, trust boundaries, and the model for applying security controls.

It serves as the **entry point** for understanding:  
– Where and how security is enforced  
– Which assumptions are considered acceptable  
– What guarantees the platform provides

This document serves as a foundation for `docs/threat-model.md` and is directly linked to `docs/hardening-baseline.md`.

---

# Place of Ansible Hardening in Sovereign AI

Ansible hardening occupies the **lowest executable security layer** of the Sovereign AI platform.

It is responsible for bringing compute nodes to a trusted state before:  
– Kubernetes  
– AI workloads  
– CI/CD artifacts  
– User workloads

Without this layer, any higher-level declarative policies are considered unreliable.

---

# Control and Execution Chain

The platform is built as a sequential trust chain:

**Terraform → Ansible → Kubernetes → Runtime / Workloads**

Component roles:

– Terraform  
  Declares the infrastructure but does not enforce its security.

– Ansible Hardening  
  Enforces nodes to the security baseline and eliminates drift.

– Kubernetes  
  Manages workloads assuming trusted nodes.

– Runtime  
  Executes containers within predefined constraints.

Each subsequent layer assumes the correctness of the previous one.

---

# Trust Boundaries

The architecture explicitly defines trust boundaries:

– Cloud boundary  
  The cloud is considered potentially untrusted.

– Node boundary  
  Trust is established only after successful hardening.

– Kubelet boundary  
  The kubelet is trusted only if it meets the baseline.

– Container boundary  
  Containers are isolated but not trusted.

– Workload boundary  
  Applications are considered potentially hostile.

Additionally:

– Control plane trust boundary  
  Control-plane nodes have an enhanced baseline.

– CI/CD → Infrastructure boundary  
  Any infrastructure change is allowed only through CI.

---

# Control Plane and Data Plane

The architecture strictly separates:

– Control Plane  
  Management nodes, minimal access, changes strictly limited.

– Data Plane  
  Worker and AI nodes, isolated from the control plane.

Hardening differs for these planes and is applied deliberately.

---

# Enforcement and Declaration

Tool role separation is fundamental:

– Terraform  
  Resource and topology declaration.

– Kubernetes manifests  
  Desired state declaration for workloads.

– Ansible  
  Enforcement: mandatory application of security controls.

– OPA / Rego  
  Enforcement: prohibition of unsafe states.

No declarative layer is considered sufficient without enforcement.

---

# Security Assumptions and Guarantees

Assumptions:

– The cloud is not trusted  
– Hosts may be compromised  
– Administrators can make mistakes  
– Workloads may be hostile  

Guarantees when following the architecture:

– Reproducible security baseline  
– Minimized blast radius  
– Change traceability  
– Verifiable standards compliance  

Guarantees are lost if CI is bypassed or manual changes are made.

---

# Day-0 / Day-1 / Day-2 Model

Day-0:  
– Infrastructure described by Terraform  
– Ansible as the sole source of hardening  

Day-1:  
– Nodes are bootstrapped and brought to baseline  
– Kubernetes deployed only after hardening  

Day-2:  
– Any changes go through CI  
– Manual changes on nodes are prohibited  
– Drift is considered an incident  

Source of truth:  
– Terraform — infrastructure  
– Ansible — node state  
– Git — the single change log  

---

# Related Documents

– `docs/hardening-baseline.md`  
  Set of concrete security controls.

– `docs/kubernetes-security.md`  
  Extension for Kubernetes nodes.

– `docs/ai-node-security.md`  
  Extension for GPU and AI nodes.

– `docs/threat-model.md`  
  Uses this document as the architectural foundation.
