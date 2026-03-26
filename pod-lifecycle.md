# 🔄 From YAML to Running Pod (Kubernetes Workflow)

Before we dive into today’s lesson, let’s quickly recap the previous quiz.

---

## 🔍 Quiz Recap & Answers

- Stores every object in the cluster → ✅ etcd  
- Assigns a Pod to a Node → ✅ Scheduler  
- You interact with it via kubectl → ✅ API Server  
- Notices if a Node goes down → ✅ Controller Manager  

🎯 If you got **3 or more correct** — you're making great progress in Kubernetes 🚀  

---

## 📌 What Happens After You Run a Command?

You’ve probably run something like:

```bash
kubectl apply -f my-pod.yaml
```

But what actually happens behind the scenes?

Let’s walk through the **step-by-step journey** from YAML to a running Pod.

---

## ⚙️ Step-by-Step Flow

---

### 1️⃣ YAML Hits the API Server

Your Pod definition is sent to the **API Server**.

### 🔹 What it does:
- Validates YAML (syntax, required fields)  
- Returns error if invalid  
- Accepts request if valid  

📌 All Kubernetes requests go through the API Server  

---

### 2️⃣ State Stored in etcd

Once validated, the API Server stores the desired state:

> "A Pod named `my-pod` should exist"

This is saved in **etcd**, the cluster database.

---

### 3️⃣ Controller Detects Missing Pod

The **Controller Manager** continuously watches cluster state.

### 🔹 It notices:
- Pod is defined in etcd  
- But not actually running  

👉 So it triggers Pod creation  

---

### 4️⃣ Scheduler Selects a Node

The **Scheduler** decides where the Pod should run.

### 🔹 It checks:
- CPU & memory availability  
- Node labels  
- Affinity / anti-affinity  
- Taints & tolerations  

📌 Important:
- Scheduler only assigns the Pod  
- It does NOT create it  

---

### 5️⃣ Kubelet Creates the Pod

The **kubelet** (on the selected Node) takes over.

### 🔹 It:
- Pulls container image (via containerd)  
- Starts container(s)  
- Monitors Pod health  

📌 If something fails (e.g., image not found), kubelet reports it  

---

### 6️⃣ Pod is Running 🎉

Once everything is successful:

- Pod enters **Running** state  
- You can verify using:

```bash
kubectl get pods
```

---

## 📊 Full Flow Summary

| Step | What Happens | Component |
|------|------------|----------|
| 1 | YAML sent | kubectl → API Server |
| 2 | State stored | API Server → etcd |
| 3 | Pod needed detected | Controller Manager |
| 4 | Node selected | Scheduler |
| 5 | Pod created | kubelet (Node) |
| 6 | Pod running | kubelet + containerd |

---

## 🧪 Try These Commands

```bash
kubectl apply -f my-pod.yaml
kubectl get pods
kubectl describe pod my-pod
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## 💡 Key Idea

Kubernetes works in a loop:

**You define → Kubernetes stores → Controllers act → Scheduler assigns → Kubelet runs**

Everything is automated to match your desired state.

---

## 🧪 Quick Quiz

👉 You applied a Pod, but the image failed to pull.

**Which component reports this error?**

(Hint: It runs on the Node 👀)

---

## 📘 Learn More (Optional)

### 📖 Read
- What Are Pods in Kubernetes? (KodeKloud Blog)

### 🎥 Watch
- Kubernetes Pod Definition with YAML (YouTube)

### 🛠️ Practice
- KodeKloud Free Pods Lab (hands-on practice without setup)

---

## 🎯 Interview Tip

👉 Common scenario-based question:

**Q: What happens internally when you run `kubectl apply`?**

👉 Answer (short version):
- API Server validates request  
- State stored in etcd  
- Controller detects change  
- Scheduler assigns Node  
- Kubelet creates Pod  
