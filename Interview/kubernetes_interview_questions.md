# Kubernetes Interview Questions and Answers

## Sources
1. [cloudzenix.in](https://cloudzenix.in/kubernetes-interview-questions-and-answers/)
2. [razorops.com](https://razorops.com/blog/kubernetes-interview-questions)
3. [cloudzero.com](https://www.cloudzero.com/blog/kubernetes-interview-questions/)
4. [spacelift.io](https://spacelift.io/blog/kubernetes-interview-questions)
5. [geeksforgeeks.org](https://www.geeksforgeeks.org/kubernetes-interview-questions/)
6. [simplilearn.com](https://www.simplilearn.com/tutorials/kubernetes-tutorial/kubernetes-interview-questions)
7. [devopsschool.com](https://www.devopsschool.com/blog/top-50-kubernetes-interview-questions-and-answers/)
8. [edureka.co](https://www.edureka.co/blog/interview-questions/kubernetes-interview-questions/)
9. [plainenglish.io](https://plainenglish.io/blog/top-50-kubernetes-interview-questions-2024-edition)
10. [medium.com](https://medium.com/@devopslearning/kubernetes-interview-questions-and-answers-2024-5a3d0f0e8f0b)

## Basic Kubernetes Concepts

### 1. What is Kubernetes?
**Answer:**
Kubernetes (K8s) is an open-source container orchestration system for automating application deployment, scaling, and management.
*   **Origin:** Originally designed by Google and now maintained by the Cloud Native Computing Foundation (CNCF).
*   **Function:** It clusters together groups of hosts running containers, and helps you easily and efficiently manage those clusters.
*   **Analogy:** If containers are passengers, Kubernetes is the airport traffic control.

### 2. What are the key components of Kubernetes architecture?
**Answer:**
It is a Master-Agent (Control Plane - Worker) architecture.
1.  **Control Plane (Master):** The brain.
    *   **API Server:** Entry point for REST commands.
    *   **Etcd:** Key-value store for cluster usage data.
    *   **Scheduler:** Assigns pods to nodes.
    *   **Controller Manager:** Maintains desired state (e.g., ensuring 3 replicas exist).
2.  **Worker Nodes:** The muscle.
    *   **Kubelet:** Agent that runs on the node.
    *   **Kube-proxy:** Handles networking.
    *   **Container Runtime:** Runs the container (Docker/containerd).

### 3. Explain Pods in Kubernetes.
**Answer:**
*   **Definition:** The smallest deployable unit in K8s. It is a logical wrapper around one or more containers.
*   **Characteristics:** Containers in a Pod share the same network namespace (IP address) and storage volumes (if configured).
*   **Lifecycle:** Pods are ephemeral (mortal). They are created, assigned a unique ID (UID), and scheduled to nodes where they remain until termination or failure.

### 4. What is the role of etcd in Kubernetes?
**Answer:**
*   **Role:** `etcd` is the single source of truth for the cluster.
*   **Data:** It stores configuration data, state, metadata, and secrets.
*   **Reliability:** It is a highly available, distributed key-value store.
*   **Backup:** Losing `etcd` data means losing the cluster configuration, so reliable backup strategies are critical.

### 5. How do you scale applications in Kubernetes?
**Answer:**
1.  **Manual Scaling:** `kubectl scale deployment <name> --replicas=5`.
2.  **Horizontal Pod Autoscaler (HPA):** Automatically increases/decreases *number* of pods based on CPU/RAM usage.
3.  **Vertical Pod Autoscaler (VPA):** Automatically adjusts the *resource requests/limits* (CPU/RAM) of existing pods.
4.  **Cluster Autoscaler:** Adds/removes actual *Nodes* (VMs) if there is pending work that cannot be scheduled due to lack of resources.

### 6. What is a Kubernetes Deployment?
**Answer:**
A Deployment is a higher-level abstraction that manages ReplicaSets and Pods.
*   **Purpose:** It allows you to describe a desired state (e.g., "Run nginx version 1.14 with 3 replicas").
*   **Updates:** It handles rolling updates seamlessly. You can update the image version, and the Deployment will replace old pods with new ones gradually.
*   **Rollback:** It maintains revision history, allowing you to undo a bad deployment.

### 7. Explain Kubernetes Service.
**Answer:**
A Service is an abstraction that defines a logical set of Pods and a policy to access them.
*   **Problem:** Pods are ephemeral; their IPs change when they restart.
*   **Solution:** A Service provides a stable, static IP address (ClusterIP) and DNS name. It load balances traffic across the dynamic set of backing Pods (identified via Selectors).

### 8. What is a Kubernetes Namespace?
**Answer:**
Namespaces are virtual clusters backed by the same physical cluster.
*   **Use Case:** Dividing cluster resources between multiple users (via Resource Quotas) or environments (Dev, Test, Prod).
*   **Isolation:** Names must be unique within a namespace, but not across namespaces.
*   **Default:** Resources go into the `default` namespace if none is specified.

### 9. What is the difference between Kubernetes and Docker?
**Answer:**
Comparison is effectively Apples vs. Fruit Basket.
*   **Docker:** The containerization platform. It builds the container image and runs the container runtime. Focuses on the "Package" and "Run" (single host).
*   **Kubernetes:** The orchestrator. It manages the Docker containers across multiple hosts. Focuses on "Scale" and "Manage".
*   *Note:* K8s has deprecated Docker Shim and now uses `containerd` or `CRI-O` as the runtime, though Docker is still used to build images.

### 10. What is kubectl?
**Answer:**
*   **Definition:** The official command-line interface (CLI) for running commands against Kubernetes clusters.
*   **Mechanism:** It acts as a client for the Kubernetes API. It converts commands (like `kubectl get pods`) into REST API calls sent to the Kube-API Server.
*   **Config:** It uses a `kubeconfig` file (usually in `~/.kube/config`) to authenticate and locate the cluster.

## Kubernetes Architecture and Components

### 11. Describe the role of the Kube-API Server.
**Answer:**
*   **Front End:** It is the only component that interacts directly with `etcd`. All other components (Scheduler, Kubelet) must talk to the API Server.
*   **Validation:** It validates the configuration of API objects (e.g., checking if the YAML syntax is correct).
*   **Authentication/Authorization:** It authenticates the user and authorizes the request before processing.

### 12. What is the function of the Kube-scheduler?
**Answer:**
*   **Role:** Evaluates requirements and resources to decide *where* to put a new Pod.
*   **Process:** It watches for newly created Pods that have no assigned Node.
*   **Decision Factors:**
    *   Resource Requirements (CPU/RAM requests).
    *   Taints and Tolerations.
    *   Node Affinity/Anti-Affinity rules.
    *   Data Locality.

### 13. Explain the Kube-controller-manager.
**Answer:**
It is a single binary that runs multiple controller loops bundled together.
*   **Node Controller:** Notices when nodes go down.
*   **Replication Controller:** Maintains the correct number of pods.
*   **Endpoints Controller:** Populates Endpoint objects (links Services & Pods).
*   **Service Account & Token Controllers:** Create default accounts and API access tokens for new namespaces.

### 14. What is Kubelet?
**Answer:**
The "Captain" of the worker node.
*   **Communication:** Talks to the API Server to receive instructions ("Run these 3 pods").
*   **Execution:** Instructs the container runtime (e.g., Docker/containerd) to pull images and start containers.
*   **Health:** Reports node status and pod health back to the Master.

### 15. What is Kube-proxy?
**Answer:**
The network proxy running on each node.
*   **Role:** Maintains network rules.
*   **Mechanism:** It manipulates `iptables` or IPVS on the node to handle routing.
*   **Function:** It enables the Service abstraction. When traffic hits a Service IP, Kube-proxy ensures it gets forwarded to one of the backend Pods.

### 16. What is a Container Runtime Interface (CRI)?
**Answer:**
*   **Problem:** K8s originally was hardcoded to use Docker.
*   **Solution:** CRI is a plugin interface that allows K8s to use *any* container runtime that implements the CRI standard.
*   **Examples:** `containerd` (industry standard now), `CRI-O` (lightweight, OCI-compliant), `Docker Engine` (via dockershim - now deprecated).

## Workload Management

### 17. What is the difference between a Deployment and a StatefulSet?
**Answer:**
*   **Deployment:**
    *   Treats pods as interchangeable cattle.
    *   Random names (e.g., `app-5d4f3-gh7y`).
    *   No persistent identity. Scaling down might kill any pod.
    *   Use for: Web servers, stateless APIs.
*   **StatefulSet:**
    *   Treats pods as unique pets.
    *   Predictable names (`db-0`, `db-1`, `db-2`) and startup/shutdown order.
    *   Persistent storage stays with the ID (if `db-0` restarts, it reattaches to `volume-0`).
    *   Use for: Databases (MySQL, Mongo), Kafka, Zookeeper.

### 18. When would you use a DaemonSet?
**Answer:**
Use a DaemonSet when you need exactly **one** instance of a pod on **every** node (or a subset of nodes).
*   **Examples:**
    *   Log collectors (Fluentd, Logstash).
    *   Monitoring agents (Prometheus Node Exporter, Datadog Agent).
    *   Network plugins (Calico, Weave).
*   **Behavior:** As nodes are added to the cluster, the DaemonSet automatically spins up a pod on them.

### 19. What is a ReplicaSet?
**Answer:**
*   **Definition:** A lower-level object that guarantees a specific number of replicas are running.
*   **Relation to Deployment:** You rarely create a ReplicaSet directly. You create a Deployment, and the Deployment creates the ReplicaSet.
*   **Selector:** It uses Set-Based selectors to identify which pods it owns.

### 20. Explain a Kubernetes Job.
**Answer:**
*   **Purpose:** Runs a task to completion (exit code 0), then stops.
*   **Difference from Pod:** A normal pod restarts if it stops (it's meant to run forever). A Job pod terminates when the task is done.
*   **Use Cases:** Database migrations, batch processing, rendering a frame of validation.

### 21. What is a CronJob?
**Answer:**
*   **Definition:** A Job that runs on a time-based schedule (like Crontab in Linux).
*   **Syntax:** Uses standard cron syntax (e.g., `0 0 * * *` for daily at midnight).
*   **Behavior:** At the scheduled time, the CronJob creates a Job object, which creates the Pod.

## Networking in Kubernetes

### 22. How does Kubernetes handle networking?
**Answer:**
The "Flat Network" Model requires:
1.  **Pod-to-Pod:** Every Pod gets a unique IP. Any Pod can talk to any other Pod without NAT.
2.  **Node-to-Pod:** Agents on a node (system daemons) can communicate with all pods on that node.
3.  **CNI:** This is achieved via specific CNI (Container Network Interface) plugins like Calico, Flannel, or Cilium which set up the virtual ethernet pairs and routing tables.

### 23. What is a ClusterIP Service?
**Answer:**
*   **Type:** `type: ClusterIP` (Default).
*   **Access:** Exposes the service on a cluster-internal IP.
*   **Visibility:** Only accessible from *within* the cluster.
*   **Use Case:** Internal microservice-to-microservice communication (e.g., Backend talking to Database).

### 24. What is a NodePort Service?
**Answer:**
*   **Type:** `type: NodePort`.
*   **Access:** Exposes the service on a static port (range 30000-32767) on each Node's IP.
*   **Visibility:** Accessible externally via `<NodeIP>:<NodePort>`.
*   **Use Case:** Quick development testing or allowing external Load Balancers to route to nodes.

### 25. Explain a LoadBalancer Service.
**Answer:**
*   **Type:** `type: LoadBalancer`.
*   **Access:** Provisions an actual Cloud Load Balancer (AWS ELB, Azure LB, GCP LB).
*   **Visibility:** Provides a public IP address.
*   **Use Case:** Production traffic for exposing a service to the internet (though Ingress is often preferred to save costs).

### 26. What is Ingress in Kubernetes?
**Answer:**
*   **Definition:** An API object that manages external access to the services in a cluster, typically HTTP/HTTPS.
*   **Function:** It acts as a Layer 7 Load Balancer / Reverse Proxy.
*   **Features:** SSL termination, path-based routing (`/api` -> Service A, `/` -> Service B), and name-based virtual hosting.
*   **Requirement:** Requires an Ingress Controller (e.g., Nginx, Traefik) to be running in the cluster to function.

### 27. What are Network Policies?
**Answer:**
*   **Definition:** K8s firewall rules.
*   **Default:** By default, all traffic is allowed (Allow All).
*   **Function:** Control traffic flow at the IP address or port level (Layer 3/4).
*   **Example:** "Allow traffic to 'DB Pod' only if it comes from 'Backend Pod'".
*   **Requirement:** The underlying CNI plugin must support Network Policies (Flannel does not; Calico does).

## Storage in Kubernetes

### 28. What is a PersistentVolume (PV)?
**Answer:**
*   **Concept:** A piece of storage in the cluster that has been provisioned by an administrator.
*   **Independence:** It has a lifecycle independent of any individual Pod that uses it.
*   **Analogy:** It is like an external hard drive (AWS EBS, Azure Disk, NFS share) that exists in the data center waiting to be plugged in.

### 29. What is a PersistentVolumeClaim (PVC)?
**Answer:**
*   **Concept:** A request for storage by a user/developer.
*   **Analogy:** A developer saying "I need 10GB of ReadWriteOnce storage."
*   **Binding:** The control plane looks for a PV that matches the PVC requirements. If found, it binds them together. The Pod then mounts the PVC as a volume.

### 30. Explain StorageClass.
**Answer:**
*   **Concept:** "Dynamic Provisioning" profile.
*   **Function:** Instead of manually creating PVs, you define a StorageClass (e.g., `fast-ssd` which maps to `gp2` in AWS).
*   **Workflow:** When a user creates a PVC asking for `storageClassName: fast-ssd`, K8s automatically talks to the Cloud Provider, creates the disk (PV), and binds it.

## Security and Access Control

### 31. What is Role-Based Access Control (RBAC) in Kubernetes?
**Answer:**
RBAC regulates access to K8s resources.
*   **Role/ClusterRole:** Defines *permissions* (rules). "Can `get`, `list` Pods".
*   **Subject:** User, Group, or ServiceAccount.
*   **RoleBinding/ClusterRoleBinding:** Connects the *Role* to the *Subject*. "Grant `User-Bob` the `Pod-Reader` role".

### 32. How do you secure a Kubernetes cluster?
**Answer:**
The "4C" Defense-in-Depth model:
1.  **Cloud:** Secure the infrastructure (VPC, Firewalls).
2.  **Cluster:** RBAC, Network Policies, Private API endpoints, Audit Logging.
3.  **Container:** Minimal base images (distroless), non-root users, image scanning (Trivy).
4.  **Code:** Static analysis, dependency scanning, secure secrets management.

### 33. What are Kubernetes Secrets?
**Answer:**
*   **Purpose:** Stores sensitive data (passwords, tokens, keys).
*   **Mechanism:** Encoded in Base64 (not encrypted by default in earlier versions, but Encryption at Rest should be enabled in Etcd).
*   **Usage:** Injected into Pods as Environment Variables or mounted as files.
*   **Best Practice:** Do not commit YAMLs with secrets to Git. Use external secret stores (Vault, Azure Key Vault).

### 34. Explain Kubernetes Admission Controllers.
**Answer:**
*   **Role:** Gatekeepers that intercept requests to the API Server *after* authentication but *before* object persistence.
*   **Types:**
    *   **Validating:** "Reject this request" (e.g., Reject deployments without resource limits).
    *   **Mutating:** "Modify this request" (e.g., Inject a sidecar container automatically).
*   **Example:** OPA Gatekeeper or Kyverno policy engines.

### 35. What are Pod Security Standards (PSS)?
**Answer:**
PSS has replaced the deprecated PodSecurityPolicies (PSP). It defines three levels of security policies:
1.  **Privileged:** Unrestricted (for system logging, drivers).
2.  **Baseline:** Minimally strict (disallows known privilege escalations).
3.  **Restricted:** Heavily restricted (requires non-root, drop capabilities). Enforced via Admission Controllers.

## Monitoring, Logging, and Troubleshooting

### 36. How do you troubleshoot a Pod that keeps restarting?
**Answer:**
1.  **`kubectl get pods`**: Check `RESTARTS` count.
2.  **`kubectl describe pod <name>`**: Check "Events" section at the bottom. look for "BackOff" or "OOMKilled".
3.  **`kubectl logs <name> --previous`**: Critical step. Viewing current logs shows nothing if the app crashes instantly. `--previous` shows the logs of the *last* crashed instance.

### 37. How do you access logs from a Kubernetes Deployment?
**Answer:**
*   **Single Pod:** `kubectl logs <pod-name>`
*   **Multi-Container:** `kubectl logs <pod-name> -c <container-name>`
*   **Label Selector:** `kubectl logs -l app=frontend` (Aggregates logs from all pods with that label, useful for deployments).
*   **Streaming:** Add `-f` to follow the logs in real-time.

### 38. What is Horizontal Pod Autoscaling (HPA)?
**Answer:**
HPA automatically scales the number of pods in a replication controller, deployment, replica set, or stateful set based on observed CPU utilization.
*   **Metric:** Default is CPU/Memory, but can use Custom Metrics (e.g., Requests per second).
*   **Command:** `kubectl autoscale deployment foo --min=2 --max=10`.
*   **Requirement:** `metrics-server` must be installed in the cluster.

### 39. What is a Pod Disruption Budget (PDB)?
**Answer:**
PDB guarantees application availability during **voluntary** disruptions (e.g., node upgrades, draining).
*   **Example:** "Always ensure at least 2 replicas are running."
*   **Effect:** If an admin tries to drain a node, K8s will block the drain until enough pods have moved and started successfully elsewhere to satisfy the PDB.

### 40. How do you monitor resources in Kubernetes?
**Answer:**
1.  **Kubernetes Dashboard:** Web UI (basic).
2.  **Metrics Server:** Enables `kubectl top nodes` and `kubectl top pods` for instant CLI sizing.
3.  **Prometheus:** The de-facto standard. Scrapes metrics from pods/nodes.
4.  **Grafana:** Visualizes the Prometheus data (Dashboards).
5.  **Cloud Native:** Azure Monitor / AWS CloudWatch Container Insights.

## Advanced Kubernetes Concepts

### 41. What is a Sidecar Container?
**Answer:**
*   **Pattern:** Running a secondary container in the same Pod alongside the main one.
*   **Concept:** They share the same disk and localhost network.
*   **Use Cases:**
    *   **Logging:** Sidecar reads app logs from file and pushes to Splunk/Elastic.
    *   **Proxy:** Sidecar (Envoy/Istio) handles mutual TLS and traffic splitting.
    *   **Config:** Sidecar watches a Git repo and updates config files on the shared volume.

### 42. Explain the concept of Container Orchestration.
**Answer:**
Container orchestration deals with the lifecycle of containers, especially in dynamic environments. It automates:
*   **Provisioning:** Placing containers on nodes.
*   **Redundancy:** Replacing failed containers.
*   **Scaling-up/down:** Adding instances.
*   **Resource Allocation:** Managing CPU/RAM to fit containers efficiently.
*   **Networking:** Exposing ports and load balancing.

### 43. What is a Kubernetes Operator?
**Answer:**
An Operator is a method of packaging, deploying, and managing a Kubernetes application.
*   **Concept:** It encodes human operational knowledge into software.
*   **Mechanism:** It uses Custom Resource Definitions (CRDs) and a custom Controller.
*   **Example:** "Prometheus Operator". Instead of manually managing config files, you create a `Prometheus` object, and the Operator code handles the complex setup, upgrades, and reconfigurations.

### 44. How does Kubernetes handle rolling updates and rollbacks?
**Answer:**
*   **Update:** When you change the image tag in Deployment, it creates a new ReplicaSet. It scales up the New RS (1 replica) and scales down the Old RS (1 replica) iteratively until the swap is complete. Service traffic is load balanced between both during the process.
*   **Rollback:** K8s keeps the Old ReplicaSet around (scaled to 0). Rolling back simply scales the Old RS back up and the New RS down.

### 45. What is the difference between Docker Swarm and Kubernetes?
**Answer:**
*   **Docker Swarm:**
    *   Simpler, easier to set up.
    *   Tightly integrated with Docker CLI.
    *   Good for small environments.
    *   Less feature-rich (Lacks built-in autoscaling, advanced networking).
*   **Kubernetes:**
    *   Steeper learning curve.
    *   Enterprise standard.
    *   Extensible API (CRDs).
    *   Massive ecosystem and support.

### 46. What is the Kubernetes Cluster Autoscaler?
**Answer:**
*   **Scope:** Infrastructure level (Nodes).
*   **Trigger:** It watches for Pods in state `Pending` due to lack of resources.
*   **Action:** It talks to the Cloud Provider (AWS/Azure) to spin up a new Virtual Machine and joins it to the cluster.
*   **Scale Down:** If a node is underutilized (e.g., < 50%) for X minutes, it moves pods elsewhere and terminates the node to save money.

### 47. Explain Kubernetes Taints and Tolerations.
**Answer:**
Mechanism to restrict what pods end up on what nodes.
*   **Taint:** "Dirtying" a node. `kubectl taint nodes node1 gpu=true:NoSchedule`. Now, no normal pods will go there.
*   **Toleration:** A special permission on a Pod. "I can handle the `gpu=true` taint."
*   **Result:** Only Pods with the toleration can schedule on the Tainted node. (Note: Tolerated pods *can* still go to untainted nodes unless Affinity is also used).

### 48. What is Helm in Kubernetes?
**Answer:**
*   **Definition:** The package manager for Kubernetes (like `apt` or `yum` or `pip`).
*   **Chart:** A package of YAML templates.
*   **Templating:** Allows parameterizing YAMLs. Instead of hardcoding `replicas: 3`, you write `replicas: {{ .Values.replicaCount }}`.
*   **Management:** Simplifies installing complex apps (like Prometheus Stack) into a single command: `helm install my-prom prometheus-community/kube-prometheus-stack`.

### 49. How would you upgrade Kubernetes clusters?
**Answer:**
*   **Strategy:** Rolling Upgrade (Node by Node).
*   **Process:**
    1.  Upgrade the Control Plane (Master) first.
    2.  `kubectl drain <node>`: Safely evict all pods from the node.
    3.  Upgrade `kubelet` and `kubectl` on the node (or replace the VM image).
    4.  `kubectl uncordon <node>`: Allow pods to return.
    5.  Repeat for all worker nodes.

### 50. What is the concept of "desired state" in Kubernetes?
**Answer:**
Kubernetes is a declarative system.
*   **Imperative:** "Start 3 containers." (Instructions).
*   **Declarative:** "There should be 3 containers." (Desired State).
*   **Reconciliation Loop:** The Controller watches the current state. If `Current != Desired` (e.g., a pod crashed, so current is 2), it takes action to fix it (starts a new pod) to return to the desired state.

## Advanced Troubleshooting & Architecture

### 51. How would you diagnose and resolve a pod in `CrashLoopBackOff` state?
**Answer:**
*   **Meaning:** The pod starts, crashes (exit code 1), K8s restarts it, it crashes again.
*   **Diagnosis:**
    1.  **Logs:** `kubectl logs <pod>`. Look for app errors (Missing env var, Database connection failed).
    2.  **Events:** `kubectl describe pod <pod>`. Look for `Liveness probe failed`.
    3.  **Command:** Verify the container `command` or `entrypoint` is correct.
*   **Resolution:** Fix the application config, increase memory limits (if OOM), or fix Liveness probe parameters.

### 52. What steps if a pod is stuck in `Pending` state?
**Answer:**
*   **Meaning:** The Scheduler cannot find a node to place the pod.
*   **Diagnosis:** `kubectl describe pod <pod>`. Check the events.
*   **Common Causes:**
    1.  **Insufficient CPU/Memory:** Cluster is full. (Solution: Cluster Autoscaler / Add Nodes).
    2.  **Taints:** Node has a taint the pod doesn't tolerate.
    3.  **Affinity:** Pod requires a specific label (nodeSelector) that doesn't exist.
    4.  **PVC:** Pod requires a Volume that cannot be provisioned (e.g., out of AWS EBS limits).

### 53. How do you troubleshoot `ImagePullBackOff`?
**Answer:**
*   **Meaning:** Kubelet cannot pull the docker image.
*   **Checklist:**
    1.  **Typo:** Check image name and tag spelling.
    2.  **Existence:** Does the image exist in the registry?
    3.  **Authentication:** Is it a private repo? If so, does the Pod have `imagePullSecrets` configured?
    4.  **Network:** Does the node have internet access/NAT Gateway to reach DockerHub/ACR?

### 54. Service is not accessible from outside. How to debug?
**Answer:**
1.  **Check Service Type:** Must be `NodePort` or `LoadBalancer`.
2.  **Endpoints:** Run `kubectl get endpoints <svc-name>`. If empty, the Service Selector doesn't match any Pod labels.
3.  **Pod Health:** Are the backend pods running involved?
4.  **Container Port:** Does the TargetPort in the Service definition match the port the container is listening on (containerPort)?
5.  **Firewall:** For `NodePort`, is the Security Group/Firewall opening port 30xxx?

### 55. How to handle Node failures in Kubernetes?
**Answer:**
*   **Automatic:** K8s detects the node is `NotReady`. After `pod-eviction-timeout` (default 5m), the Controller Manager deletes the pods on that node. The ReplicaSet then creates new replacement pods on healthy nodes.
*   **Manual:**
    *   `kubectl delete node <dead-node>`.
    *   Provision a new node and join it to the cluster.

### 56. Explain the role of `etcd` in HA and disaster recovery.
**Answer:**
*   **HA (High Availability):** Production clusters run an odd number of etcd nodes (3 or 5) to establish a quorum. If one fails, the cluster continues to write data.
*   **DR (Disaster Recovery):** `etcd` contains *all* cluster state. You must take periodic snapshots (`etcdctl snapshot save`). In a catastrophic failure where all masters are lost, you restore the cluster by restoring the etcd snapshot (`etcdctl snapshot restore`).

### 57. How do you secure a Kubernetes cluster? (4C Security Model)
**Answer:**
1.  **Cloud:** Use Private Clusters (API server not on public internet). Restrict access to ports 22, 6443.
2.  **Cluster:** Enable RBAC. Disallow anonymous access. Use Network Policies to deny-all default traffic.
3.  **Container:** Use `distroless` images. Do not run as `root`. Make filesystem Read-Only.
4.  **Code:** Scan supply chain. Use OPA/Kyverno to enforce "No privileged containers" policies.

### 58. Difference between StatefulSet and Deployment?
**Answer:**
*   **Deployment:** Designed for stateless applications (Web servers). Pods are identical and disposable. Names are random hashes. Storage is ephemeral or shared.
*   **StatefulSet:** Designed for stateful apps (Databases).
    *   **Updates:** Sequential/Ordered updates (0 -> 1 -> n).
    *   **Networking:** Hostnames are predictable (`web-0`, `web-1`).
    *   **Storage:** Example creates a unique PersistentVolumeClaim (PVC) for *each* pod replica so data persists with that specific index.

### 59. What are Custom Resource Definitions (CRDs)?
**Answer:**
*   **Concept:** Extending the K8s API.
*   **Default:** K8s knows about Pods, Services, etc.
*   **Extension:** You can teach K8s new tricks. e.g., define a `PostgresDatabase` resource.
*   **Usage:** A user applies a YAML for `kind: PostgresDatabase`. A custom **Controller** (Operator) sees this and spins up the necessary StatefulSets, PVCs, and Services to make it happen.

### 60. Describe a production-grade K8s architecture.
**Answer:**
*   **Masters:** 3x Control Plane nodes across 3 Availability Zones (AZ).
*   **Etcd:** Stacked on masters or external cluster (3 nodes).
*   **Workers:** Multiple Node Groups (Autoscaling Groups) spanning AZs.
*   **Ingress:** Public Load Balancer -> Ingress Controller (Nginx) -> Services.
*   **Storage:** StorageClasses backed by Cloud Block Store (EBS/Premium SSD).
*   **Observability:** Prometheus + Grafana (Monitoring), ELK/Splunk (Logging).
*   **Security:** Private endpoints, Jumpbox/Bastion for admin access.
