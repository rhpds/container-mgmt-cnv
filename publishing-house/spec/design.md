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
3. Perform day-2 cluster operations, including an over-the-air upgrade, and observe self-healing/reconciliation behavior.
4. Monitor cluster health and application performance using built-in observability tooling (metrics, LokiStack logging, troubleshooting panel).
5. Manage multiple clusters — including a non-OpenShift cluster — from a single hub using Red Hat Advanced Cluster Management.
6. Secure the cluster and its workloads using Red Hat Advanced Cluster Security (vulnerability scanning, compliance, risk profiling).

<!-- 6 objectives for ~70-75 min of content across 7 modules — see note on duration under Module Map. -->

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
- Microsoft SQL Server (third-party software, deployed via Helm — used as the "deploy third-party software" example, not a Red Hat product)

## Module Map

| Module | Title | Duration |
|--------|-------|----------|
| 1 | Console Overview | 10 min |
| 2 | Deploying Third-Party & Existing Applications | 12 min |
| 3 | Security by Default | 10 min |
| 4 | Day-2 Cluster Operations | 8 min |
| 5 | Observability | 8 min |
| 6 | Multi-Cluster Management with ACM | 12 min |
| 7 | Securing the Cluster with ACS | 10 min |
| — | **Total hands-on/demo** | **70 min** |
| — | Intro / presentation | ~10 min |
| — | **Total demo** | **~80 min** |

<!-- NOTE: This exceeds the typical 15-45 min "demo" guideline. Modules 1-5 reuse an existing Showroom repo (mpbravo/showroom_container-management-demo) that already has real, developed content at this scope. Modules 6-7 (ACM, ACS) are net-new. Flagging for the author: either accept this as an extended/roadshow-style demo (consistent with its `security-roadshow-cnv` base CI, which runs similarly long), or trim Module 1 (Console Overview) since it's generic OpenShift navigation rather than container-management-specific. -->

## Difficulty Level

Intermediate

## Environment

**Learner view:** A live, pre-provisioned OpenShift 4.22 cluster (CNV-based) with Red Hat Advanced Cluster Management, Red Hat Advanced Cluster Security, OpenShift GitOps, OpenShift Pipelines, Quay, and Keycloak already installed. A SQL Server Helm chart repository and a sample vulnerable demo VM are pre-staged for the ACS scanning walkthrough. A second, non-OpenShift cluster is available as an ACM-managed spoke.

**Automation needed:** Yes

- Provision the OpenShift hub cluster with ACM, ACS, GitOps, Pipelines, and Quay pre-installed (based on the `agd_v2/security-roadshow-cnv` catalog item)
- Stage the SQL Server Helm repository access and demo data
- Deploy the ACS demo target VM (vulnerability-scan scenario)
- Provision a non-OpenShift spoke cluster and wire it into ACM via auto-import (net-new automation — not yet built; see Infrastructure Requirements)

## Infrastructure Requirements

- **Cloud provider:** TBD — confirmed in infrastructure phase
- **Cluster type:** TBD — confirmed in infrastructure phase
- **OCP version:** TBD — confirmed in infrastructure phase
- **Topology:** TBD — confirmed in infrastructure phase
- **Sizing:** TBD — confirmed in infrastructure phase
- **Automation approach:** TBD — confirmed in infrastructure phase
- **AI/MaaS:** TBD — confirmed in infrastructure phase
- **External services:** TBD — confirmed in infrastructure phase
- **AAP version:** TBD — confirmed in infrastructure phase
- **Non-GA products:** TBD — confirmed in infrastructure phase

## Assessment Strategy (Optional)

Trust-based — this is a presenter-led demo with no automated learner validation.
