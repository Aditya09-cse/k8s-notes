# 📌 Kubernetes Cheat Sheet (All Topics)

A quick revision guide covering all core Kubernetes concepts.

---

## 🧱 Core Concepts

- **Container** → Runs your app  
- **Pod** → Smallest deployable unit (wraps containers)  
- **Node** → Machine that runs Pods  
- **Cluster** → Collection of Nodes  
- **kubectl** → CLI to interact with cluster  

---

## 🧠 Control Plane

- **API Server** → Entry point for all requests  
- **etcd** → Stores cluster state  
- **Controller Manager** → Ensures desired = actual  
- **Scheduler** → Assigns Pods to Nodes  
- **Cloud Controller** → Cloud integrations (optional)  

---

## 🔄 Pod Lifecycle (High-Level Flow)

1. `kubectl apply` → API Server  
2. Stored in etcd  
3. Controller detects change  
4. Scheduler selects Node  
5. kubelet creates Pod  
6. Pod runs  

---

## 🌐 Services

- **ClusterIP** → Internal access  
- **NodePort** → External via Node IP  
- **LoadBalancer** → External via cloud  
- **ExternalName** → Maps to external DNS  

📌 Services provide **stable access to Pods**

---

## 🏷️ Labels, Selectors, Namespaces

- **Label** → Key-value pair  
- **Selector** → Finds resources using labels  
- **Namespace** → Logical separation  

```bash
kubectl get pods -l app=web -n dev
```

---

## 🚀 Deployments & ReplicaSets

- **ReplicaSet** → Maintains number of Pods  
- **Deployment** → Manages ReplicaSets  

### Key Features:
- Scaling  
- Rolling updates  
- Rollbacks  

```bash
kubectl scale deployment app --replicas=3
kubectl rollout undo deployment app
```

---

## ⚙️ ConfigMaps, Secrets, Volumes

### 🔹 ConfigMap
- Non-sensitive config  
- Inject as env or file  

### 🔹 Secret
- Sensitive data (passwords, tokens)  

### 🔹 Volume
- Persistent or shared storage  

---

## 🩺 Probes

- **Liveness** → Restart if dead  
- **Readiness** → Control traffic  
- **Startup** → Delay checks  

---

## 🛠️ Useful kubectl Commands

```bash
kubectl get pods
kubectl describe pod <name>
kubectl logs <pod-name>
kubectl apply -f file.yaml
kubectl delete -f file.yaml
kubectl exec -it <pod-name> -- /bin/sh
kubectl get all
```

---

## 🔍 Debugging

```bash
kubectl describe pod <pod>
kubectl logs <pod>
kubectl get events
```

---

## 📦 YAML Basics

```yaml
apiVersion:
kind:
metadata:
spec:
```

---

## 💡 Key Ideas to Remember

- Kubernetes works on **desired state**  
- Everything is **declarative (YAML)**  
- Pods are **ephemeral**  
- Services provide **stable networking**  
- Deployments manage **lifecycle & updates**  

---

## 🎯 Interview Quick Points

- Smallest unit → Pod  
- Control Plane components → API Server, etcd, Scheduler, Controller  
- Why Services? → Stable access  
- Deployment vs ReplicaSet → Management + updates  
- ConfigMap vs Secret → Non-sensitive vs sensitive  

---

## 🚀 Final Mental Model

```
User → kubectl → API Server → etcd  
        ↓  
Controller → Scheduler → Node → kubelet → Pod  
```

---

## 🧪 Quick Self-Test

- Can you explain Pod vs Container?  
- Can you describe `kubectl apply` flow?  
- Can you deploy an app with Service?  
- Do you understand probes?  

👉 If yes — you're Kubernetes-ready 🚀
