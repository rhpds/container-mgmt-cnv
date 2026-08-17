# Module Outline (spec/modules/module-03-security-by-default.md)

**Module Title:** Secure by default

### Brief Overview
This module demonstrates OpenShift's secure-by-default posture, a key differentiator for platform engineers and infrastructure architects evaluating container-management platforms. Following the real Showroom content, the presenter contrasts Security Context Constraints (SCCs) with RBAC using a live "naughty container" demo, creates a project-scoped RoleBinding and verifies isolation via user impersonation, and configures NetworkPolicies to move a namespace from default-allow to default-deny and back to selective allow.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows general container/Kubernetes fundamentals but not OpenShift's specific security defaults.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required.
- **Estimated duration:** 10 min

### Learning Objectives
- Explain how Security Context Constraints (SCCs) govern pod-level host privileges, distinct from RBAC's control over API-object permissions.
- Demonstrate why arbitrary-UID enforcement blocks root-requiring container images by default, and how a certified Red Hat UBI image resolves it.
- Create a project-scoped RoleBinding using the built-in `edit` role and verify namespace isolation using the console's user impersonation feature.
- Configure namespace-scoped NetworkPolicies to move a project from default-allow to default-deny, then selectively re-allow traffic from a specific pod.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Security and Compliance | 3 min |
| 2       | RBAC | 3 min |
| 3       | Introduction to network security | 4 min |

### Detailed Steps
1. Explain the distinction between RBAC and SCCs: RBAC controls who can manipulate API objects (e.g., "can this user create a pod?"), while SCCs control what a pod is allowed to do to the underlying host once it exists (run as root, escalate privileges, use host volumes/networking, etc.).
2. Introduce the three most common default SCCs: `restricted-v2` (applied to nearly all workloads automatically — no root, no host network/volumes, allocated non-root UID), `anyuid` (allows running as UID 0 but still blocks host filesystem access), and `privileged` (full host control, reserved for cluster infrastructure/CNI plugins).
3. Create project `demo-security`. Go to `Workloads` → `Deployments` and create a new Deployment using image `docker.io/httpd:latest` with 1 replica. Show that the container fails to start with `Permission denied: ... could not bind to address [::]:80`, because OpenShift assigns an arbitrary high-numbered non-root UID by default and the Apache image expects to run as root to bind port 80.
4. Show that the common workaround, setting `privileged: true`, does not work — the OpenShift API server rejects the request because the user lacks permission to use that SCC.
5. Change the image to the certified `registry.access.redhat.com/ubi9/httpd-24` (non-root, listens on port 8080 instead of 80) and confirm the pod now runs without issue.
6. Introduce RBAC's three core objects — Rules (permitted verbs on objects), Roles (collections of rules), and Bindings (associations of a user/group/service account to a role) — and the two-level hierarchy of Cluster RBAC (cluster-wide) versus Local RBAC (project-scoped, can reference either a local or cluster role). List the default cluster roles: `cluster-admin`, `admin`, `basic-user`, `cluster-status`, `edit`, and `view`.
7. In the `Core Platform` perspective, go to `Home` → `Projects`, create project `finance-frontend`. In `User Management` → `RoleBindings`, filter to `finance-frontend` and click `Create Binding` to create a namespace-wide RoleBinding named `finance-developer-edit-binding`, binding the built-in `edit` role to subject `User` `dev-bob`.
8. Use the `Impersonate User` feature (top-right user icon) to impersonate `dev-bob`, then open the Project dropdown to show that only `finance-frontend` is visible — all other business and infrastructure namespaces are invisible to that user.
9. Introduce NetworkPolicies: namespace-scoped `NetworkPolicy` objects are additive and shift a project from default-allow to default-deny once any policy is applied; cluster-scoped `AdminNetworkPolicy` (higher priority, Allow/Deny/Pass actions) and `BaselineAdminNetworkPolicy` (cluster-wide fallback) extend this at the administrator level.
10. In project `demo-security`, deploy the `api` Deployment (image `registry.access.redhat.com/ubi9/python-311`, serving on port 8080 via `python3 -m http.server 8080`) and the `api-service` ClusterIP Service, plus two test pods, `client-allowed` and `client-denied` (both `registry.access.redhat.com/ubi9/ubi`, sleeping infinitely).
11. From each test pod's terminal, run `curl -s -o /dev/null -w '%{http_code}\n' http://api-service:8080` and confirm both currently succeed with `200` (no policy yet applied).
12. Create the `default-deny-ingress` NetworkPolicy (Policy type: `Deny all ingress traffic`) under `Networking` → `Network Policies`, then re-run the `curl` commands with `--max-time 3` to show both pods now time out.
13. Create `allow-api-from-allowed`, scoping the pod selector to `app=api` and adding an ingress rule allowing traffic from pods with `role=allowed`. Re-test: `client-allowed` succeeds with `200`, `client-denied` fails/times out.

### Key Takeaways
- SCCs and RBAC are complementary but distinct controls — SCCs are why OpenShift blocks root-requiring containers by default even when a user is otherwise authorized to create pods, and this is a core reason OpenShift is "secure by default" versus vanilla Kubernetes.
- OpenShift's built-in roles (`edit`, `view`, `admin`, `cluster-admin`) and the console's RoleBindings UI let administrators enforce least-privilege project isolation without hand-written RBAC YAML, and impersonation gives an easy, password-free way to verify it actually works.
- NetworkPolicies are opt-in and additive per namespace: a project is fully open by default until any policy is applied, after which it becomes default-deny and only explicitly allowed traffic passes.

### Infrastructure Notes
Demo content and manifests are adapted from `github.com/ralvares/openshift-security-framework` (basic labs `b1` and `b4`). No special environment prerequisites beyond the standard pre-provisioned cluster.
