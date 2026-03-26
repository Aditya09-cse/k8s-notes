# 🩺 Kubernetes Probes (Liveness, Readiness, Startup)

You’ve deployed your app. It runs, scales, and even stores data ✅  

But Kubernetes still asks an important question:

👉 **Is your app actually working?**

---

## ❓ What Could Go Wrong?

- Is the app crashed internally?  
- Is it still starting up?  
- Is it unable to handle traffic?  

📌 Kubernetes doesn’t assume — it verifies.

👉 That’s where **Probes** come in.

---

## ⚙️ Types of Probes

| Probe Type | Checks | What Happens on Failure |
|-----------|--------|------------------------|
| Liveness | Is the app alive? | Container is restarted |
| Readiness | Is app ready for traffic? | Removed from Service |
| Startup | Is app fully started? | Delays other probes |

📌 Probes help Kubernetes decide when to **trust, stop, or restart** your app.

---

## ❤️ 1. Liveness Probe — "Is it still alive?"

Used to detect if your app has **crashed or become unresponsive**.

---

### 🔹 Behavior:
- If it fails → container is restarted  

---

### 🧪 Example (HTTP Check)

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

👉 Checks `/healthz` every 5 seconds  
👉 If it fails → Pod is restarted  

---

## 🚦 2. Readiness Probe — "Can it handle traffic?"

Checks if your app is **ready to serve requests**.

---

### 🔹 Behavior:
- If it fails → Pod stays running  
- But removed from Service (no traffic sent)

---

### 🧪 Example (TCP Check)

```yaml
readinessProbe:
  tcpSocket:
    port: 5432
  initialDelaySeconds: 5
  periodSeconds: 10
```

---

### 📌 Important

Even if you see:

```bash
kubectl get pods
```

👉 Pod may show:
```
Running (0/1 Ready)
```

Meaning:
- App is running  
- But not ready to receive traffic  

---

## ⏳ 3. Startup Probe — "Is it still booting?"

Used for **slow-starting applications** (Java apps, databases, etc.)

---

### 🔹 Behavior:
- Runs only during startup  
- Blocks Liveness & Readiness until success  

---

### 🧪 Example

```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 5
```

👉 Gives app up to **150 seconds** to start  

---

## 🔄 Probe Lifecycle Flow

1. Pod starts  
2. Startup Probe runs  
   - ❌ Fail → restart  
   - ✅ Pass → continue  
3. Liveness & Readiness begin  
4. Liveness fails → restart container  
5. Readiness fails → remove from Service  

---

## ⚙️ Common Probe Fields

| Field | Description |
|------|------------|
| initialDelaySeconds | Wait before first check |
| periodSeconds | Time between checks |
| timeoutSeconds | Max wait time for response |
| failureThreshold | Failures before action |
| successThreshold | Successes to mark healthy |

---

## 💡 Key Idea

- Liveness → **Should I restart it?**  
- Readiness → **Should I send traffic?**  
- Startup → **Should I wait longer?**  

👉 Together, they ensure your app is **healthy, stable, and reliable**

---

## 🧪 Quick Review

- Which probe prevents early traffic? → **Readiness**  
- Which delays health checks until startup? → **Startup**  
- Which restarts containers on failure? → **Liveness**  

---

## 🎯 Interview Tip

👉 Common question:

**Q: Difference between Liveness and Readiness Probe?**

👉 Answer:
- Liveness → checks if app is alive (restarts if failed)  
- Readiness → checks if app is ready (removes from traffic if failed)  
