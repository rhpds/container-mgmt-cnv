# Container Management Standard Demo

<!-- This file is the design document for your lab or demo. -->
<!-- Fill in each section below, or run /rhdp-publishing-house to have the intake skill help. -->
<!-- Sections marked with [brackets] are placeholders — replace with real content. -->
<!-- The validation gate checks for all required sections before submission. -->

## Overview

This demo shows how Red Hat OpenShift handles core container-management responsibilities beyond application-platform features — deploying third-party software, enforcing security by default, running day-2 operations, and managing multiple clusters (including a non-OpenShift cluster) from a single pane of glass. It targets audiences evaluating OpenShift specifically as a container-management platform, not just an application platform. Participants will watch a presenter deploy a third-party application (Microsoft SQL Server via Helm) and an existing application (Online Boutique) on OpenShift, walk through OpenShift's secure-by-default RBAC/SCC/NetworkPolicy posture, perform a live over-the-air cluster upgrade and observe self-healing/reconciliation, review built-in observability (metrics, LokiStack logging, troubleshooting panel), see Red Hat Advanced Cluster Management manage both an OpenShift and a non-OpenShift cluster from a single hub, and see Red Hat Advanced Cluster Security scan and secure the environment.

## Target Audience

- **Role:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management
- **Experience level:** Intermediate
- **What they already know:** General container/Kubernetes fundamentals (pods, deployments, namespaces), basic cluster operations concepts
- **What they don't know:** OpenShift-specific console workflows, OpenShift's built-in security defaults, and ACM/ACS capabilities for multi-cluster management and security

## Prerequisites

- General familiarity with Kubernetes/container concepts
- Can the lab validate these automatically? No — this is a presenter-led demo; prerequisites are assumed, not verified

## Learning Objectives

1. Deploy third-party and existing applications on OpenShift without relying on application-platform-specific features (e.g., SQL Server via Helm, Online Boutique).
2. Demonstrate OpenShift's secure-by-default posture using Security Context Constraints, RBAC, and NetworkPolicies.
3. Implement day-2 cluster operations, including an over-the-air upgrade, and observe self-healing/reconciliation behavior.
4. Monitor cluster health and application performance using built-in observability tooling (metrics, LokiStack logging, troubleshooting panel).
5. Manage multiple clusters — including a non-OpenShift cluster — from a single hub using Red Hat Advanced Cluster Management.
6. Secure the cluster and its workloads using Red Hat Advanced Cluster Security (vulnerability scanning, compliance, risk profiling).

<!-- 6 objectives for ~70-75 min of content across 7 modules — see the duration note below the module table. -->

## Content Type

Demo (presenter-led)

## Products & Technologies

- Red Hat OpenShift Container Platform (4.22)
- Red Hat OpenShift Virtualization (CNV)
- Red Hat Advanced Cluster Management
- Red Hat Advanced Cluster Security
- Red Hat OpenShift GitOps
- Red Hat OpenShift Pipelines
- Red Hat Quay
- Red Hat build of Keycloak
- OpenShift Lightspeed (requires an external LLM provider — OpenAI, Azure OpenAI, or IBM watsonx)
- Cluster Observability Operator (Troubleshooting Panel — some capabilities are Technology Preview)
- Microsoft SQL Server (third-party software, deployed via Helm — used as the "deploy third-party software" example, not a Red Hat product)

## Module Map

| Module | Title | Duration |
|--------|-------|----------|
| 1 | Console Overview | 10 min |
| 2 | Deploying COTS and existing applications | 12 min |
| 3 | Secure by default | 10 min |
| 4 | Cluster Operations | 10 min |
| 5 | Observability - Logging and Monitoring | 10 min |
| 6 | Multi-Cluster Management with ACM | 12 min |
| 7 | Securing the Cluster with ACS | 10 min |

**Total hands-on/demo:** 74 min. Plus ~10 min intro/presentation — **total demo ~84 min.**

<!-- NOTE: This exceeds the typical 15-45 min "demo" guideline. Modules 1-5 reuse an existing Showroom repo (mpbravo/showroom_container-management-demo) that already has real, developed content at this scope. Modules 6-7 (ACM, ACS) are net-new. Flagging for the author: either accept this as an extended/roadshow-style demo (consistent with its `security-roadshow-cnv` base CI, which runs similarly long), or trim Module 1 (Console Overview) since it's generic OpenShift navigation rather than container-management-specific. -->

## Difficulty Level

Intermediate

## Environment

**Learner view:** A live, pre-provisioned OpenShift 4.22 cluster (CNV-based) with Red Hat Advanced Cluster Management, Red Hat Advanced Cluster Security, OpenShift GitOps, OpenShift Pipelines, Quay, and Keycloak already installed. A SQL Server Helm chart repository and a sample vulnerable demo VM are pre-staged for the ACS scanning walkthrough. A second, non-OpenShift cluster is available as an ACM-managed spoke.

**Automation needed:** Yes

- Provision the OpenShift hub cluster with ACM, ACS, GitOps, Pipelines, and Quay pre-installed (based on the `agd_v2/security-roadshow-cnv` catalog item)
- Deploy SQL Server, demo VMs, and any other extra in-cluster workloads via GitOps (Helm + ArgoCD) from a dedicated automation repo — to be created and built separately from this catalog item
- Provision a non-OpenShift spoke cluster and wire it into ACM via auto-import (net-new automation — not yet built; likely GitOps-managed once the automation repo exists — see Infrastructure Requirements)

## Infrastructure Requirements

- **Cloud provider:** CNV
- **Cluster type:** Multinode (1 control plane sized as SNO — 32 vCPU/128GB — plus 3 autoscaled CNV workers at 16 vCPU/32GB each; inherited as-is from the `agd_v2/security-roadshow-cnv` base CI)
- **OCP version:** 4.22
- **Topology:** CNV pool
- **Sizing:** Control plane: 1x (32 vCPU, 128GB RAM). Workers: 3x (16 vCPU, 32GB RAM). Additional headroom needed: ≥10-15 GiB free storage for the Crunchy Postgres demo (Module 2), and a default StorageClass for MinIO-backed LokiStack (Module 5).
- **Automation approach:** Combo — base cluster and operators (ACM, ACS, GitOps, Pipelines, Quay, Keycloak) provisioned via the `agd_v2/security-roadshow-cnv` AgnosticD/Ansible base CI; SQL Server, demo VMs, and any other extra in-cluster workloads deployed via GitOps (Helm + ArgoCD) from a dedicated automation repo — to be created and built separately. Interim state: the real Showroom content currently points Helm chart installs at Pilar's personal GitHub Pages repos (`mpbravo.github.io/helm-charts`, `mpbravo.github.io/online-boutique-helm`) rather than the future automation repo.
- **AI/MaaS:** MaaS (frontier model) — OpenShift Lightspeed (Module 4) requires an external LLM provider. Reusing the `security-roadshow-cnv` base CI's proven pattern: Azure AI backend, with automatic token provisioning/rotation. Justification: OpenShift Lightspeed only supports a small set of proprietary LLM providers (OpenAI, Azure OpenAI, IBM watsonx) — no open-source option is available for this feature.
- **External services:** `github.com` (Showroom content repo, RHACS demo VM init script from `rhpds/rhacs-demo`, and the `ralvares/openshift-security-framework` content Module 3 is adapted from), `mpbravo.github.io` (SQL Server and Online Boutique Helm chart repos — interim, pending the dedicated automation repo), and the Azure AI endpoint for OpenShift Lightspeed
- **AAP version:** N/A — Ansible Automation Platform is not in the products list (AgnosticD workload roles are provisioning automation, not a demoed product)
- **Non-GA products:** Cluster Observability Operator's Troubleshooting Panel UI plugins ship some capabilities as Technology Preview (per the real Module 5 content) — access plan: none required, enabled via standard OperatorHub install on OCP 4.22, same as any other operator

## Assessment Strategy (Optional)

Trust-based — this is a presenter-led demo with no automated learner validation.
