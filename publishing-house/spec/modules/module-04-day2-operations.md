# Module Outline (spec/modules/module-04-day2-operations.md)

### Brief Overview
This module shows OpenShift's day-2 cluster operations capabilities, which matter to platform engineers and infrastructure architects responsible for ongoing cluster lifecycle management. The presenter performs a live over-the-air cluster upgrade and demonstrates the cluster's self-healing and reconciliation behavior, illustrating that OpenShift manages ongoing operational concerns beyond initial deployment.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows basic cluster operations concepts.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts and basic cluster operations concepts. Presenter-led; no learner setup required.
- **Estimated duration:** 10 min

### Learning Objectives
- Implement a day-2 cluster operation: an over-the-air cluster upgrade.
- Observe self-healing/reconciliation behavior on the cluster.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Over-the-air cluster upgrade | 5 min |
| 2       | Self-healing and reconciliation | 5 min |

### Detailed Steps
1. Presenter introduces day-2 operations as an ongoing responsibility beyond initial cluster deployment.
2. Presenter performs a live over-the-air cluster upgrade on the OpenShift cluster.
3. Presenter narrates the upgrade progress as it runs.
4. Presenter demonstrates the cluster's self-healing/reconciliation behavior.
5. Presenter observes and highlights the cluster reconciling back to its declared state.

### Key Takeaways
- OpenShift supports live, over-the-air cluster upgrades as a built-in day-2 operation.
- OpenShift automatically self-heals and reconciles cluster state, reducing manual operational burden.

### Infrastructure Notes
Requires a cluster configuration that supports a live, observable over-the-air upgrade during the demo window.
