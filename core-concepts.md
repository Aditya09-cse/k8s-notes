# 🧱 Kubernetes Core Concepts

Before you start running commands or writing YAML, there are **5 core concepts** you must understand.

These are the **building blocks of every Kubernetes cluster**.

---

## 📦 1. Container

A container is a **lightweight, isolated environment** used to run your application.

### 🔹 It includes:
- Application binary  
- Required libraries  
- Dependencies  
- Runtime environment  

📌 Containers are managed by a **container runtime** (commonly `containerd`)

### ⚠️ Important
- Kubernetes does **NOT manage containers directly**  
- It manages **Pods**, which run containers  

```
[Container]
 ├── App binary
 ├── Libraries
 └── Dependencies
```

---

## 🧩 2. Pod

A Pod is the **smallest deployable unit in Kubernetes**.

### 🔹 Each Pod:
- Contains at least one container  
- Shares the same IP address and port space  
- Can be restarted automatically if it crashes  

📌 You never deploy containers directly — always deploy **Pods**

```
[POD]
 ├── container-1
 └── container-2 (optional)
```

---

## 🖥️ 3. Node

A Node is a **machine (physical or virtual)** where Pods run.

### 🔹 Each Node includes:
- `kubelet` → communicates with control plane  
- `kube-proxy` → handles networking  
- `containerd` → container runtime  

📌 Pods run on Nodes  
📌 Nodes are part of a Cluster  

```
[Node]
 ├── kubelet
 ├── kube-proxy
 ├── containerd
 └── Pods
     ├── Pod 1
     └── Pod 2
```

---

## 🌐 4. Cluster

A Kubernetes Cluster is the **complete system**.

### 🔹 It consists of:
- Control Plane → manages the system  
- Worker Nodes → run applications  

📌 All operations (deploying, scaling, healing) happen inside the cluster  

```
[Kubernetes Cluster]
 ├── Control Plane (API Server, Scheduler, etc.)
 └── Worker Nodes (running Pods)
```

---

## 🛠️ 5. kubectl

`kubectl` is the **command-line tool** used to interact with Kubernetes.

### 🔹 With kubectl, you can:
- View cluster status  
- Create, update, delete resources  
- Apply YAML configurations  
- Debug issues  

### 🧪 Example Commands

```bash
kubectl get pods
kubectl get all
kubectl apply -f app.yaml
kubectl describe pod <name>
```

📌 Think of `kubectl` as your **remote control for Kubernetes**

---

## 🎯 Quick Recap

| Concept   | What It Is | Why It Matters |
|----------|-----------|---------------|
| Container | Runs your app | What you package |
| Pod | Wraps containers | What Kubernetes deploys |
| Node | Machine running Pods | Where apps run |
| Cluster | Full system | Manages everything |
| kubectl | CLI tool | How you control K8s |

---

## 💡 Key Idea

Kubernetes works in layers:

**Container → Pod → Node → Cluster**

You interact with everything using **kubectl**.

---

## 🧪 Quick Check

- [ ] Do I understand the difference between a container and a Pod?  
- [ ] Do I know where Pods run?  
- [ ] Do I understand what a Node does?  
- [ ] Do I know what a Cluster is?  
- [ ] Have I used basic kubectl commands?  

---

## 🎯 Interview Tip

**Q: What is the smallest unit in Kubernetes?**  
👉 Pod (not container)

---

## 📘 More Reads (Optional)

- Kubernetes Terminology: Pods, Containers, Nodes & Clusters  
- What’s a Pod, Really?  
- Kubernetes Pod  
