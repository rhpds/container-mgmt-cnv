# Module Outline (spec/modules/module-07-securing-acs.md)

### Brief Overview
This closing module shows Red Hat Advanced Cluster Security (ACS) scanning and securing the environment, addressing the audience's other key knowledge gap: ACS's capabilities for cluster and workload security. The presenter uses a pre-staged, sample vulnerable demo VM to walk through ACS's scanning capabilities.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience does not yet know ACS's capabilities.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required. A sample vulnerable demo VM must be pre-staged for the scanning walkthrough.
- **Estimated duration:** 10 min

### Learning Objectives
- Secure the cluster and its workloads using Red Hat Advanced Cluster Security.
- Perform vulnerability scanning using ACS.
- Review compliance and risk profiling using ACS.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Vulnerability scanning | 4 min |
| 2       | Compliance | 3 min |
| 3       | Risk profiling | 3 min |

### Detailed Steps
1. Presenter introduces Red Hat Advanced Cluster Security as the tool for securing the cluster and its workloads.
2. Presenter runs a vulnerability scan against the pre-staged, sample vulnerable demo VM.
3. Presenter reviews the scan results in ACS.
4. Presenter reviews ACS's compliance reporting.
5. Presenter reviews ACS's risk profiling of the environment.
6. Presenter closes by tying ACS's findings back to the demo's overall container-management security narrative.

### Key Takeaways
- ACS scans workloads and the cluster for vulnerabilities.
- ACS provides compliance reporting and risk profiling.
- ACS complements OpenShift's secure-by-default posture (Module 3) with active scanning and risk management.

### Infrastructure Notes
Requires the ACS demo target VM (vulnerability-scan scenario) to be deployed and pre-staged in the environment.
