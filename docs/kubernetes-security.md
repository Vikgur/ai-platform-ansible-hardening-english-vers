# Table of Contents

- [Kubernetes Security Layer in Sovereign AI](#kubernetes-security-layer-in-sovereign-ai)
- [Role of Kubernetes in the Trust Chain](#role-of-kubernetes-in-the-trust-chain)
- [Kubelet Hardening Requirements](#kubelet-hardening-requirements)
- [Node-level Security Controls](#node-level-security-controls)
- [Interaction with Kubernetes Mechanisms](#interaction-with-kubernetes-mechanisms)
- [Scope of Responsibility](#scope-of-responsibility)
- [Kubernetes-specific Threats](#kubernetes-specific-threats)
- [Application and Lifecycle](#application-and-lifecycle)
- [Related Documents](#related-documents)

---

# Kubernetes Security Layer in Sovereign AI

This document describes the Kubernetes security model at the node and kubelet level within the Sovereign AI platform.

It extends `docs/hardening-baseline.md`, refines the architecture from `docs/architecture.md`, and complements `docs/threat-model.md` with Kubernetes-specific threats.

The focus of this document is **node-level security**, not workload policies.

---

# Role of Kubernetes in the Trust Chain

In Sovereign AI, Kubernetes is viewed as:

– Workload orchestrator  
– Consumer of trusted nodes  
– Enforcement layer atop hardened hosts  

Kubernetes **is not** a source of trust.  
It assumes nodes have already been brought to the correct baseline.

---

# Kubelet Hardening Requirements

The kubelet is considered part of the node's attack surface and must undergo mandatory hardening.

Requirements:

– Secured kubelet API  
– Authentication and authorization enabled  
– Anonymous access prohibited  
– Network access restricted  
– Certificates securely configured  
– Startup parameters controlled  

Any deviation from the baseline is considered a violation.

---

# Node-level Security Controls

Additional controls for Kubernetes nodes:

– Isolation of control-plane nodes  
– Workload scheduling prohibited on control-plane  
– HostPath access restricted  
– Cgroup and namespace control  
– Protection for kube-proxy and CNI  

These controls complement OS hardening rather than replace it.

---

# Interaction with Kubernetes Mechanisms

Ansible hardening integrates with Kubernetes security mechanisms:

– Seccomp  
  Prepares the node and profiles.

– AppArmor  
  Ensures availability and correct configuration.

– Admission Controllers  
  Ansible does not replace admission controllers but establishes a trusted foundation.

Ansible provides **enforcement capability**, Kubernetes provides **enforcement at the workload level**.

---

# Scope of Responsibility

Clear separation of responsibilities:

Ansible is responsible for:  
– Kubelet configuration  
– Node-level hardening  
– Runtime environment  
– Basic security profiles  

Kubernetes is responsible for:  
– Admission policies  
– Workload isolation  
– Network policies  
– Runtime enforcement  

Mixing responsibilities is considered an architectural flaw.

---

# Kubernetes-specific Threats

Building on `docs/threat-model.md`, identified threats include:

– Kubelet compromise  
– Unauthorized access to the node API  
– Container escape to the host  
– Lateral movement via kubelet  
– Control-plane compromise through the node  

Controls aim to reduce these attack surfaces.

---

# Application and Lifecycle

Kubernetes hardening:

– Applied only after OS hardening  
– Re-executable without side effects  
– Verified via CI  

Any changes to kubelet configuration:  
– Go through Git  
– Applied via Ansible  
– Logged and audited  

---

# Related Documents

– `docs/hardening-baseline.md`  
– `docs/architecture.md`  
– `docs/threat-model.md`  
– `docs/ai-node-security.md`  
– `docs/compliance-mapping.md`
