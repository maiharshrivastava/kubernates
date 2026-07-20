# Kubernetes NetworkPolicy Demo

## Overview

This project demonstrates Kubernetes NetworkPolicy where:

- Client pod can communicate with Server pod.
- Blocked pod cannot communicate with Server pod.
- Network traffic is controlled using pod labels.

---

## Architecture

```
                    NetworkPolicy

client pod  -------------------->  server pod
(app=client)        ALLOW          (app=server)


blocked pod  ------------------>  server pod
(app=blocked)        DENY          (app=server)
```

---

## Project Structure

```
network-policy-demo/

├── namespaces/
│   └── demo-namespace.yaml
│
├── deployment/
│   ├── server-deployment.yaml
│   ├── client-deployment.yaml
│   └── blocked-deployment.yaml
│
├── service/
│   └── server-service.yaml
│
├── network-policy/
│   └── allow-client-only.yaml
│
└── README.md
```

---

# Components

## 1. Namespace

Namespace created:

```
demo
```

All Kubernetes resources are deployed inside this namespace.

---

## 2. Server Deployment

File:

```
deployment/server-deployment.yaml
```

Creates an Nginx server pod.

Label:

```
app: server
```

This pod is protected by NetworkPolicy.

---

## 3. Client Deployment

File:

```
deployment/client-deployment.yaml
```

Creates a client pod.

Label:

```
app: client
```

This pod is allowed to access the server pod.

---

## 4. Blocked Deployment

File:

```
deployment/blocked-deployment.yaml
```

Creates a blocked pod.

Label:

```
app: blocked
```

This pod is not allowed to access the server pod.

---

## 5. Service

File:

```
service/server-service.yaml
```

Creates a Kubernetes Service:

```
server-service
```

It provides network access to the server pod.

---

# NetworkPolicy

File:

```
network-policy/allow-client-only.yaml
```

Policy:

- Applies to pods with:

```
app: server
```

- Allows traffic only from:

```
app: client
```

Rule:

```
client pod  ---> server pod   ✅ Allowed

blocked pod ---> server pod   ❌ Denied
```

---

# Deployment Steps

## 1. Create Namespace

```bash
kubectl apply -f namespaces/demo-namespace.yaml
```

---

## 2. Deploy Applications

```bash
kubectl apply -f deployment/
```

---

## 3. Create Service

```bash
kubectl apply -f service/
```

---

## 4. Apply NetworkPolicy

```bash
kubectl apply -f network-policy/
```

---

# Verify Resources

Check pods:

```bash
kubectl get pods -n demo
```

Check service:

```bash
kubectl get svc -n demo
```

Check NetworkPolicy:

```bash
kubectl get networkpolicy -n demo
```

---

# Testing

## Test Allowed Connection

Command:

```bash
kubectl exec -it deployment/client -n demo -- wget -qO- http://server-service
```

Expected:

```
Welcome to nginx!
```

Result:

```
Client pod can access Server pod ✅
```

---

## Test Blocked Connection

Command:

```bash
kubectl exec -it deployment/blocked -n demo -- wget -qO- http://server-service
```

Expected:

```
wget: can't connect to remote host: Connection timed out
```

Result:

```
Blocked pod cannot access Server pod ❌
```

---

# How It Works

Kubernetes NetworkPolicy uses labels to control communication.

Server pod:

```
app=server
```

allows only:

```
app=client
```

Any other pod without the allowed label will be blocked.

---

# Important Note

NetworkPolicy requires a CNI plugin that supports policy enforcement.

Examples:

- Calico
- Cilium
- Weave Net

For Minikube, start with Calico:

```bash
minikube start --cni=calico
```

---

# Result

Successfully implemented Kubernetes NetworkPolicy:

✅ Pod-to-pod communication control  
✅ Label-based access rules  
✅ Allowed and denied traffic testing  
✅ Secure Kubernetes networking