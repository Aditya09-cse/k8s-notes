# ⚙️ ConfigMaps, Secrets & Volumes in Kubernetes

You’ve deployed your app using a Deployment — it can scale, update, and roll back ✅  

But there are still some real-world problems:

- ❌ Load configs dynamically  
- ❌ Store sensitive data securely  
- ❌ Persist data across restarts  

👉 Kubernetes solves this using:

- ConfigMaps  
- Secrets  
- Volumes  

---

## 🧩 1. ConfigMap — For Configuration Data

A **ConfigMap** is used to store **non-sensitive configuration data**.

---

### 🔹 You can store:
- Key-value pairs  
- Environment variables  
- Application configs  

📌 Not meant for sensitive data  

---

### 🧪 Example

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: "production"
  APP_PORT: "8080"
```

---

### 🔹 Use in a Pod

```yaml
envFrom:
  - configMapRef:
      name: app-config
```

👉 This injects:
- `APP_MODE`
- `APP_PORT`

as environment variables inside the container  

---

## 🔐 2. Secret — For Sensitive Data

A **Secret** is used to store **sensitive information securely**.

---

### 🔹 Used for:
- Passwords  
- API tokens  
- TLS certificates  

📌 Data is base64-encoded (and can be encrypted at rest)

---

### 🧪 Example

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=     
  password: cGFzc3dvcmQ=
```

(Base64 for `admin` and `password`)

---

### 🔹 Use in a Pod

```yaml
env:
  - name: DB_USER
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: username
```

👉 Injects sensitive data **without exposing it in plain text**

---

## 💾 3. Volumes — For Storage & Data Sharing

A **Volume** provides storage to containers inside a Pod.

---

### 🔹 Used for:
- Persisting data across restarts  
- Sharing data between containers  
- Mounting ConfigMaps/Secrets as files  

---

### 🧪 Example: Mount ConfigMap as Files

```yaml
volumes:
  - name: config-volume
    configMap:
      name: app-config

containers:
  - name: app
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config
```

---

### 🔹 Result:

Each key in the ConfigMap becomes a file:

```
/etc/config/APP_MODE
/etc/config/APP_PORT
```

---

## 📦 Common Volume Types

| Volume Type | Use Case |
|------------|----------|
| emptyDir | Temporary storage (cleared on restart) |
| hostPath | Mount node filesystem (use carefully) |
| configMap | Mount config as files |
| secret | Mount secrets as files |
| persistentVolumeClaim | Durable storage (DBs, logs) |

---

## 📊 Summary

| Feature | Purpose | Common Use Case |
|--------|--------|----------------|
| ConfigMap | Store config data | Env vars, app settings |
| Secret | Store sensitive data | API keys, passwords |
| Volume | Persist/share data | Files, logs, storage |

---

## 💡 Key Idea

- ConfigMap → **configuration (non-sensitive)**  
- Secret → **secure data (sensitive)**  
- Volume → **storage & file access**  

👉 Together, they make your application **flexible, secure, and stateful**

---

## 🧪 Quick Review

- Which resource is used for API keys? → **Secret**  
- How to mount config as files? → **Volume**  
- Do ConfigMaps store encrypted data? → **No**  

---

## 🎯 Interview Tip

👉 Common question:

**Q: Difference between ConfigMap and Secret?**

👉 Answer:
- ConfigMap stores non-sensitive data  
- Secret stores sensitive data (encoded & secured)  
