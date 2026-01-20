# Table of Contents

- [Ansible Hardening Threat Model in Sovereign AI](#ansible-hardening-threat-model-in-sovereign-ai)
- [Attacker Models](#attacker-models)
- [Protected Assets](#protected-assets)
- [AI-Specific Threats](#ai-specific-threats)
- [Supply Chain Attacks](#supply-chain-attacks)
- [Lateral Movement](#lateral-movement)
  - [Node → Node](#node--node)
  - [Workload → Node](#workload--node)
  - [Workload → Workload](#workload--workload)
- [Assumptions and Limitations](#assumptions-and-limitations)
- [Mitigating Layers](#mitigating-layers)
- [Threat-to-Control Mapping](#threat-to-control-mapping)
- [Using the Threat Model](#using-the-threat-model)

---

# Ansible Hardening Threat Model in Sovereign AI

This document defines the **threat model** for the Ansible-hardening layer in the Sovereign AI platform.

It answers the following questions:  
– Who we are defending against  
– What we are protecting  
– Which threats are considered realistic  
– Which controls mitigate these threats  

The document is based on architectural assumptions from `docs/architecture.md` and serves as an input for `docs/compliance-mapping.md`.

--- 

# Attacker Models

The following attacker classes are considered within Sovereign AI:

– External Attacker  
  Has no legitimate access, operates via vulnerabilities or the supply chain.

– Compromised Workload  
  A container or AI task that has achieved code execution.

– Internal User with Limited Privileges  
  Developer or operator without administrative rights.

– Compromised Administrator  
  Has access but cannot be trusted.

– Hostile or Compromised Cloud  
  Infrastructure is not considered trusted by default.

--- 

# Protected Assets

Critical platform assets:

– Hosts and Kubernetes nodes  
– Control-plane components  
– GPUs and AI accelerators  
– Training data  
– Inference data  
– Credentials and tokens  
– Logs and audit trails  

Loss of control over any of these is considered critical.

---

# AI-Specific Threats

For AI infrastructures, the following threat classes are highlighted:

– Unauthorized GPU access  
– Training data leakage  
– Data poisoning or manipulation  
– Inference data exfiltration  
– GPU side-channel attacks  
– Persistence in drivers or runtime  

These threats are amplified by the high value of data and computation.

---

# Supply Chain Attacks

Attacks on the supply chain are considered:

– Compromise of Ansible roles  
– Tampering with dependencies in requirements.yml  
– Substitution of Kubernetes binaries  
– Substitution of container runtime  
– Malicious container images  

The supply chain is considered one of the primary attack surfaces.

---

# Lateral Movement

Lateral movement is analyzed along specific directions:

## Node → Node

– Movement between nodes  
– Escalation via SSH, services, shared secrets  

## Workload → Node

– Container breakout to the host  
– Access to kubelet or container runtime  
– Exploitation of kernel vulnerabilities  

## Workload → Workload

– Side-channel attacks  
– Network-based lateral movement  
– Access to shared volumes  

Each direction requires separate controls.

---

# Assumptions and Limitations

Assumptions are based on `docs/architecture.md`:

– Hosts may be compromised  
– Workloads are considered potentially hostile  
– Manual changes are prohibited  
– CI is the only trusted change channel  

Out-of-scope threats:

– Physical access to the data center  
– Hardware backdoors  
– Nation-state level attacks outside the risk model  

---

# Mitigating Layers

Threats are mitigated through multiple layers:

– Ansible Hardening  
  Basic control of nodes and runtime.

– Kubernetes Security  
  Restriction of workloads and network access.

– AI Node Security  
  Isolation of GPUs and AI-specific resources.

Detailed measures are described in:  
– `docs/hardening-baseline.md`  
– `docs/kubernetes-security.md`  
– `docs/ai-node-security.md`

---

# Threat-to-Control Mapping

Each threat category is mapped to specific controls:

– OS hardening  
– SSH and access control  
– auditd and logging  
– Container runtime hardening  
– Kubernetes node hardening  
– GPU hardening  

Detailed mapping is provided in `docs/hardening-baseline.md` and subsequently used in `docs/compliance-mapping.md`.

---

# Using the Threat Model

This model is used for:

– Designing the security baseline  
– Justifying controls  
– Building compliance reports  
– Assessing Day-2 changes  

Any architectural change should be reviewed through this threat model.
