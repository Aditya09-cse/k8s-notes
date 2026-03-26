# 🧠 Kubernetes Control Plane

Who actually runs Kubernetes itself?  
That’s the job of the **Control Plane** — the brain of your cluster.

---

## 📌 What is the Control Plane?

The Control Plane is responsible for managing the entire Kubernetes cluster.

### 🔹 It:
- Decides what runs where  
- Monitors the cluster  
- Responds to changes  
- Stores both desired and actual state  

📌 When you run a command like:

```bash
kubectl apply -f app.yaml
```

👉 You are communicating directly with the **Control Plane**

---

## ⚙️ Core Components of Control Plane

Let’s break down each component:

---

## 1️⃣ API Server – Front Door of the Cluster

Every request (like `kubectl`) first goes to the **API Server**.

### 🔹 Responsibilities:
- Validates requests  
- Processes them  
- Stores desired state in etcd  

📌 Think of it as the **entry point to Kubernetes**

---

## 2️⃣ etcd – Cluster Database

`etcd` is a **key-value store** that holds all cluster data.

### 🔹 It stores:
- Cluster state  
- ConfigMaps  
- Secrets  
- Object definitions  

📌 You never interact with etcd directly  
👉 Only the API Server communicates with it  

---

## 3️⃣ Controller Manager – Keeps Things in Sync

The Controller Manager ensures that the **actual state matches the desired state**.

### 🔹 What it does:
- Watches cluster state  
- Detects differences  
- Takes corrective actions  

### 🧪 Example:
You want **3 Pods**, but only **2 are running**  
👉 Controller will create 1 more Pod automatically  

---

## 4️⃣ Scheduler – Decides Where Pods Run

The Scheduler selects the **best Node** for a new Pod.

### 🔹 It considers:
- CPU & memory availability  
- Node labels  
- Affinity / anti-affinity rules  

📌 Important:
- Scheduler only **assigns Pods to Nodes**  
- It does **NOT start them**  

👉 The **kubelet (on Node)** actually creates the Pod  

---

## 5️⃣ Cloud Controller Manager (Optional)

Used in **cloud environments** like AWS, GCP, Azure.

### 🔹 It helps with:
- Creating Load Balancers  
- Managing storage  
- Syncing cloud resources  

📌 Not required for:
- Minikube  
- Kind  
- Local clusters  

---

## 🎯 Summary Snapshot

| Component | Role |
|----------|------|
| API Server | Entry point, validates & processes requests |
| etcd | Stores cluster state |
| Controller Manager | Maintains desired vs actual state |
| Scheduler | Assigns Pods to Nodes |
| Cloud Controller | Handles cloud integrations (optional) |

---

## 💡 Key Idea

The Control Plane is the **brain of Kubernetes**.

- API Server → communication  
- etcd → storage  
- Controller → correction  
- Scheduler → placement  

Together, they keep your cluster running smoothly.

---

## 🧪 Quick Quiz

Match the components:

- Stores every object in the cluster → **etcd**  
- Assigns a Pod to a Node → **Scheduler**  
- You interact with it via kubectl → **API Server**  
- Notices if something goes wrong → **Controller Manager**  

---

## 🎯 Interview Tip

👉 Very common question:

**Q: What are the main components of Kubernetes Control Plane?**  

👉 Answer:
- API Server  
- etcd  
- Controller Manager  
- Scheduler  
- (Optional) Cloud Controller Manager  
