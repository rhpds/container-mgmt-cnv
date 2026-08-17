# Module Outline (spec/modules/module-01-console-overview.md)

### Brief Overview
This opening module orients the audience to the OpenShift web console before the demo moves into container-management-specific workflows. It gives platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift a shared baseline of console navigation so that later modules (deploying applications, security, day-2 operations, observability, ACM, ACS) can be followed without pausing to explain basic UI orientation. Per the design spec, this module covers generic OpenShift console navigation rather than content unique to container management — it is scoped as a short warm-up ahead of the container-management-specific modules that follow.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows general container/Kubernetes fundamentals (pods, deployments, namespaces) and basic cluster operations concepts, but not OpenShift-specific console workflows.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. This is a presenter-led demo; prerequisites are assumed, not verified.
- **Estimated duration:** 10 min

### Learning Objectives
- Orient participants to the OpenShift web console as the entry point for the rest of the demo.
- Establish familiarity with core console navigation ahead of the container-management-specific workflows covered in Modules 2-7.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Console orientation | 10 min |

### Detailed Steps
1. Presenter logs into the OpenShift web console on the pre-provisioned OpenShift 4.22 (CNV-based) cluster.
2. Presenter walks the audience through the console's main navigation areas.
3. Presenter frames the rest of the demo, previewing that subsequent modules will build on this console starting point to show container-management capabilities (deploying applications, security by default, day-2 operations, observability, multi-cluster management, and security scanning).

### Key Takeaways
- The OpenShift web console is the common starting point used throughout the rest of the demo.

### Infrastructure Notes
Per the design spec's flag on the Module Map: this module is generic OpenShift navigation rather than container-management-specific. It is a candidate for trimming if the overall demo duration (~84 min total) needs to be reduced.
