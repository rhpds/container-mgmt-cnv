# Module Outline (spec/modules/module-05-observability.md)

**Module Title:** Observability - Logging and Monitoring

### Brief Overview
This module reviews OpenShift's built-in observability tooling, showing platform engineers and infrastructure architects how to monitor cluster health and application performance without bolting on separate tooling. Following the real Showroom content, the presenter enables User Workload Monitoring and runs PromQL queries, deploys a demo-sized LokiStack (backed by MinIO) with a ClusterLogForwarder for centralized logging, and installs the Cluster Observability Operator's Troubleshooting Panel to correlate a firing alert with its related resources and logs in one click.

### Audience and Time
- **Target personas:** Platform engineers, infrastructure architects, and IT decision-makers evaluating OpenShift for container management.
- **Experience level:** Intermediate.
- **Prerequisites for this module:** General familiarity with Kubernetes/container concepts. Presenter-led; no learner setup required.
- **Estimated duration:** 10 min

### Learning Objectives
- Explain OpenShift's built-in monitoring stack (Prometheus, Alertmanager, Thanos Querier), split into always-on core platform monitoring and optional, self-service User Workload Monitoring.
- Enable User Workload Monitoring, explore the built-in Compute Resources dashboards, and run PromQL queries via `Observe` → `Metrics`.
- Create and silence a custom `PrometheusRule` to demonstrate the full alerting pipeline end-to-end.
- Deploy a demo-sized `LokiStack` (backed by MinIO object storage) and a `ClusterLogForwarder` to enable centralized application and infrastructure logging.
- Use the Cluster Observability Operator's Troubleshooting Panel (Korrel8r) to correlate a firing alert with its related pod, deployment, and logs in a single click.

### Lab Structure
| Section | Title | Duration |
|---------|-------|----------|
| 1       | Monitoring your cluster and applications | 3 min |
| 2       | Centralized logging with LokiStack | 4 min |
| 3       | Correlating signals with the Troubleshooting Panel | 3 min |

### Detailed Steps
1. Introduce the built-in monitoring stack (Prometheus, Alertmanager, Thanos Querier) as a day-1 capability on every cluster, unlike vanilla Kubernetes where a metrics pipeline must be sourced and wired together manually — including on this compact Single Node OpenShift (SNO) environment.
2. Explain the two monitoring layers: always-on core platform monitoring (nodes, API server, etcd, Operators, with pre-built Red Hat dashboards and alerting rules) and optional User Workload Monitoring (UWM), which lets application teams bring their own metrics/alerts/dashboards without `cluster-admin` access.
3. Log in as `cluster-admin` and enable UWM by applying the `cluster-monitoring-config` ConfigMap (`enableUserWorkload: true`) in the `openshift-monitoring` namespace from the embedded terminal.
4. In the `Administrator` perspective, go to `Observe` → `Dashboards`, select the `Kubernetes / Compute Resources / Cluster` dashboard with `All Projects` selected, then switch the dashboard dropdown to `Node` and select the single node to drill into per-node network, disk I/O, and filesystem detail.
5. Go to `Observe` → `Metrics` and run `count(kube_pod_status_phase{phase="Running"}) by (namespace)` to count running pods per namespace, then `sum(rate(container_cpu_usage_seconds_total{container!=""}[5m])) by (namespace)` to spot the noisiest workloads by CPU, toggling between `Table` and `Graph` views.
6. Create project `demo-observability` and apply the `demo-always-firing` `PrometheusRule` (`expr: vector(1) > 0`, `for: 1m`). After about a minute, go to `Observe` → `Alerting`, clear filters, and show the `DemoAlwaysFiring` alert in a `Firing` state. Click it and click `Silence Alert` to demonstrate acknowledging noise without deleting the rule, then clean up with `oc delete prometheusrule demo-always-firing -n demo-observability`.
7. Introduce OpenShift Logging's three components: Vector (a `DaemonSet` collector tailing container/node logs), the Loki Operator/`LokiStack` (a horizontally scalable, LogQL-queried store with `application`/`infrastructure`/`audit` streams — no Elasticsearch cluster to babysit), and the Logging UI Plugin (embeds a `Logs` view directly in `Observe`). Note the `1x.demo` sizing profile is purpose-built for PoC and small/single-node clusters.
8. Deploy a MinIO instance in a new `demo-logging-storage` project as the S3-compatible object store backing LokiStack (a single-replica Deployment, PVC, and Service), then create the `logging-loki` bucket using the `mc` client bundled in the MinIO image (`oc rsh` into the MinIO deployment).
9. Install the Red Hat OpenShift Logging Operator and create the `logging-loki-s3` secret referencing the MinIO endpoint and credentials. Install the Loki Operator and the Red Hat Cluster Observability Operator (create an instance for logging). Check the cluster's default `StorageClass` via `oc get storageclass`, then deploy the `LokiStack` custom resource named `logging-loki` in the `openshift-logging` project with `size: 1x.demo`.
10. Create the `logging-collector` `ServiceAccount` in `openshift-logging`, then bind the `collect-application-logs` and `collect-infrastructure-logs` ClusterRoles (created by the Logging Operator) and the `logging-collector-logs-writer` ClusterRole (created by the Loki Operator once `LokiStack` is deployed) to it via `oc adm policy add-cluster-role-to-user`.
11. Create the `lokistack-gateway-ca-bundle` `ConfigMap` with the `service.beta.openshift.io/inject-cabundle: "true"` annotation to trust LokiStack's internal service-CA-signed TLS certificate, and verify the CA content was injected.
12. Apply the `ClusterLogForwarder` (`observability.openshift.io/v1`) named `instance`, using the `logging-collector` service account, forwarding `application` and `infrastructure` logs to the `logging-loki` `LokiStack` output over TLS with the CA bundle — noting that Steps 2-4 above must all be complete first, or the controller silently rejects the resource. Verify the collector pods are running with no SSL errors via `oc get pods` and `oc logs`.
13. In `Observe` → `Logs`, filter by the `demo-lightspeed` namespace (created in Module 4) and search for `bad-app` to show its `echo starting && exit 1` log line is still available even though that pod was deleted long ago — the punchline that centralized logging decouples log lifespan from pod lifespan. Try a free-text search across all namespaces, e.g. `CrashLoopBackOff`, to show fast cluster-wide triage.
14. Introduce the Cluster Observability Operator's Troubleshooting Panel, powered by the open-source Korrel8r correlation engine, as the newest evolution beyond checking `Observe -> Metrics/Alerting` and `Observe -> Logs` as three separate tools.
15. Install the Cluster Observability Operator via `Ecosystem` → `Software Catalog`, then apply the `Monitoring`, `Logging` (referencing the `logging-loki` `LokiStack`), and `TroubleshootingPanel` `UIPlugin` resources one at a time, verifying each becomes `Available` with `oc get uiplugin` before moving to the next, then hard-refresh the console.
16. Reopen the `DemoAlwaysFiring` alert (or the `bad-app` `CrashLoopBackOff` pod from Module 4) under `Observe` → `Alerting`, click it, open the `Troubleshooting Panel` drawer, and show it automatically proposing the related `Pod`/`Deployment` and a link into `Observe -> Logs` pre-filtered to that exact pod and time range — what used to take three tools now takes one click.

### Key Takeaways
- OpenShift ships Prometheus/Alertmanager/Thanos-based monitoring and self-service User Workload Monitoring out of the box, even on constrained Single Node OpenShift environments.
- The `1x.demo` LokiStack sizing profile makes centralized logging (Vector + Loki + the Logging UI Plugin) practical on small and single-node clusters, and decouples log lifespan from pod lifespan — logs from long-deleted pods remain searchable.
- The Cluster Observability Operator's Troubleshooting Panel (Korrel8r) is the newest observability capability, replacing manual cross-referencing between alerts, metrics, and hand-written LogQL queries with one-click correlation across signals.

### Infrastructure Notes
Requires a default `StorageClass` for the MinIO-backed object storage behind LokiStack, and enough headroom for the Loki Operator, Cluster Observability Operator, and their `1x.demo`-sized components on the single node. `ClusterLogForwarder` setup order matters: the collector `ServiceAccount`, its RBAC bindings, and the CA bundle `ConfigMap` must all exist before the `ClusterLogForwarder` is applied, or it is silently rejected. The Cluster Observability Operator's UI plugins ship some capabilities as Technology Preview and can vary by cluster version — verify what's available before presenting live.
