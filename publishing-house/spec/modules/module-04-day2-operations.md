# Module Outline (spec/modules/module-04-day2-operations.md)

**Module Title:** Cluster Operations

### Brief Overview
This module shows OpenShift's ongoing cluster-operations capabilities, which matter to platform engineers and infrastructure architects responsible for lifecycle management. Following the real Showroom content, the presenter walks through the two-phase over-the-air (OTA) update process and update channels, demonstrates continuous reconciliation by deliberately breaking and watching OpenShift repair a core console component and by inspecting host-level `MachineConfig` reconciliation, and uses OpenShift Lightspeed's AI assistant to diagnose three common workload failure scenarios.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows basic cluster operations concepts.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts and basic cluster operations concepts. Presenter-led; no learner setup required.
- **Estimated duration:** 10 min

### Learning Objectives
- Explain the two-phase OTA update process (Cluster Version Operator payload rollout by Runlevel, then Machine Config Operator cordon/drain/reboot/uncordon per node) and how update channels (fast/stable/EUS/candidate) govern rollout timing.
- Change the update channel from Cluster Settings and explain what changes — and what doesn't — on a Single Node OpenShift cluster versus a multi-node cluster.
- Demonstrate OpenShift's continuous reconciliation model by deleting and observing automatic recovery of a core platform Deployment.
- Explain how `MachineConfig` objects extend the same declarative reconciliation model down to host-level OS configuration.
- Use OpenShift Lightspeed's natural-language assistant to diagnose `CrashLoopBackOff`, `ImagePullBackOff`, and misconfigured liveness-probe failures.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Full-Stack Over-The-Air (OTA) Upgrades | 4 min |
| 2       | Self-Healing by Design - Continuous Reconciliation | 3 min |
| 3       | Intelligent Cluster Troubleshooting - OpenShift Lightspeed | 3 min |

### Detailed Steps
1. Explain OTA updates as one supported, versioned operation covering both Red Hat Enterprise Linux CoreOS (RHCOS) and OpenShift components together, rather than separately tracking OS patch level, container runtime, CNI plugin, and orchestrator versions.
2. Walk through the two update phases: the Cluster Version Operator (CVO) queries the OpenShift Update Service for a valid path, retrieves the release image, and applies it Runlevel by Runlevel, requiring every ClusterOperator to report `Available=True`/`Degraded=False` before proceeding; then the Machine Config Operator (MCO) cordons, drains, updates, reboots, and uncordons each node in turn.
3. Note the Single Node OpenShift caveat: with only one node there is nowhere to reschedule pods during the reboot, so the environment is briefly interrupted — but the validated update graph and per-Operator health gating are identical to a multi-node cluster, which gets the same mechanism with zero workload downtime. Mention that disconnected environments can mirror update graphs locally via a local OpenShift Update Service instance.
4. Log in as a user with the `cluster-admin` role, go to `Administration` → `Cluster Settings`, and point out the `Update Status`, current version, and channel directly on the page. Explain the four channel types using 4.21 as an example: `fast-4.21` (GA releases immediately), `stable-4.21` (delayed for regression analysis, the default for new clusters), `eus-4.y` (Extended Update Support for even-numbered minor versions), and `candidate-4.21` (unsupported early access, testing only). Change the `Update Channel` selector and click `Save` — without triggering a live update, since a real update exceeds a typical demo slot.
5. Introduce reconciliation as a pattern repeated at every layer of the stack: a `Deployment`'s `ReplicaSet` controller for user workloads (as already seen with the Crunchy Postgres pod deletion in Module 2), every `ClusterOperator` under `Administration` → `Cluster Settings` → `ClusterOperators` for platform components, the Machine Config Operator for host OS configuration, and — on multi-node, cloud-backed clusters only — the Machine API (`MachineSet`) for compute infrastructure (not observable on this single-node environment).
6. Live demo: open a terminal and run `oc get deployment console -n openshift-console`, then delete it with `oc delete deployment console -n openshift-console`. Note the web console may become briefly unreachable on refresh. Check `oc get clusteroperator console` and, within roughly 30-60 seconds, confirm the Console Operator has recreated the Deployment automatically and `AVAILABLE` reports `True` again via `oc get deployment console -n openshift-console` — with no manual `oc apply` and no on-call escalation. This is safe to repeat.
7. Show host-configuration reconciliation: run `oc get machineconfigpool` (the single node belongs to both the `master` and `worker` pools) and `oc get machineconfig | grep rendered`. Explain — without executing live, since it triggers a full node reboot — how a `MachineConfig` such as `99-demo-chrony` would add an NTP server, kernel argument, or trusted CA to every node, applied via the exact same cordon-drain-reboot-uncordon mechanics used for the OTA upgrade, just triggered by a config change instead of a new release.
8. Introduce OpenShift Lightspeed as a generative-AI assistant embedded in the console, using Insights Operator data for proactive issue identification, Deployment Validation Operator integration for workload configuration insights, and a Home Overview status card. Note that it does not provide its own LLM — an external provider (OpenAI, Azure OpenAI, or IBM watsonx) must be configured before installing the Operator.
9. Create project `demo-lightspeed`. Deploy the `bad-app` pod (image `busybox`, command `echo starting && exit 1`), observe it enter `CrashLoopBackOff`, and ask Lightspeed "My pod bad-app is in CrashLoopBackOff, what's wrong?" — provide logs when prompted, apply its suggested fix, and retry.
10. Deploy the `broken-deploy` Deployment referencing a nonexistent image (`myregistry.internal/nonexistent:v99`), confirm the pod fails in the `Pods` page, and ask Lightspeed "Why can't my deployment broken-deploy start?" to see it identify the `ImagePullBackOff`.
11. Deploy the `probe-fail` Deployment (an nginx image whose liveness probe checks port 8080 instead of the actual port 80), confirm it keeps restarting, and ask Lightspeed "My nginx deployment keeps restarting but the image is fine, why?" to show its deeper diagnostic reasoning on a subtler misconfiguration.

### Key Takeaways
- OTA upgrades bundle RHCOS and OpenShift into one tested, versioned, rollback-capable artifact and update path instead of separately tracked OS/runtime/CNI/orchestrator components.
- Reconciliation control loops operate at every layer of the stack — workloads, platform Operators, and host OS configuration (and compute infrastructure on multi-node/cloud clusters) — so a wide class of failures self-heals automatically without a human re-running a script.
- OpenShift Lightspeed turns common failure signatures (`CrashLoopBackOff`, `ImagePullBackOff`, misconfigured liveness probes) into natural-language, actionable diagnoses directly inside the console, and requires an external LLM provider to be configured first.

### Infrastructure Notes
Demonstrations assume a Single Node OpenShift (SNO) cluster — the reboot-related caveats called out for both the OTA upgrade channel change and the `MachineConfig` example apply specifically because there is no second node to absorb workloads. OpenShift Lightspeed requires an external LLM provider (OpenAI, Azure OpenAI, or IBM watsonx) to be configured before the Operator is installed.
