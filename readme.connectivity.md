# Kubernetes Namespace Communication Lab

## Project Objective

This project demonstrates:

- Communication between Pods and Services within the same namespace.
- Communication across different namespaces using Kubernetes DNS.
- Service discovery using ClusterIP Services.

---

## Architecture

### Same Namespace Communication

```text
hr-client
    │
    ▼
hr-service
    │
    ▼
hr-web (Nginx)
    │
    ▼
Hello from HR
```

### Cross Namespace Communication

```text
finance-client
      │
      ▼
hr-service.hr
      │
      ▼
hr-service
      │
      ▼
hr-web (Nginx)
      │
      ▼
Hello from HR
```

---

## Namespaces Created

### Create HR Namespace

```bash
kubectl create namespace hr
```

### Create Finance Namespace

```bash
kubectl create namespace finance
```

### Verify Namespaces

```bash
kubectl get ns
```

---

## HR Application Setup

### Create Deployment

```bash
kubectl create deployment hr-web --image=nginx -n hr
```

### Expose Deployment as Service

```bash
kubectl expose deployment hr-web --port=80 --target-port=80 --name=hr-service -n hr
```

### Customize Nginx Page

```bash
kubectl exec -it deployment/hr-web -n hr -- bash
```

```bash
echo "<h1>Hello from HR</h1>" > /usr/share/nginx/html/index.html
```

```bash
exit
```

---

## Finance Application Setup

### Create Deployment

```bash
kubectl create deployment finance-web --image=nginx -n finance
```

### Expose Deployment as Service

```bash
kubectl expose deployment finance-web --port=80 --target-port=80 --name=finance-service -n finance
```

### Customize Nginx Page

```bash
kubectl exec -it deployment/finance-web -n finance -- bash
```

```bash
echo "<h1>Hello from Finance</h1>" > /usr/share/nginx/html/index.html
```

```bash
exit
```

---

## Create Client Pods

### HR Client

```bash
kubectl run hr-client --image=busybox -n hr -- sleep 3600
```

### Finance Client

```bash
kubectl run finance-client --image=busybox -n finance -- sleep 3600
```

---

## Test Same Namespace Communication

### Enter HR Client

```bash
kubectl exec -it hr-client -n hr -- sh
```

### Access HR Service

```bash
wget -O- http://hr-service
```

### Output

```html
<h1>Hello from HR</h1>
```

### Exit

```bash
exit
```

---

## Test Cross Namespace Communication

### Enter Finance Client

```bash
kubectl exec -it finance-client -n finance -- sh
```

### Access Finance Service

```bash
wget -O- http://finance-service
```

### Output

```html
<h1>Hello from Finance</h1>
```

### Access HR Service from Finance Namespace

```bash
wget -O- http://hr-service.hr
```

### Output

```html
<h1>Hello from HR</h1>
```

### Exit

```bash
exit
```

---

## Verification Commands

### Check Pods

```bash
kubectl get pods -n hr
kubectl get pods -n finance
```

### Check Services

```bash
kubectl get svc -n hr
kubectl get svc -n finance
```

### Check Deployments

```bash
kubectl get deployments -n hr
kubectl get deployments -n finance
```

### Describe Service

```bash
kubectl describe svc hr-service -n hr
kubectl describe svc finance-service -n finance
```

---

## Key Concepts

### Deployment

Creates and manages application Pods.

### Service

Provides a stable endpoint for accessing Pods.

### ClusterIP

Allows communication between Pods inside the cluster.

### Namespace

Provides logical separation of resources.

### Kubernetes DNS

Enables service discovery using:

```text
<service-name>
```

within the same namespace and

```text
<service-name>.<namespace>
```

across namespaces.

Example:

```text
hr-service.hr
```

---

## Result

✅ Successfully established communication within the same namespace.

```text
hr-client
    │
    ▼
hr-service
    │
    ▼
hr-web
```

✅ Successfully established communication across namespaces.

```text
finance-client
      │
      ▼
hr-service.hr
      │
      ▼
hr-web
```

The successful `wget` responses confirmed that Kubernetes DNS resolved the Service names correctly and that the Services routed traffic to the target Pods.