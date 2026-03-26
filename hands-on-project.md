# 🚀 Kubernetes Hands-On Project (End-to-End App Deployment)

Now that you understand Kubernetes fundamentals, it's time to **put everything together**.

Let’s deploy a simple real-world application using multiple Kubernetes concepts.

---

## 📦 What You’ll Deploy

This setup includes:

- Deployment (2 NGINX Pods)  
- ConfigMap (configuration)  
- Secret (credentials)  
- Liveness & Readiness Probes  
- NodePort Service (external access)  

---

## 🧩 Step 1: ConfigMap

**File: `configmap.yaml`**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: "production"
```

👉 Injects configuration like:
```
APP_MODE=production
```

---

## 🔐 Step 2: Secret

**File: `secret.yaml`**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  username: YWRtaW4=
  password: c2VjdXJlcGFzcw==
```

👉 Stores credentials securely (base64 encoded)

---

## 🚀 Step 3: Deployment with Probes

**File: `deployment.yaml`**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 2
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

          envFrom:
            - configMapRef:
                name: app-config

          env:
            - name: ADMIN_USER
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: username
            - name: ADMIN_PASS
              valueFrom:
                secretKeyRef:
                  name: app-secret
                  key: password

          livenessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 10
            failureThreshold: 3

          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            failureThreshold: 2
```

---

## 🌐 Step 4: Service (NodePort)

**File: `service.yaml`**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: NodePort
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

👉 Access your app at:

```
http://<Node-IP>:30080
```

---

## 🛠️ Deploy Everything

```bash
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## 🔍 Useful Debug Commands

```bash
kubectl get pods -w
kubectl describe pod <pod-name>
kubectl logs <pod-name>

kubectl get service web-service

kubectl port-forward service/web-service 8080:80

kubectl exec -it <pod-name> -- env

kubectl get pods
kubectl describe pod PODNAME
```

---

## ✅ What You Just Built

- Scalable app (Deployment)  
- Configurable (ConfigMap)  
- Secure (Secret)  
- Health-checked (Probes)  
- Externally accessible (Service)  

👉 This is the **foundation of real-world Kubernetes applications**

---

## 💡 Key Idea

You combined multiple Kubernetes components into a **working system**:

**Config + Secrets + Deployment + Probes + Service = Production-ready app**

---

## 🧪 Quick Review

- Where is config stored? → ConfigMap  
- Where are credentials stored? → Secret  
- What ensures app health? → Probes  
- What exposes the app? → Service  

---

## 🗺️ What Next? (Kubernetes Roadmap)

---

### 🔹 Phase 1: YAML Mastery
- Write manifests from scratch  
- Explore Service types  
- Mount ConfigMaps & Secrets as files  

---

### 🔹 Phase 2: Tools
- Helm  
- K9s  
- Lens  
- kubectl exec  

---

### 🔹 Phase 3: Cluster Concepts
- Resource limits  
- Affinity / Taints  
- Namespaces  
- Network policies  

---

### 🔹 Phase 4: Security
- RBAC  
- Secrets best practices  
- Audit logging  

---

### 🔹 Phase 5: Real Clusters
- Minikube / Kind  
- KodeKloud labs  
- Deploy on GKE / EKS / AKS  

---

## 🚀 Go Beyond

| If you liked this | Try this next |
|------------------|-------------|
| YAML basics | Helm / Kustomize |
| Pods & configs | StatefulSets |
| Services | Ingress |
| Manual deploy | CI/CD (ArgoCD, Flux) |
| Core concepts | KCNA / CKA |

---

## 🎉 Congratulations!

You’ve completed a **full Kubernetes learning journey**:

- From basics → to real deployment  
- From theory → to hands-on  

👉 You now understand Kubernetes better than most beginners.

Now go build something real 💪
