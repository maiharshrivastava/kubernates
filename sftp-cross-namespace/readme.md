# SFTP Communication Across Different Kubernetes Namespaces

## Overview

This project demonstrates how a pod in one Kubernetes namespace securely communicates with an SFTP server running in another namespace using a Kubernetes Service.

* **Frontend Namespace** → Contains the SFTP client pod.
* **Backend Namespace** → Contains the SFTP server deployment and service.
* Communication takes place over **Port 22 (SSH/SFTP)**.

---

## Project Structure

```text
sftp-cross-namespace/
│
├── namespaces/
│   ├── frontend.yaml
│   └── backend.yaml
│
├── deployment/
│   └── sftp-deployment.yaml
│
├── service/
│   └── sftp-service.yaml
│
├── client/
│   └── client-pod.yaml
│
└── README.md
```

---

## Prerequisites

* Minikube
* kubectl
* Docker
* Running Kubernetes cluster

Verify:

```bash
minikube status
kubectl get nodes
```

---

## Step 1: Create Namespaces

```bash
kubectl apply -f namespaces/
```

Verify:

```bash
kubectl get namespaces
```

Expected:

```text
frontend
backend
```

---

## Step 2: Deploy the SFTP Server

```bash
kubectl apply -f deployment/
```

Verify:

```bash
kubectl get pods -n backend
```

Expected:

```text
NAME                           READY   STATUS
sftp-server-xxxxxxxxxx         1/1     Running
```

---

## Step 3: Create the SFTP Service

```bash
kubectl apply -f service/
```

Verify:

```bash
kubectl get svc -n backend
```

Expected:

```text
NAME            TYPE        PORT(S)
sftp-service    ClusterIP   22/TCP
```

---

## Step 4: Create the Client Pod

```bash
kubectl apply -f client/
```

Verify:

```bash
kubectl get pods -n frontend
```

Expected:

```text
NAME
sftp-client
```

---

## Step 5: Access the Client Pod

```bash
kubectl exec -it sftp-client -n frontend -- sh
```

---

## Step 6: Install the SFTP Client

Inside the pod:

```sh
apk add --no-cache openssh-client
```

---

## Step 7: Connect to the SFTP Server

Using the service name:

```sh
sftp demo@sftp-service.backend
```

or

```sh
sftp demo@sftp-service.backend.svc.cluster.local
```

Password:

```text
demo123
```

---

## Step 8: Verify the Connection

Successful connection:

```text
Connected to sftp-service.backend.
sftp>
```

Try the following commands:

```sh
pwd
```

```sh
ls
```

```sh
mkdir test
```

```sh
quit
```

---

## Kubernetes Communication Flow

```text
Frontend Namespace
------------------
sftp-client Pod
       |
       |  Port 22
       |
       v
Backend Namespace
-----------------
sftp-service
       |
       v
sftp-server Pod
```

---

## Kubernetes Objects Used

| Resource   | Purpose                                    |
| ---------- | ------------------------------------------ |
| Namespace  | Separates frontend and backend resources   |
| Deployment | Runs the SFTP server pod                   |
| Service    | Exposes the SFTP server within the cluster |
| Pod        | Runs the SFTP client                       |

---

## Useful Commands

```bash
kubectl get namespaces
```

```bash
kubectl get pods -A
```

```bash
kubectl get svc -A
```

```bash
kubectl describe pod sftp-server -n backend
```

```bash
kubectl logs -n backend deployment/sftp-server
```

---

## Learning Outcome

After completing this project, you will understand:

* Creating multiple Kubernetes namespaces.
* Deploying an SFTP server using a Deployment.
* Exposing applications with a ClusterIP Service.
* Communicating securely across namespaces using Kubernetes DNS.
* Using the DNS format:

  ```
  <service-name>.<namespace>.svc.cluster.local
  ```
* Secure file transfer between pods using the SFTP protocol over SSH (Port 22).
