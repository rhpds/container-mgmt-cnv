# Module Outline (spec/modules/module-06-multicluster-acm.md)

### Brief Overview
This module shows Red Hat Advanced Cluster Management (ACM) managing multiple clusters — including a non-OpenShift cluster — from a single hub. It directly addresses one of the audience's key knowledge gaps: ACM's capabilities for multi-cluster management, which matters to platform engineers and infrastructure architects operating heterogeneous fleets.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience does not yet know ACM's capabilities for multi-cluster management.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required. A second, non-OpenShift cluster must be available as an ACM-managed spoke.
- **Estimated duration:** 12 min

### Learning Objectives
- Manage multiple clusters from a single ACM hub.
- Manage a non-OpenShift cluster alongside an OpenShift cluster from the same hub.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | ACM hub overview | 4 min |
| 2       | Managing the OpenShift spoke cluster | 4 min |
| 3       | Managing the non-OpenShift spoke cluster | 4 min |

### Detailed Steps
1. Presenter introduces Red Hat Advanced Cluster Management as the single pane of glass for multi-cluster management.
2. Presenter shows the ACM hub view listing managed clusters.
3. Presenter demonstrates managing the OpenShift cluster from the ACM hub.
4. Presenter demonstrates managing the non-OpenShift cluster from the same ACM hub.
5. Presenter highlights that both cluster types are managed from a single hub.

### Key Takeaways
- ACM provides a single pane of glass for managing multiple clusters.
- ACM can manage non-OpenShift clusters alongside OpenShift clusters from the same hub.

### Infrastructure Notes
Requires a second, non-OpenShift spoke cluster provisioned and wired into ACM via auto-import. Per the design spec, this is net-new automation not yet built as of the design phase.
