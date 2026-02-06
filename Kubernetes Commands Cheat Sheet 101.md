

# Kubernetes Commands Cheat Sheet
### A Comprehensive Guide to Managing Your Clusters
---
*   **The Go-To Handbook for DevOps Engineers**
*   Step-by-Step CLI Mastery with `kubectl`
*   Learn to Create, Manage, and Monitor Your Kubernetes Resources efficiently
*   Designed for Clarity and Real-World Application



## 🏗️ Creating Objects in Kubernetes
### From YAMLs to One-Liners

| Action | Command | Description |
| :--- | :--- | :--- |
| **Apply Manifest** | `kubectl apply -f ./file.yaml` | Create resources directly from a configuration file. |
| **Run Pod** | `kubectl run name --image=img` | Spin up a quick pod for testing or simple workloads. |
| **Deployment** | `kubectl create deployment d_name --image=img` | Create a managed set of replicas for high availability. |
| **Service** | `kubectl create service clusterip s_name --tcp=80:8080` | Network exposure for your internal applications. |
| **Quick YAML** | `kubectl run ... --dry-run=client -o yaml > pod.yaml` | Generate a template without actually creating the resource. |
| **Config/Secret** | `kubectl create configmap name --from-literal=k=v` | Store configuration data or sensitive keys. |



## 📦 Master of Pods (Pod Commands)
### Managing the Smallest Deployable Units

*   **View Your Pods:** `kubectl get pods -n namespace` 
    *   *Tip:* Use `-o wide` to see internal IPs and Node names.
*   **Deep Dive:** `kubectl describe pod <pod_name>`
    *   Essential for debugging; shows events like Pulling, Creating, and Failing.
*   **Live Stream Logs:** `kubectl logs -f <pod_name>`
    *   Watch your application logs in real-time as they happen.
*   **Inside the Shell:** `kubectl exec -it <pod_name> -- /bin/bash`
    *   Jump directly into the container's terminal to run manual checks.
*   **Cleanup:** `kubectl delete pod <pod_name>`
    *   Manually kill a pod. (Note: If it's in a Deployment, it will auto-restart).



## 🚀 Deployment Operations
### Managing the Lifecycle of Applications

- **Check Status:** `kubectl get deployment <name>`
    - See how many replicas are ready vs. requested.
- **Scale Up/Down:** `kubectl scale deployment <name> --replicas=5`
    - Instantly increase your application capacity based on load.
- **Switch Versions:** `kubectl set image deployment <name> <container>=<new_image>`
    - Trigger a rolling update with a new container version.
- **Configuration Edit:** `kubectl edit deployment <name>`
    - Open the running configuration in your default text editor for instant changes.
- **Debug Deployment:** `kubectl logs deployment/<name> -f`
    - View logs for all pods belonging to that specific deployment.



## 🌐 Networking: Services & Endpoints
### Connecting Users to Applications

*   **Service Overview:** `kubectl get service (svc)`
    - Lists all services, including their ClusterIP and external ports.
*   **Detailed Network Config:** `kubectl get svc <name> -o yaml`
    - View the complete networking manifest including selectors.
*   **Endpoints Tracking:** `kubectl get endpoints <name>`
    - Check which Pod IPs are currently attached to a specific Service.
*   **Modify Network:** `kubectl edit svc <name>`
    - Change port mappings or service types (ClusterIP to NodePort/LoadBalancer).
*   **Expose Resources:** `kubectl expose pod <name> --type=NodePort --port=80`
    - Create a Service for an existing Pod on the fly.



## 🖥️ Node Management
### Maintaining the Infrastructure

- **Node Inspection:** `kubectl describe node <node_name>`
    - Check CPU/Memory capacity, OS info, and Taints.
- **Maintenance Prep (Drain):** `kubectl drain <node_name>`
    - Safely evict all pods from a node so you can perform maintenance.
- **Block Scheduling (Cordon):** `kubectl cordon <node_name>`
    - Mark a node as "Unschedulable" so no new pods start there.
- **Resume Scheduling (Uncordon):** `kubectl uncordon <node_name>`
    - Allow new pods to be placed on the node again.
- **Cluster Glance:** `kubectl get nodes`
    - Quick check: Are all nodes "Ready"?



## 🛠️ Advanced Workloads
### Ingress, DaemonSets, and StatefulSets

| Resource | Purpose | Key Command |
| :--- | :--- | :--- |
| **Ingress** | HTTP/HTTPS Routing | `kubectl get ingress` - Manage external access. |
| **DaemonSet** | Run on Every Node | `kubectl get ds` - For logs/monitoring agents. |
| **StatefulSet** | Ordered Deployment | `kubectl describe statefulset` - For DBs/Storage. |
| **Edit Ingress** | Live Rule Updates | `kubectl edit ingress <name>` |
| **Wide View** | Network Details | `kubectl get ingress -o wide` |



## 🔐 Configuration & Secrets
### Handling Data Safely

*   **ConfigMaps:** 
    *   `kubectl get cm`: List all configuration maps.
    *   `kubectl create configmap app-config --from-file=config.env`: Load an entire file into a map.
*   **Secrets:**
    *   `kubectl get secret <name>`: View metadata of a secret (content stays hidden).
    *   `kubectl describe secret <name>`: Check keys within, but values are base64 encoded.
*   **Direct Modification:**
    *   `kubectl edit secret <name>`: Manually update credentials or API keys.
*   **Exporting:**
    *   `kubectl get cm <name> -o yaml`: Export current config for backup or version control.



## 🔄 Rollouts, Jobs & Scheduling
### Automation and Time Management

- **Rollout History:** `kubectl rollout history deployment <name>`
    - View past versions of your application.
- **Rollback:** `kubectl rollout undo deployment <name> --to-revision=2`
    - Mistake in production? Revert to a previous stable state instantly.
- **Jobs:** `kubectl create job <name> --image=img`
    - Run a task to completion (like a migration or data processing).
- **CronJobs:** `kubectl create cronjob <name> --image=img --schedule="* * * * *"`
    - Automate tasks to run on a specific schedule (e.g., daily backups).
- **Manual Trigger:** `kubectl create job --from=cronjob/<name>`
    - Run a scheduled job manually right now.

0

## 📊 Monitoring, Policy & Labels
### Keeping Everything Organized and Secure

*   **Performance Monitoring:**
    *   `kubectl top node`: View CPU/Memory usage per node.
    *   `kubectl top pod`: Locate resource-hungry pods.
*   **Label Management:**
    *   `kubectl get pods --show-labels`: Identify pods by their metadata.
    *   `kubectl label pod <name> env=production`: Tag resources for easier filtering.
*   **Filtering:**
    *   `kubectl get pod -l env=production`: Only show pods with a specific label.
*   **Security (Network Policy):**
    *   `kubectl get networkpolicy`: List traffic rules.
    *   `kubectl describe netpol <name>`: Verify which pods can talk to each other.
