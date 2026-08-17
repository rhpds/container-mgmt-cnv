# Module Outline (spec/modules/module-05-observability.md)

### Brief Overview
This module reviews OpenShift's built-in observability tooling, showing platform engineers and infrastructure architects how to monitor cluster health and application performance without bolting on separate tooling. The presenter walks through metrics, LokiStack-based logging, and the troubleshooting panel.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required.
- **Estimated duration:** 10 min

### Learning Objectives
- Monitor cluster health and application performance using built-in metrics.
- Review application and cluster logs using LokiStack logging.
- Use the built-in troubleshooting panel to investigate issues.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Metrics | 4 min |
| 2       | LokiStack logging | 3 min |
| 3       | Troubleshooting panel | 3 min |

### Detailed Steps
1. Presenter introduces OpenShift's built-in observability tooling as a core container-management capability.
2. Presenter reviews cluster health and application performance metrics.
3. Presenter reviews logs via LokiStack logging.
4. Presenter demonstrates the troubleshooting panel to investigate cluster or application issues.
5. Presenter summarizes how these built-in tools reduce the need for separate observability tooling.

### Key Takeaways
- OpenShift includes built-in metrics for cluster and application health.
- LokiStack provides built-in logging without external tooling.
- The troubleshooting panel gives a built-in path for investigating issues.

### Infrastructure Notes
Requires LokiStack logging to be installed and configured on the pre-provisioned cluster.
