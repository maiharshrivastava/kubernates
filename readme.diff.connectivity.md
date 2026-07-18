# Kubernetes Pod Communication Across Different Namespaces

## Project Objective

This project demonstrates how a Pod in one Kubernetes namespace communicates with a Pod in another namespace using a **ClusterIP Service** and **Kubernetes DNS**.

---

## Project Structure

```text
different-namespace-communication/
│
├── frontend/
│   └── frontend-client.yaml
│
├── backend/
│   ├── backend-deployment.yaml
│   └── backend-service.yaml
│
└── README.md
```

---

## Architecture

```text
Frontend Namespace
┌────────────────────────┐
│ frontend-client (Pod)  │
└───────────┬────────────┘
            │
            │ http://backend-service.backend
            ▼
Backend Namespace
┌────────────────────────┐
│ backend-service        │
│ (ClusterIP Service)    │
└───────────┬────────────┘
            ▼
┌────────────────────────┐
│ backend-app (Nginx Pod)│
└────────────────────────┘
```

---

## Prerequisites

* Docker Desktop
* Minikube
* kubectl

---

## Step 1: Create Namespaces

```bash
kubectl create namespace frontend

kubectl create namespace backend
```

Verify:

```bash
kubectl get namespaces
```

---

## Step 2: Deploy the Backend Application

Apply the deployment:

```bash
kubectl apply -f backend/backend-deployment.yaml
```

Check the pod:

```bash
kubectl get pods -n backend
```

---

## Step 3: Create the Backend Service

Apply the service:

```bash
kubectl apply -f backend/backend-service.yaml
```

Verify:

```bash
kubectl get svc -n backend
```

---

## Step 4: Deploy the Frontend Client

Apply the client pod:

```bash
kubectl apply -f frontend/frontend-client.yaml
```

Verify:

```bash
kubectl get pods -n frontend
```

---

## Step 5: Access the Frontend Pod

```bash
kubectl exec -it frontend-client -n frontend -- sh
```

---

## Step 6: Test Communication

Inside the frontend pod, run:

```bash
wget -O- http://backend-service.backend
```

or

```bash
wget -O- http://backend-service.backend.svc.cluster.local
```

---

## Expected Output

```text
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```

---

## How It Works

1. Two namespaces were created: **frontend** and **backend**.
2. An Nginx application was deployed in the **backend** namespace.
3. A ClusterIP Service named **backend-service** exposed the backend pod.
4. A client pod was created in the **frontend** namespace.
5. The frontend client sent a request to **backend-service.backend**.
6. Kubernetes DNS resolved the service name and forwarded the request to the backend pod.
7. The backend pod returned the Nginx welcome page, confirming successful communication.

---

## Commands Used

```bash
kubectl create namespace frontend

kubectl create namespace backend

kubectl apply -f backend/backend-deployment.yaml

kubectl apply -f backend/backend-service.yaml

kubectl apply -f frontend/frontend-client.yaml

kubectl get pods -n backend

kubectl get svc -n backend

kubectl get pods -n frontend

kubectl exec -it frontend-client -n frontend -- sh

wget -O- http://backend-service.backend
```

---

## Key Learning

* Created multiple Kubernetes namespaces.
* Deployed applications in separate namespaces.
* Exposed a pod using a ClusterIP Service.
* Used Kubernetes DNS for cross-namespace communication.
* Verified successful communication between namespaces.

---

## Conclusion

This project demonstrates how Kubernetes enables communication between Pods in different namespaces using a ClusterIP Service and namespace-qualified DNS names. It provides a simple example of service discovery and internal networking within a Kubernetes cluster.
