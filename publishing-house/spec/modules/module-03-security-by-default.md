# Module Outline (spec/modules/module-03-security-by-default.md)

### Brief Overview
This module demonstrates OpenShift's secure-by-default posture, a key differentiator for platform engineers and infrastructure architects evaluating container-management platforms. The presenter walks through OpenShift's built-in Security Context Constraints (SCCs), RBAC, and NetworkPolicies to show that meaningful security controls are enforced out of the box, rather than requiring extensive manual configuration.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows general container/Kubernetes fundamentals but not OpenShift's specific security defaults.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required.
- **Estimated duration:** 10 min

### Learning Objectives
- Demonstrate OpenShift's secure-by-default posture using Security Context Constraints.
- Demonstrate OpenShift's secure-by-default posture using RBAC.
- Demonstrate OpenShift's secure-by-default posture using NetworkPolicies.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Security Context Constraints | 4 min |
| 2       | RBAC | 3 min |
| 3       | NetworkPolicies | 3 min |

### Detailed Steps
1. Presenter explains the concept of secure-by-default in the context of OpenShift as a container-management platform.
2. Presenter demonstrates Security Context Constraints (SCCs) enforcing pod security restrictions by default.
3. Presenter demonstrates RBAC controlling access within the cluster.
4. Presenter demonstrates NetworkPolicies restricting network traffic between workloads.
5. Presenter ties the three mechanisms together as evidence of OpenShift's out-of-the-box security posture.

### Key Takeaways
- OpenShift enforces meaningful security controls by default via SCCs, RBAC, and NetworkPolicies.
- These defaults reduce the security configuration burden compared to a bare Kubernetes distribution.

### Infrastructure Notes
None beyond the standard pre-provisioned OpenShift 4.22 (CNV-based) cluster environment.
