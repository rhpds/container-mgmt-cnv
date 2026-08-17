# Module Outline (spec/modules/module-01-console-overview.md)

**Module Title:** Console Overview

### Brief Overview
This opening module orients the audience to the OpenShift web console before the demo moves into container-management-specific workflows. It follows the real Showroom content, which walks through eight console areas in sequence: perspectives, the main dashboard, the events page, the software catalog, the topology view, the embedded web terminal, user management, and quick starts. It gives platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift a shared baseline of console navigation so that later modules (deploying applications, security, cluster operations, observability) can be followed without pausing to explain basic UI orientation.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows general container/Kubernetes fundamentals (pods, deployments, namespaces) and basic cluster operations concepts, but not OpenShift-specific console workflows.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. This is a presenter-led demo; prerequisites are assumed, not verified.
- **Estimated duration:** 10 min

### Learning Objectives
- Navigate the OpenShift Console's Unified Perspective model (Core Platform, Virtualization, and Fleet Management areas).
- Interpret the main dashboard's Details, Status, Cluster Inventory, Cluster Utilization, and Activity cards.
- Use the Events page to scope and filter cluster and resource events for troubleshooting.
- Discover and install software (Operators, Helm Charts, S2I) via the Software Catalog.
- Visualize workload relationships (nodes, pods, services, routes) using the Topology view.
- Access cluster resources without local tooling via the embedded web terminal.
- Manage users, groups, identity providers, and RBAC bindings via User Management.
- Use Quick Starts for guided, in-console tutorials.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | The different perspectives in the OpenShift Console | 1 min |
| 2       | The main dashboard | 1 min |
| 3       | The events page | 1.5 min |
| 4       | The software catalog | 1.5 min |
| 5       | The topology view | 1 min |
| 6       | The embedded web terminal | 1.5 min |
| 7       | The user management | 1.5 min |
| 8       | The quick starts | 1 min |

### Detailed Steps
1. Open the *Perspective* dropdown in the upper-right menu. Explain that starting with OpenShift 4.19, the console moved from a dual-perspective model to a Unified Perspective organized into three functional areas: *Core Platform* (the new default, merging Administrator and Developer tools), *Virtualization Perspective* (surfaced when the OpenShift Virtualization Operator is installed), and *Fleet Management Perspective* (the ACM-powered "single pane of glass" for multi-cluster operations). Note that on this single-node demo environment only Core Platform and Virtualization are active — Fleet Management requires ACM and can only be discussed, not navigated live.
2. Walk through the main dashboard's cards: the *Details Card* (Cluster ID, Provider, OpenShift Version, Update Status), the *Status Card* (color-coded health with drill-down to Control Plane/Storage), the *Cluster Inventory Card* (Nodes, Pods, Storage Claims, VMs), the *Cluster Utilization Card* (CPU/Memory/Storage/Network graphs with adjustable time range), and the *Activity Card* (live event feed of pod crashes, node scale-ups, VM migrations).
3. Go to `Home` → `Events`. Show the three ways to scope events (project-level via the Events page, resource-specific via an object's Events tab, and the high-level Home → Overview feed) and the three filters (Resources list, All Types list, Search bar). Explain that events are informative and retained for a maximum of 3 hours in etcd, and that permanent audit/security trails require API Audit Logging forwarded via OpenShift Logging.
4. Go to `Ecosystem` → `Software Catalog`. Show the Operators, Certified Operators, Community Operators, and Other categories, and explain the self-service model (developers provision databases/languages/services without filing infrastructure tickets), standardized Templates/Helm Charts that eliminate configuration "snowflakes," and the broader ecosystem integration (OperatorHub, Helm, Source-to-Image).
5. Go to `Home` → `Topology`. Explain the Nodes/Pods/Services/Routes organization. Once the Online Boutique application is deployed (Module 2), return here to select the `frontend` microservice and show its associated pods, service, and active route; click the route to open the running application. Note the dark-mode contrast limitation on connection lines.
6. Click the terminal icon in the top-right corner to open the embedded web terminal. Highlight that it requires zero local installation (`oc`, `kubectl`, `helm`, `kn`, `tkn`, `jq`, and `git` come pre-installed and stay version-matched to the cluster), uses automatic authentication from the existing console session, is contextually aware of the current project, and provisions an isolated, quota-controlled pod per user session.
7. Go to `Home` → `Security` → `User Management`. Demonstrate identity provider onboarding via `Administration` → `Cluster Settings` → `Configuration` → `OAuth` → `Identity Providers` → `Add`; user impersonation from the Users list to debug another user's view without needing their password; the searchable RoleBindings/ClusterRoleBindings table for visual RBAC mapping; and Group management with LDAP Group Sync status/error visibility.
8. Open Quick Starts via the `(?)` help menu, or via the `Add` page in Topology. Show the real-time UI element highlighting as a task step is started, the built-in verification checks ("Did your build succeed?"), and note that installed Operators (e.g., CrunchyDB, ACM) can inject their own Quick Starts, and that administrators can author custom ones via the `ConsoleQuickStart` resource.

### Key Takeaways
- OpenShift 4.19+ unifies the console into a single perspective organized into Core Platform, Virtualization, and Fleet Management areas, rather than requiring manual perspective switching.
- The dashboard, events page, and topology view give administrators layered visibility — from cluster-wide health, down to individual resource events, down to workload relationships.
- The Software Catalog and Quick Starts turn OpenShift into a self-service platform, letting developers provision software and learn workflows without filing tickets to the infrastructure team.
- The embedded web terminal removes local CLI installation and authentication overhead by running a pre-authenticated, per-session pod inside the browser.
- User Management centralizes identity-provider onboarding, RBAC visibility, and impersonation-based debugging in one console section.

### Infrastructure Notes
The Fleet Management perspective requires Red Hat Advanced Cluster Management and is only discussed, not navigated live, on this single-node environment (see Module 6). The Topology view walkthrough of a real application depends on the Online Boutique application deployed in Module 2.
