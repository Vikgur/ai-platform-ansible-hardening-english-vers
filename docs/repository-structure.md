# Repository Structure of Ansible Hardening

- [Architectural Logic of the Structure](#architectural-logic-of-the-structure)
- [Repository Root](#repository-root)
  - [inventories/](#inventories)
  - [playbooks/](#playbooks)
  - [roles/](#roles)
  - [collections/](#collections)
  - [policies/](#policies)
  - [ci/](#ci)
  - [scripts/](#scripts)
  - [docs/](#docs)
- [Purpose of Directories and Files](#purpose-of-directories-and-files)
  - [README.md](#readmemd)
  - [ansible.cfg](#ansiblecfg)
  - [requirements.yml](#requirementsyml)
  - [.ansible-lint](#ansible-lint)
  - [.yamllint](#yamllint)
  - [.gitignore](#gitignore)
  - [inventories/](#inventories-1)
  - [playbooks/](#playbooks-1)
    - [site.yaml](#siteyaml)
    - [bootstrap.yaml](#bootstrapyaml)
    - [hardening.yaml](#hardeningyaml)
    - [kubernetes.yaml](#kubernetesyaml)
    - [ai-nodes.yaml](#ai-nodesyaml)
    - [break-glass.yaml](#break-glassyaml)
  - [collections/](#collections-1)
    - [ansible_collections/.gitkeep](#ansible_collectionsgitkeep)
  - [policies/](#policies-1)
    - [ansible/](#ansible)
    - [README.md](#readmemd-1)
  - [ci/](#ci-1)
    - [ansible-lint.yml](#ansible-lintyml)
    - [ansible-check.yml](#ansible-checkyml)
    - [compliance-report.yml](#compliance-reportyml)
  - [scripts/](#scripts-1)
    - [bootstrap.sh](#bootstrapsh)
    - [run-playbook.sh](#run-playbooksh)
    - [verify.sh](#verifysh)
    - [architecture_bootstrap.sh](#architecture_bootstrapsh)
  - [docs/](#docs-1)
    - [architecture.md](#architecturemd)
    - [threat-model.md](#threat-modelmd)
    - [hardening-baseline.md](#hardening-baselinemd)
    - [kubernetes-security.md](#kubernetes-securitymd)
    - [ai-node-security.md](#ai-node-securitymd)
    - [break-glass.md](#break-glassmd)
    - [compliance-mapping.md](#compliance-mappingmd)
    - [repository-structure.md](#repository-structuremd)
  - [roles/](#roles-1)
    - [common/](#common)
    - [os-hardening/](#os-hardening)
    - [ssh-hardening/](#ssh-hardening)
    - [auditd/](#auditd)
    - [logging/](#logging)
    - [firewall/](#firewall)
    - [selinux/](#selinux)
    - [container-runtime-hardening/](#container-runtime-hardening)
    - [kubernetes-node-hardening/](#kubernetes-node-hardening)
    - [gpu-hardening/](#gpu-hardening)
    - [compliance/](#compliance)
    - [break-glass/](#break-glass)

---

# Ansible Hardening Repository Structure

This document describes the **repository structure as an architectural solution**, not just a file list.

It explains:  
– The purpose of each layer  
– Where enforcement occurs versus declaration  
– How the repository supports the Sovereign AI DevSecOps model  

The document builds on `docs/architecture.md` and serves as a navigation map for `docs/README.md`.

---

# Architectural Logic of the Structure

The repository is built on the principle of **separation of responsibilities and trust layers**.

Key structural ideas:  
– Configuration ≠ Security  
– Declaration ≠ Enforcement  
– Documentation = Part of Security  
– CI = Mandatory participant in the lifecycle  

The structure reflects the path:  
**policy → control → application → verification → evidence**

---

# Repository Root

The root contains only elements related to:  
– Managing Ansible as a platform  
– Quality and reproducibility  
– CI integration  

No hardening logic is present here — only its management.

DevSecOps rationale:  
– Single entry point  
– Minimization of accidental changes  
– Standardized execution and verification

## inventories/

Layer describing **environments and node roles**.

Responsibilities:  
– Separation of dev / stage / prod  
– No hardening logic  
– Data only, no behavior  

DevSecOps rationale:  
– Environment does not affect the security model  
– Differences are isolated and explicit  
– Minimization of environment-specific drift  

## playbooks/

Layer of **executable scenarios**.

Responsibilities:  
– Formalize application flows  
– Fix order and dependencies  
– Eliminate “manual role combinations”  

DevSecOps rationale:  
– Scenarios instead of ad-hoc runs  
– Reproducibility of actions  
– Control of blast-radius

## roles/

Main layer of **security enforcement**.

Responsibilities:  
– Implement specific controls  
– Isolate areas of responsibility  
– Ensure repeatability and idempotence  

Each role:  
– Covers a specific threat class  
– Maps to the hardening baseline  
– Contributes to compliance  

DevSecOps rationale:  
– Security as code  
– Reusable controls  
– Verifiability and auditability  

## collections/

Ansible extension layer.

Responsibilities:  
– Isolate external dependencies  
– Control supply chain  
– Ensure environment reproducibility  

DevSecOps rationale:  
– Protection from implicit dependencies  
– Deterministic builds

## policies/

**Policy-as-code layer**

Responsibilities:  
– Prohibit unsafe states  
– Formalize mandatory requirements  
– Automatic validation  

DevSecOps rationale:  
– Security before execution  
– Prevent errors, not react  
– Enforcement without human intervention  

## ci/

**Security proof layer**

Responsibilities:  
– Automatic verification  
– Detect violations  
– Generate compliance reports  

DevSecOps rationale:  
– CI as security gate  
– No trust in manual actions  
– Compliance as outcome, not declaration

## scripts/

Auxiliary utilities layer.

Responsibilities:  
– Standardize execution  
– Prevent erroneous commands  
– Simplify bootstrap and validation  

DevSecOps rationale:  
– Reduce human factor  
– Ensure consistent behavior locally and in CI  

## docs/

**Architectural knowledge layer**

Responsibilities:  
– Document threat model  
– Describe security architecture  
– Link controls to compliance  

DevSecOps rationale:  
– Documentation as part of security  
– Reproducibility of understanding  
– Audit of architecture, not just code

---

# Purpose of Directories and Files

## README.md
Main entry point of the repository.  
Contains positioning, scope, non-goals, and links to docs/.  
DevSecOps rationale: fast understanding of architecture and security policy.

## ansible.cfg
Ansible configuration file for the repository.  
Defines playbook behavior, role paths, and linting.  
DevSecOps rationale: uniform execution and enforcement control.

## requirements.yml
Specifies external Ansible collections and dependencies.  
DevSecOps rationale: environment reproducibility and supply chain control.

## .ansible-lint
Configuration for ansible-lint.  
DevSecOps rationale: enforce quality and security standards for roles.

## .yamllint
YAML-lint configuration.  
DevSecOps rationale: ensure correct syntax and prevent errors.

## .gitignore
Excludes temporary and sensitive files from git.  
DevSecOps rationale: prevent secret leaks and repository clutter.

---

## inventories/
Contains descriptions of all environments: dev, stage, prod.  
Each subdirectory includes:
- hosts.yaml — list of nodes  
- group_vars/all.yaml — shared variables  
- host_vars/.gitkeep — per-node variables  

DevSecOps rationale:
- environment isolation  
- no manually managed configurations  
- minimize drift via Ansible

## playbooks/
Contains Ansible playbooks for managing the lifecycle of nodes and the platform.

### site.yaml
Main playbook, orchestrates all other scenarios.  
DevSecOps rationale: single entry point for full hardening.

### bootstrap.yaml
Initializes a new node, installs dependencies, prepares environment.  
DevSecOps rationale: guaranteed Day-0 reproducibility.

### hardening.yaml
Applies OS and runtime hardening to the node.  
DevSecOps rationale: enforces security baseline without side-effects.

### kubernetes.yaml
Configures and hardens a Kubernetes node.  
DevSecOps rationale: enforces kubelet and node-level security.

### ai-nodes.yaml
Hardens AI/GPU nodes.  
DevSecOps rationale: isolation, runtime restrictions, data exfiltration prevention.

### break-glass.yaml
Playbook for controlled emergency access.  
DevSecOps rationale: enforce temporary access with full logging and automatic revocation.

---

## collections/
Holds local Ansible collections or placeholders for CI/CD.

### ansible_collections/.gitkeep
Ensures the empty directory is preserved in Git.  
DevSecOps rationale: maintain repository structure and reproducibility.

---

## policies/
Policy-as-code layer for configuration enforcement.

### ansible/
Contains OPA/Rego rules for enforcement.

- mandatory-hardening.rego — enforces required security controls  
- forbidden-configs.rego — forbids unsafe configurations  

### README.md
Describes purpose and principles of policy-as-code.  
DevSecOps rationale: transparent, formalized security management.

---

## ci/
Contains CI configurations for verifying Ansible code.

### ansible-lint.yml
Runs ansible-lint in the pipeline.  
DevSecOps rationale: automatic enforcement of coding and security standards.

### ansible-check.yml
Validates playbooks for idempotence and potential drift.  
DevSecOps rationale: proof that changes are safe and predictable.

### compliance-report.yml
Generates reports on baseline and standards compliance.  
DevSecOps rationale: CI as the primary audit-ready compliance tool.

---

## scripts/
Helper utilities for node bootstrap and validation.

### bootstrap.sh
Prepares a node before running Ansible.  
DevSecOps rationale: ensures consistent Day-0 initialization.

### run-playbook.sh
Unified execution of playbooks with environment parameters.  
DevSecOps rationale: reproducible, controlled enforcement.

### verify.sh
Validates node state after hardening.  
DevSecOps rationale: detects drift and confirms baseline compliance.

### architecture_bootstrap.sh
Initializes repository structure and scaffolding.  
DevSecOps rationale: enforces standardized project layout.

---

## docs/
Holds documentation capturing architecture, threats, controls, and compliance.

### architecture.md
Describes the role of Ansible-hardening in Sovereign AI, trust boundaries, control/data plane, enforcement vs declaration, and Day-0/1/2 model.  
DevSecOps rationale: conceptual map, entry point for all security decisions.

### threat-model.md
Documents attacker models, AI-specific threats, lateral movement, and supply chain attacks.  
DevSecOps rationale: justifies controls and prioritizes mitigations.

### hardening-baseline.md
Defines the “hardened host”, OS and runtime controls, immutable zones, and drift requirements.  
DevSecOps rationale: foundational security layer for all nodes.

### kubernetes-security.md
Extends the baseline to Kubernetes nodes, kubelet hardening, node-level controls, integration with admission controllers, seccomp, and AppArmor.  
DevSecOps rationale: protection of Kubernetes control and data planes.

### ai-node-security.md
Describes GPU isolation, runtime restrictions for AI workloads, multi-tenant threats, and data exfiltration prevention.  
DevSecOps rationale: securing AI computation and data.

### break-glass.md
Defines emergency access policy, logging, access revocation, and post-incident review.  
DevSecOps rationale: controlled exception with full auditability.

### compliance-mapping.md
Maps security controls to CIS/NIST/ISO standards, automation, CI checks, and external audits.  
DevSecOps rationale: proof of compliance with standards and threat model.

### repository-structure.md
Documents directory and file purposes, enforcement layers, policy, docs, and role extension rules.  
DevSecOps rationale: navigation, reproducibility, and drift minimization.

---

## roles/
Primary enforcement layer implementing all security controls.

### common/
Base role with shared settings and helper tasks.  
- defaults/main.yaml — common variables  
- handlers/main.yaml — shared handlers  
- tasks/main.yaml — base tasks  
- templates/.gitkeep / files/.gitkeep — empty directories for structure  

DevSecOps rationale: minimize repetition and provide a single extension point for all roles.

### os-hardening/
Role for OS hardening.  
- defaults/main.yaml — general variables  
- tasks/kernel.yaml — kernel hardening  
- tasks/sysctl.yaml — sysctl configuration  
- tasks/filesystem.yaml — filesystem setup  
- templates/sysctl.conf.j2 — sysctl template  
- handlers/main.yaml — handlers  

DevSecOps rationale: enforce a secure OS foundation for all other layers.

### ssh-hardening/
SSH security control.  
- defaults/main.yaml — common variables  
- tasks/main.yaml — configure sshd  
- templates/sshd_config.j2 — configuration template  
- handlers/main.yaml — restart service on changes  

DevSecOps rationale: prevent remote attacks and enforce access policies.

### auditd/
Auditd configuration.  
- defaults/main.yaml — common variables  
- tasks/main.yaml — audit rules  
- templates/audit.rules.j2 — rules template  
- handlers/main.yaml — reload auditd  

DevSecOps rationale: mandatory host auditing, full traceability.

### logging/
Centralized logging management.  
- defaults/main.yaml — common variables  
- tasks/main.yaml — configure rsyslog  
- templates/rsyslog.conf.j2 — configuration template  
- handlers/main.yaml — restart rsyslog  

DevSecOps rationale: protect logs and ensure centralized availability.

### firewall/
Manage iptables/nftables rules.  
- defaults/main.yaml — common variables  
- tasks/main.yaml — firewall rules  

DevSecOps rationale: control network traffic and minimize attack surface.

### selinux/
Configure SELinux.  
- defaults/main.yaml — common variables  
- tasks/main.yaml — enforce policy  

DevSecOps rationale: mandatory access control for OS and applications.

### container-runtime-hardening/
Container runtime hardening.  
- defaults/main.yaml — common variables  
- tasks/main.yaml — configure daemon.json  
- templates/daemon.json.j2 — configuration template  
- handlers/main.yaml — restart runtime  

DevSecOps rationale: secure runtime for AI workloads.

### kubernetes-node-hardening/
Kubernetes node hardening.  
- defaults/main.yaml — common variables  
- tasks/kubelet.yaml — kubelet configuration  
- tasks/cni.yaml — CNI security  
- tasks/main.yaml — orchestrates kubelet and CNI, ensures idempotent application of all node-level security controls  
- templates/kubelet-config.yaml.j2 — kubelet template  
- handlers/main.yaml — reload services  

DevSecOps rationale: node-level security, control of data/control plane.

### gpu-hardening/
GPU node hardening.  
- defaults/main.yaml — common variables  
- tasks/main.yaml — runtime restrictions  
- handlers/main.yaml — if needed  
- files/.gitkeep — placeholder  

DevSecOps rationale: AI workload security, isolation, multi-tenant protection.

### compliance/
Standards compliance automation.  
- tasks/cis.yaml — CIS controls  
- tasks/nist.yaml — NIST controls  
- tasks/main.yaml — orchestrates CIS and NIST tasks, centralized compliance enforcement  
- reports/.gitkeep — report generation  
- defaults/main.yaml — variables  

DevSecOps rationale: compliance as first-class citizen, audit-ready.

### break-glass/
Emergency access implementation.  
- tasks/main.yaml — temporary privilege assignment  
- templates/sudoers.j2 — sudoers template  
- defaults/main.yaml — time-window settings  

DevSecOps rationale: controlled exception, full audit, enforce policy.
