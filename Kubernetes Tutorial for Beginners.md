

# Kubernetes Tutorial for Beginners
## Your Complete Guide to Container Orchestration


## What is Container Orchestration?
- **Automated Management:** Handling the deployment, scaling, and networking of containerized apps.
- **Complexity Reducer:** Managing 100s of containers manually is impossible.
- **Workflow Automation:** Kubernetes does the "heavy lifting" for you.



## Why Do We Need Kubernetes?
- **High Availability:** Keeps your app running even if a server fails.
- **Scalability:** Automatically adds more instances during high traffic.
- **Resource Efficiency:** Packs containers efficiently onto hardware.



## Real-World Scaling Challenge
- **Manual vs. Auto:** Imagine scaling from 1 to 50 server instances manually.
- **Downtime Risks:** One error during manual scaling can crash the whole system.
- **The Kubernetes Way:** You define the goal, K8s makes it happen.



## Key Benefits for Beginners
- **Portability:** Move apps between cloud and local smoothly.
- **Self-Healing:** Restarts failed containers automatically.
- **Open Source:** Huge community support and standard across tech.



## Installing Kubernetes Tools
- **minikube:** Runs a mini cluster on your laptop.
- **kubectl:** The "remote control" command-line tool.
- **Docker:** The engine that runs the actual containers.



## Choose Your Hypervisor
- **Windows:** Use Hyper-V or VirtualBox.
- **Mac:** Use HyperKit or VirtualBox.
- **Linux:** Docker runs natively (fastest!).
- **Recommended:** VirtualBox is a great universal choice for learners.



## Setup Step-by-Step
1. Install a Hypervisor (like VirtualBox).
2. Install Docker Desktop.
3. Install the `kubectl` binary.
4. Install the `minikube` binary.



## Starting the Cluster
- **Command:** `minikube start`
- **What happens:** minikube creates a Virtual Machine and sets up the Kubernetes control plane.
- **Verification:** Use `kubectl get nodes` to see your running cluster.



## Troubleshooting Setup
- **Check Status:** `minikube status`
- **Driver Issues:** Use `--driver=virtualbox` if the automatic choice fails.
- **Logs:** Use `minikube logs` if the cluster won't start.



## Your First "Hello World"
- **Run command:**
  `kubectl run hello-kube --image=fhsinchy/hello-kube --port=80`
- **Goal:** Get a web server running inside a Pod quickly.



## What is a Pod?
- **Smallest Unit:** The smallest thing you can run in Kubernetes.
- **Wrapper:** It holds one (or more) containers.
- **Analogy:** Like a protective shell for your application code.



## Checking Pod Status
- **Command:** `kubectl get pods`
- **Status Meanings:**
  - `Pending`: Downloading image.
  - `Running`: App is live!
  - `CrashLoopBackOff`: Something is wrong with the code.



## Exposing the App
- **Command:**
  `kubectl expose pod hello-kube --type=LoadBalancer --port=80`
- **Service:** This creates a stable "address" so you can reach the Pod.



## Accessing the URL
- **Minikube Shortcut:**
  `minikube service hello-kube`
- **Result:** Your browser opens and shows the "Hello World" page!
- **Cleanup:** `kubectl delete pod hello-kube`



## Cluster Architecture
- **Control Plane:** The "Brain" (decides what happens).
- **Nodes:** The "Workers" (do the actual work/run apps).
- **Cluster:** A group of nodes linked together.



## Control Plane: The API Server
- **kube-api-server:** The entry point.
- Every command you type goes through here.
- It validates your requests and translates them for the cluster.



## Control Plane: Storage
- **etcd:** A key-value database.
- Stores the "Source of Truth" for the entire cluster.
- Remembers exactly what is supposed to be running.



## Control Plane: Scheduler & Manager
- **kube-scheduler:** Decides which node is healthy enough to run a new Pod.
- **kube-controller-manager:** Constantly checks if current state matches your desired state.



## Worker Node: The Kubelet
- **kubelet:** The "manager" inside each worker node.
- It takes orders from the Control Plane and makes sure containers are running.
- If a container dies, kubelet tries to restart it.



## Worker Node: Networking
- **kube-proxy:** Handles the network traffic.
- It makes sure that requests to a Service address actually reach the right Pod.
- It manages IP tables and rules on each node.



## Architecture Summary
| Part | Function |
|---|---|
| API Server | Gateway |
| etcd | Database |
| Scheduler | Matchmaker |
| Kubelet | Local Boss |
| Kube-proxy | Traffic Cop |



## Kubernetes Objects: Introduction
- Everything in K8s is an "Object."
- **Persistent Entities:** K8s uses these to represent the state of your cluster.
- Examples: Pods, Services, Deployments, Namespaces.



## Pod Protocols
- **Sharing:** Containers in the same Pod share the same IP.
- **Communication:** They can talk to each other via "localhost."
- **Storage:** They can share local disk folders easily.



## Service Types: ClusterIP
- **Internal Only:** Used for communication within the cluster.
- **Use Case:** A front-end app talking to a back-end database.
- Default service type in Kubernetes.



## Service Types: NodePort
- **External Access:** Opens a specific port (30000-32767) on all nodes.
- **URL:** `http://<Node-IP>:<NodePort>`
- Great for quick testing without a cloud provider.



## Service Types: LoadBalancer
- **Cloud Feature:** Provisions an external IP address (GCP, AWS, Azure).
- **The Standard:** Best way to expose apps to the public internet.
- Handles traffic distribution automatically.



## Labels & Selectors
- **Labels:** Meta-tags like `app: guestbook` or `env: prod`.
- **Selectors:** Services use these to "find" which Pods they should send traffic to.
- Logical grouping instead of using IP addresses.



## Why Declarative Approach?
- **Imperative:** "Run this pod now." (Manual)
- **Declarative:** "I want 3 pods running nginx." (Automated)
- **GitOps:** You save these declarations in files (YAML) for version control.



## Anatomy of a YAML File
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
  - image: nginx
    name: nginx-container
```
- **Kind:** What is it?
- **Spec:** What should it do?



## Applying Configurations
- **Command:** `kubectl apply -f filename.yaml`
- This sends your "wish list" to the API server.
- Kubernetes then starts a loop to make your cluster match that file.



## Managing Changes
- To update, you don't delete things. 
- You edit the YAML file (e.g., change image version) and run `kubectl apply` again.
- K8s handles the transition smoothly.



## Checking Differences
- **Command:** `kubectl diff -f filename.yaml`
- Shows you what will change before you actually apply it.
- **Verify:** `kubectl get <object>` to see the results.



## Deployments: The Big Boss
- Deployments manage **ReplicaSets**.
- They ensure the correct number of pods is always running.
- They handle updates (Rollouts) and Rollbacks.



## Self-Healing in Action
- If a node goes down, the Deployment notices.
- It automatically schedules the missing pods onto a different healthy node.
- No human intervention required!



## Scaling Up & Down
- **Command:** `kubectl scale deployment my-app --replicas=10`
- Instantly jumps from 3 to 10 instances.
- Traffic is automatically balanced across all 10 by the Service.



## Rolling Updates
- Change image version in the YAML.
- `kubectl apply`
- K8s stops one old pod, starts one new pod, and repeats.
- Result: **Zero Downtime!**



## Rolling Back Updates
- **Command:** `kubectl rollout undo deployment my-app`
- If the new version is buggy, K8s quickly reverts to the previous stable version.
- Minimizes user impact.



## Kubernetes Storage
- **Ephemeral:** By default, if a container restarts, data is lost.
- **Persistent Volumes (PV):** Virtual disks that stay alive even if pods die.
- **Analogy:** External hard drives for your cluster.



## Persistent Volume Claims (PVC)
- A Pod doesn't look for a specific disk.
- It makes a "Claim" (PVC): "I need 5GB of storage."
- K8s finds a matching Volume (PV) and connects them.



## Storage Classes
- Different speeds for different needs (SSD vs. HDD).
- **Dynamic Provisioning:** K8s can automatically create a disk when a PVC is submitted.
- No need for manual disk setup by admins.



## Volume Mount Paths
- Inside the YAML, you tell the container where to "find" the disk.
- Example: Mount the PV to `/var/lib/mysql`.
- The database code just sees a normal folder, but the data is safe on the PV.



## ConfigMaps
- Store non-sensitive configuration data (e.g., app settings, URLs).
- Injected into containers as Environment Variables or files.
- Separates configuration from application code.



## Secrets
- Special objects for sensitive data (Passwords, API Keys, Tokens).
- **Security:** Base64 encoded and stored more securely than ConfigMaps.
- Still needs extra encryption for production use.



## Environment Variables
- Most apps read config from environment variables.
- K8s makes it easy to map ConfigMap/Secret values directly to these variables.
- No hardcoding passwords in Dockerfiles!



## Managing Secrets Safely
- **Don't Commit:** Never put secret YAML files into public Git repositories.
- **RBAC:** Use Role-Based Access Control to limit who can see certain secrets.
- **Namespaces:** Keep secrets isolated to specific project areas.



## Ingress Controllers
- **Beyond Services:** Manages complex rules like `myapp.com/api` vs `myapp.com/web`.
- Acts as an intelligent gateway for HTTP(S) traffic.
- Handles SSL certificates in one central place.



## The Kubernetes Dashboard
- **Web UI:** A visual way to see your cluster health.
- **Minikube:** Use `minikube dashboard` to open it.
- View logs, CPU usage, and pod status in your browser.



## Liveness & Readiness Probes
- **Liveness:** Is the pod alive or should I restart it?
- **Readiness:** Is the app ready to handle traffic yet?
- Helps Kubernetes manage traffic to "ready" apps only.



## Next Steps in Your Journey
- **Helm:** Learn the "Package Manager" for Kubernetes.
- **Monitoring:** Look into Prometheus and Grafana.
- **CKA:** Aim for the Certified Kubernetes Administrator certification!
- **End:** Keep experimenting and building!
