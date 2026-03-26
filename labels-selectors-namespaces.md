# 🏷️ Labels, Selectors & Namespaces in Kubernetes

These three concepts help you **organize, connect, and manage resources** in Kubernetes.

---

## 🏷️ 1. Labels — Add Meaning to Your Objects

A **label** is a simple **key-value pair** attached to Kubernetes objects.

### 🔹 You can add labels to:
- Pods  
- Services  
- Deployments  
- Nodes  
- And more  

### 🧪 Example

```yaml
labels:
  app: web
  env: prod
```

👉 This object now has:
- `app=web`  
- `env=prod`  

📌 Labels are not just for grouping  
👉 They are the **glue** that connects:
- Services → Pods  
- Deployments → Pods  
- CLI queries → resources  

---

## 🔍 2. Selectors — Find Resources by Labels

A **selector** tells Kubernetes:

> "Find all objects matching these labels"

---

### 🔹 Used by:
- Services (to find Pods)  
- Deployments (to manage Pods)  
- kubectl (to filter resources)  

### 🧪 Example

```yaml
selector:
  app: web
```

👉 Matches all Pods with:
```
app=web
```

---

### 💻 CLI Example

```bash
kubectl get pods -l app=web
```

👉 Returns all Pods with label `app=web`

---

## 🧩 3. Namespaces — Organize and Isolate

A **namespace** is like a **virtual cluster inside your cluster**.

---

### 🔹 Why use namespaces?

- Organize resources (dev, staging, prod)  
- Avoid naming conflicts  
- Apply security and access control  

📌 Every Kubernetes object exists inside a namespace  
(Default is `default`)

---

### 💻 CLI Examples

```bash
kubectl get namespaces
kubectl get pods -n dev
kubectl create namespace staging
```

---

### 📌 Default Namespaces

- `default` → normal resources  
- `kube-system` → internal Kubernetes components  
- `kube-public` → publicly accessible info  
- `kube-node-lease` → node heartbeat tracking  

---

## 📊 Summary

| Concept | What It Is | Why It Matters |
|--------|-----------|---------------|
| Label | Key-value pair | Group & identify resources |
| Selector | Label query | Connect & filter resources |
| Namespace | Virtual cluster | Isolation & organization |

---

## 💡 Key Idea

- Labels → **identify resources**  
- Selectors → **find resources**  
- Namespaces → **organize resources**  

Together, they make Kubernetes **manageable at scale**.

---

## 🧪 Quick Test

👉 What does this command return?

```bash
kubectl get pods -l env=prod -n staging
```

💭 Answer:
All Pods in the `staging` namespace with label `env=prod`

---

## 🚀 Bonus Challenge

👉 What does this command do?

```bash
kubectl get pods -l 'tier in (frontend,backend)' -n dev \
--field-selector=status.phase=Running
```

💭 Explanation:
- Selects Pods where:
  - `tier=frontend` OR `tier=backend`  
- Inside namespace `dev`  
- Only Pods in **Running state**  

👉 Returns only active Pods matching those conditions  

---

## 🎯 Interview Tip

👉 Common question:

**Q: Difference between labels and selectors?**

👉 Answer:
- Labels are attached to resources  
- Selectors are used to find those resources using labels  
