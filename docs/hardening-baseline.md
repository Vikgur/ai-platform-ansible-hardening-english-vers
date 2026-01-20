# Table of Contents

- [Hardening Baseline for Sovereign AI](#hardening-baseline-for-sovereign-ai)
- [Formal Definition of Hardened Host](#formal-definition-of-hardened-host)
- [Basic OS Controls](#basic-os-controls)
  - [Kernel](#kernel)
  - [Sysctl](#sysctl)
  - [Filesystem](#filesystem)
  - [Users and Access](#users-and-access)
  - [Network](#network)
- [Container Runtime Baseline](#container-runtime-baseline)
- [Immutable and Mutable Zones](#immutable-and-mutable-zones)
- [Drift and Change Control](#drift-and-change-control)
- [Implementation and Compliance](#implementation-and-compliance)
- [Using the Baseline](#using-the-baseline)

---

# Hardening Baseline for Sovereign AI

This document formally defines what is considered a **hardened host** within the Sovereign AI platform.

It describes the **minimally acceptable security baseline**, mandatory for all nodes before running Kubernetes, AI workloads, or any production workloads.

The document serves as a foundational security layer and is used:  
– As a reference for Ansible roles  
– As a source for `docs/compliance-mapping.md`  
– As a foundation for `docs/architecture.md`  

---

# Formal Definition of Hardened Host

A node is considered hardened only if all of the following conditions are met simultaneously:

– Node state is fully reproducible  
– Configuration is managed exclusively by Ansible  
– No manual changes exist  
– Access is minimal and controlled  
– Auditing is enabled and immutable  
– Network policies are explicitly enforced  
– Configuration drift is detectable  

Partial compliance is not allowed.

---

# Basic OS Controls

## Kernel

– Unused modules are disabled  
– Loading of arbitrary modules is prohibited  
– Memory protection parameters are strengthened  
– Protection against privilege escalation enabled  

## Sysctl

– Network attack surfaces restricted  
– Protection against spoofing and flooding  
– IPC and namespaces controlled  
– Kernel hardening parameters reinforced  

## Filesystem

– Separation of mutable and immutable zones  
– noexec, nodev, nosuid applied where applicable  
– System directories protected  
– Access rights controlled  

## Users and Access

– No shared accounts  
– Strict sudo policies  
– Limited SSH access  
– Break-glass only according to policy  

## Network

– Deny-by-default  
– Only minimally required ports open  
– Outgoing connections controlled  
– Explicit separation of traffic planes  

---

# Container Runtime Baseline

For all nodes running container workloads:

– Privileged mode disabled by default  
– Access to host namespaces restricted  
– Seccomp and capabilities drop enabled  
– Image and runtime configuration controlled  

The runtime is considered part of the host's attack surface.

---

# Immutable and Mutable Zones

The host architecture is divided into zones:

Immutable:  
– System binaries  
– Hardening configurations  
– Security policies  

Mutable:  
– Runtime data  
– Logs  
– Temporary files  

Any change to the immutable zone is considered an incident.

---

# Drift and Change Control

Drift requirements:

– Drift is considered a security violation  
– Drift detection is automated  
– Correction is performed via Ansible  
– Manual correction is prohibited  

Drift management complements, rather than replaces, hardening.

---

# Implementation and Compliance

This baseline is implemented via the roles:

– os-hardening  
– ssh-hardening  
– auditd  
– firewall  
– logging  

Each control is:  
– Executable  
– Verifiable  
– Traceable  

---

# Using the Baseline

The baseline is applied:

– During bootstrap of new nodes  
– During repeated hardening  
– For audits and checks  
– As input for `docs/compliance-mapping.md`  

Deviations from the baseline are allowed only through formal exceptions.
