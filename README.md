# Table of Contents

- [AI Platform Ansible Hardening](#ai-platform-ansible-hardening)
  - [Key Idea](#key-idea)
- [Repository Purpose in the Sovereign AI Platform](#repository-purpose-in-the-sovereign-ai-platform)
- [Scope and Responsibility Boundaries](#scope-and-responsibility-boundaries)
- [Key Security Principles](#key-security-principles)
- [Architectural Place in Sovereign AI](#architectural-place-in-sovereign-ai)
- [Repository Composition](#repository-composition)
- [Application Flows and Invariants](#application-flows-and-invariants)
- [Compliance and Standards](#compliance-and-standards)
- [Usage Methods](#usage-methods)
- [How to Read the Documentation](#how-to-read-the-documentation)
- [Repository Navigation](#repository-navigation)
- [Lifecycle Overview](#lifecycle-overview)
- [Non-Goals and Anti-Patterns](#non-goals-and-anti-patterns)

---

# AI Platform Ansible Hardening

Reference Ansible hardening platform for sovereign and regulated AI infrastructures.

This repository provides a **Senior DevSecOps-level reference implementation**, designed for strictly regulated, sovereign AI environments where security, reproducibility, and compliance take priority over speed and convenience.

The project focuses on **infrastructure hardening**, not configuration for its own sake:  
– Operating systems  
– Kubernetes nodes  
– GPU nodes for AI workloads  
– Access, auditing, and change control  
– Compliance with standards (CIS, NIST, ISO)  

Ansible is used as an **executable security control**, embedded into the platform lifecycle and CI/CD, without manual actions or snowflake hosts.

## Key Idea

> Sovereign AI is impossible without managed hosts.  
> Hardening is not configuration—it's an architectural property.

This repository demonstrates what **production-level hardening** looks like, applicable in enterprise, government, and defense contexts.

---

# Repository Purpose in the Sovereign AI Platform

This repository exists as the **trust foundation** for the entire Sovereign AI platform.

It addresses a key DevSecOps challenge in AI infrastructures:  
**Kubernetes, CI/CD, and AI pipelines cannot be considered secure if hosts and nodes are not under strict, reproducible, and verifiable control.**

The repository implements:  
– System hardening of hosts and nodes as code  
– Elimination of configuration drift  
– Enforcement of a security baseline  
– Built-in verifiability and auditing  

Here, Ansible is not used as a "configuration tool" but as an **executable security control**, integrated into the Sovereign AI infrastructure lifecycle.

---

# Scope and Responsibility Boundaries

Within the responsibility of this repository:

– Basic operating system hardening  
– Hardening of Kubernetes nodes (control-plane and worker)  
– Hardening of container runtime  
– Hardening of GPU / AI nodes  
– Access control (SSH, sudo, break-glass)  
– Auditing, logging, and change traceability  
– CIS / NIST / ISO compliance at the host level  

Intentionally out of scope for this repository:

– Infrastructure provisioning (Terraform)  
– Cloud networking and IAM constructs  
– Cluster-level Kubernetes policies  
– Runtime security and admission controls  
– CI/CD pipelines and supply-chain security  

This separation removes overlapping responsibilities and ensures that each platform layer can be independently verified.

---

# Key Security Principles

The platform is built on the following immutable principles:

– immutable-first: hosts are considered replaceable, not "fixable"  
– policy-as-code: all requirements are expressed in code and verifiable  
– least privilege: default to the minimum necessary permissions  
– zero trust nodes: a node is not trusted by virtue of existence  
– auditability by default: every change leaves a trace  

Hardening is treated as an **architectural property of the system**, not just a set of best practices.

---

# Architectural Place in Sovereign AI

This repository is part of the lower security layer of the Sovereign AI platform.

Connections with other components:

– ai-cloud-platform-terraform  
  Provides outputs (IP, node roles, metadata) on which hardening is applied.

– ai-kubernetes-security-platform  
  Assumes nodes are already brought to a trusted baseline and does not include OS hardening.

– ai-secure-ci-cd-platform  
  Uses this repository for automated checks, dry-runs, and compliance reporting.

Thus, this repository closes the **trust boundary between infrastructure and the platform**.

---

# Repository Composition

The internal structure reflects the architectural separation of responsibilities:

– inventories  
  Environments and node roles without hardening logic.

– playbooks  
  Explicit application scenarios: bootstrap, hardening, Kubernetes, AI nodes, break-glass.

– roles  
  Isolated security controls, applicable independently.

– policies  
  OPA/Rego policies preventing unsafe configurations.

– ci  
  Checks, linting, and compliance reports as part of DevSecOps.

– docs  
  Architecture, threat model, security baseline, and compliance mapping.

---

# Application Flows and Invariants

All application scenarios have the following properties:

– Idempotence  
– Safe re-execution  
– No side-effects  

Supported flows:

– Bootstrap of a new node  
  Minimal access, preparing for managed hardening.

– Hardening of an existing node  
  Bringing it to baseline without manual changes.

– Kubernetes control-plane and worker nodes  
  Nodes brought to cluster security expectations.

– AI / GPU nodes  
  Special consideration for drivers and AI workloads.

– Break-glass scenario  
  Controlled temporary access with auditing and revocation.

---

# Compliance and Standards

Standards compliance is treated as a first-class system feature.

Supported frameworks:

– CIS Benchmarks  
– NIST 800-53 / 800-190  
– ISO 27001 / 27002  

Coverage is ensured by:

– Automated hardening roles  
– Verifiable policy-as-code rules  
– Generated compliance reports  

Compliance here is not declarative; it is **executable and verifiable**.

---

# Usage Methods

The preferred method of application is through CI/CD.

Allowed options:

– CI execution  
  The primary and recommended method.

– Execution using Terraform outputs  
  For automatic hardening after provisioning.

– Local execution  
  Allowed only for development and validation.

Manual ad-hoc runs are considered exceptions and require justification.

---

# How to Read the Documentation

The documentation is structured as a continuous security chain:

1. `docs/architecture.md`  
2. `docs/threat-model.md`  
3. `docs/hardening-baseline.md`  
4. `docs/kubernetes-security.md`  
5. `docs/ai-node-security.md`  
6. `docs/compliance-mapping.md`  
7. `docs/break-glass.md`  

Each document references the previous one and does not exist in isolation.

---

# Repository Navigation

Use `docs/repository-structure.md` for navigation.

---

# Lifecycle Overview

– Infrastructure is provisioned with Terraform  
– Nodes undergo bootstrap  
– Hardening is applied  
– State is validated by policies  
– Compliance is recorded in reports  
– Changes go through CI  

The lifecycle is intentionally simple and reproducible.

---

# Non-Goals and Anti-Patterns

This repository deliberately does not address:

– Manual node administration  
– Drift management as a CMDB  
– Ad-hoc "fixes" bypassing CI  
– Mixing provisioning and hardening  

Any attempt to use it as a "set of Ansible scripts" is considered an architectural mistake.

