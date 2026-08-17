# Module Outline (spec/modules/module-02-deploying-applications.md)

### Brief Overview
This module demonstrates that OpenShift can run third-party software and existing applications as a general container-management platform, not only applications built specifically for it. The presenter deploys Microsoft SQL Server (third-party software, deployed via Helm) and Online Boutique (an existing application), showing both without relying on application-platform-specific features. This directly supports the demo's positioning of OpenShift for audiences evaluating it specifically as a container-management platform.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows general container/Kubernetes fundamentals (pods, deployments, namespaces).
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required. A SQL Server Helm chart repository is pre-staged in the environment.
- **Estimated duration:** 12 min

### Learning Objectives
- Deploy third-party software (Microsoft SQL Server) on OpenShift via Helm.
- Deploy an existing application (Online Boutique) on OpenShift.
- Demonstrate that both deployments work without relying on application-platform-specific features.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Deploy third-party software: SQL Server via Helm | 6 min |
| 2       | Deploy an existing application: Online Boutique | 6 min |

### Detailed Steps
1. Presenter introduces the goal: showing OpenShift running software it wasn't purpose-built for, using the pre-staged SQL Server Helm chart repository.
2. Presenter deploys Microsoft SQL Server on the cluster via its Helm chart.
3. Presenter confirms the SQL Server deployment is running on OpenShift.
4. Presenter introduces Online Boutique as an existing application being deployed on OpenShift.
5. Presenter deploys Online Boutique on the cluster.
6. Presenter confirms the Online Boutique deployment is running, reinforcing that neither deployment required application-platform-specific features.

### Key Takeaways
- OpenShift can host third-party software (e.g., SQL Server) via standard packaging mechanisms like Helm.
- OpenShift can run existing applications (e.g., Online Boutique) without modification for an application platform.
- These capabilities support OpenShift's role as a general container-management platform, not just an application platform.

### Infrastructure Notes
Requires a pre-staged SQL Server Helm chart repository and access, plus the Online Boutique application manifests/images, as part of the environment automation.
