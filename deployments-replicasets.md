# 🚀 Deployments & ReplicaSets in Kubernetes

---

## 🔁 Quick Answers Corner

Let’s quickly recap the previous quiz:

### ❓ Q: What does this command return?

```bash
kubectl get pods -l env=prod -n staging
```

👉 **Answer:**  
Lists all Pods in the `staging` namespace with label `env=prod`

---

### ❓ Bonus Question

```bash
kubectl get pods -l 'tier in (frontend,backend)' -n dev \
--field-selector=status.phase=Running
```

👉 **Answer:**  
Lists all **running Pods** in the `dev` namespace where:
- `tier=frontend` OR `tier=backend`

---

## ⚠️ The Real-World Problem

So far, you’ve:
- Created Pods  
- Exposed them using Services  

But what about real-world scenarios?

- ❓ What if a Pod crashes?  
- ❓ What if you need multiple copies of your app?  
- ❓ What if you want zero-downtime updates?  

👉 This is where **ReplicaSets and Deployments** come in.

---

## 🧩 What is a ReplicaSet?

A **ReplicaSet** ensures that a specified number of identical Pods are always running.

---

### 🔹 What it does:

- Maintains desired number of Pods  
- Recreates Pods if they crash  
- Scales Pods up/down automatically  

---

### 🧪 Example

```yaml
replicas: 3
selector:
  matchLabels:
    app: web
```

👉 Ensures:
- 3 Pods are always running  
- All Pods have label `app=web`  

📌 You usually don’t create ReplicaSets directly  
👉 They are managed by Deployments  

---

## 🚀 What is a Deployment?

A **Deployment** is a higher-level object that manages ReplicaSets.

---

### 🔹 It can:

- Create and manage ReplicaSets  
- Perform rolling updates  
- Roll back to previous versions  
- Define how your app should run  

📌 Think of Deployment as your app’s **control system**

---

## 📦 Example: Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: nginx
          image: nginx:1.21
          ports:
            - containerPort: 80
```

---

### 🔹 What this does:

- Creates a ReplicaSet  
- Runs 3 Pods  
- Uses image `nginx:1.21`  
- Labels all Pods with `app=web`  

---

## 🧬 What is a Pod Template?

The `template:` section inside a Deployment is called the **Pod Template**.

---

### 🔹 It defines:

- Container image  
- Labels  
- Ports  
- Pod configuration  

👉 Every new Pod is created using this template  

📌 If a Pod crashes → Kubernetes recreates it using this template  

---

## 🔄 What Happens During an Update?

Let’s say you update:

```yaml
image: nginx:1.21 → nginx:1.22
```

Then run:

```bash
kubectl apply -f web-deployment.yaml
```

---

### 🔹 Kubernetes will:

1. Create new Pods with the updated image  
2. Wait until they are healthy  
3. Gradually delete old Pods  
4. Complete update with **zero downtime**  

---

## 🔙 Rollback to Previous Version

If something goes wrong:

```bash
kubectl rollout undo deployment web-deployment
```

📌 Kubernetes keeps deployment history  
👉 You can revert anytime  

---

## 🛠️ Useful Commands

```bash
kubectl get deployments
kubectl describe deployment web-deployment
kubectl scale deployment web-deployment --replicas=5
kubectl rollout status deployment web-deployment
kubectl rollout undo deployment web-deployment
```

---

## 📊 Summary

| Concept | What It Does | Why It Matters |
|--------|-------------|---------------|
| ReplicaSet | Keeps Pods running | Auto-healing & scaling |
| Deployment | Manages ReplicaSets | Updates & rollbacks |
| Pod Template | Blueprint for Pods | Used during recreation |

---

## 💡 Key Idea

- ReplicaSet → **keeps Pods alive**  
- Deployment → **manages lifecycle & updates**  
- Pod Template → **defines how Pods are created**  

👉 Together, they make your app **reliable and scalable**

---

## 🧪 Quick Review

- What keeps Pods running? → **ReplicaSet**  
- What manages updates? → **Deployment**  
- Where do recreated Pods come from? → **Pod Template**  

---

## 🎯 Interview Tip

👉 Common question:

**Q: Difference between Deployment and ReplicaSet?**

👉 Answer:
- ReplicaSet ensures the number of Pods  
- Deployment manages ReplicaSets and handles updates/rollbacks  
