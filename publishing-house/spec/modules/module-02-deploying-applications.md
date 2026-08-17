# Module Outline (spec/modules/module-02-deploying-applications.md)

**Module Title:** Deploying COTS and existing applications

### Brief Overview
This module demonstrates that OpenShift can run third-party software and existing applications as a general container-management platform, not only applications built specifically for it. Following the real Showroom content, the presenter installs the Crunchy Postgres for Kubernetes Operator and deploys a self-healing, scalable HA PostgreSQL cluster with pgAdmin; deploys Microsoft SQL Server (third-party software) via a Helm chart; and deploys the existing Online Boutique microservices application via Helm. This directly supports the demo's positioning of OpenShift for audiences evaluating it specifically as a container-management platform.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate — audience already knows general container/Kubernetes fundamentals (pods, deployments, namespaces).
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required. Helm chart repositories for SQL Server and Online Boutique are pre-staged in the environment.
- **Estimated duration:** 12 min

### Learning Objectives
- Install a certified Operator (Crunchy Postgres for Kubernetes) from the Software Catalog and deploy an HA `PostgresCluster`.
- Deploy pgAdmin and connect it to the Operator-managed database using an auto-generated Secret.
- Observe Operator-driven self-healing (automatic failover) and horizontal scaling of a stateful database workload.
- Add a Helm chart repository and deploy third-party software (Microsoft SQL Server) via the Software Catalog.
- Deploy an existing microservices application (Online Boutique) via Helm and confirm it in the Topology view.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Using Operators - Crunchy PostgreSQL | 6 min |
| 2       | Using Helm Charts - SQL Server | 4 min |
| 3       | Deploying existing applications - Online Boutique | 2 min |

### Detailed Steps
1. Create a new project for the Crunchy components (`Home` → `Projects` → `Create Project`). Verify at least 10-15 GiB of free storage is available under `Storage` → `PersistentVolumes` before starting, since the demo creates two Postgres instance volumes, one backup repository volume, and two pgBouncer-related volumes (reduce `storage: 1Gi` to `512Mi` in the YAMLs if storage is tight).
2. In `Ecosystem` → `Software Catalog`, filter by `Operators`, search for `Crunchy`, and install `Crunchy Postgres for Kubernetes` scoped to the new namespace.
3. Click `Postgres Cluster` → `Create instance` and paste the sample `hippo-ha` `PostgresCluster` YAML (2 `pgha1` replicas with pod anti-affinity, a `pgbackrest` backup repo, and a 2-replica `pgBouncer` proxy). Verify the resources are created successfully.
4. From `Installed Operators` → `Crunchy Postgres for Kubernetes`, deploy the `rhino` `PGAdmin` custom resource, create a Route (`route-pgadmin`) to expose it, and create the `pgadmin-password-secret` via `oc create secret generic pgadmin-password-secret --from-literal=rhino-password=redhat01`.
5. Access the PGAdmin console with `rhino@example.com` / `redhat01`, connect to the database using the password found in `Workloads` → `Secrets` → `hippo-ha-pguser-hippo-ha`, and run the sample script to create the `demo_app` schema and `demo_transactions` table with sample rows.
6. Demonstrate self-healing: open the Topology view to show the Crunchy deployment, go to `Workloads` → `Pods`, delete the pod labeled `role=master` (e.g., `hippo-ha-pgha1`), and watch the Operator promote a replica to Primary and provision a new Standby in Topology. Confirm the `demo_transactions` data survived by re-running the `SELECT` in pgAdmin.
7. Demonstrate scale-on-demand: edit the `PostgresCluster` YAML in the console, change `replicas: 2` to `replicas: 3`, click `Save`, and watch a new pod join and start replicating in Topology.
8. Demonstrate credential security: go to `Workloads` → `Secrets` and show the auto-rotated `hippo-ha-pguser-hippo-ha` Secret, explaining that applications bind to the Secret rather than hardcoding passwords in Git.
9. Create a new project for SQL Server (`Home` → `Projects`). Go to `Helm` → `Repositories` → `Create Helm Repository` and add the cluster-scoped `sqlserver-helm-charts` repository (`https://mpbravo.github.io/helm-charts/`).
10. In `Ecosystem` → `Software Catalog`, filter by `Helm Charts`, select the new repository, choose the SQL Server chart, click `Create`, expand the form to set the SA password, and click `Create` again. Confirm the pod appears running in the Topology view.
11. Verify the exposed port from the embedded web terminal (`oc get services`). For a live demo, skip the NodePort connection test (disallowed in most customer environments) and instead open the SQL Server pod's own terminal under `Workloads` → `Pods` and run `sqlcmd` there — it is bundled in the container image — to confirm `SELECT @@VERSION;` returns successfully.
12. Create a new project for Online Boutique. Go to `Helm` → `Repositories` → `Create Helm Repository` and add the cluster-scoped `online-boutique-charts` repository (`https://mpbravo.github.io/online-boutique-helm/`).
13. In `Software Catalog`, filter by `Helm Charts`, select the Online Boutique chart, and click `Create`. After a few moments, confirm the application appears in the `Topology` view and access it via `Networking` → `Routes`.

### Key Takeaways
- Operators (Crunchy Postgres for Kubernetes) go beyond simple deployment — they actively manage HA failover, horizontal scaling, and credential rotation for stateful workloads, acting as an automated "24/7 SRE."
- Helm is well suited to "Day 1" packaging of both third-party software (SQL Server) and existing, complex microservices applications (Online Boutique), while Operators handle ongoing "Day 2" state management for stateful workloads — the two approaches are complementary, not competing.
- Both third-party (COTS) and pre-existing applications can be onboarded to OpenShift through standard, repeatable Software Catalog and Helm workflows without custom platform-specific rework.

### Infrastructure Notes
Requires at least 10-15 GiB of free storage for the Crunchy Postgres demo (or reduced `512Mi` storage requests). Requires pre-staged, reachable Helm chart repositories at `https://mpbravo.github.io/helm-charts/` (SQL Server) and `https://mpbravo.github.io/online-boutique-helm/` (Online Boutique). No local `sqlcmd` installation is needed — the live demo connects from within the SQL Server pod's own terminal instead.
