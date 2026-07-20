# Kubernetes ConfigMap with Pod Self-Healing

## Project Overview

This project demonstrates how to:

- Create a ConfigMap using a YAML file.
- Store configuration data in a YAML file (`app.yaml`).
- Mount the ConfigMap inside a Kubernetes Pod.
- Access the YAML file from inside the Pod.
- Demonstrate Pod self-healing using a Deployment.

---

# Folder Structure

```text
configmap-demo/
│
├── namespace.yaml
├── configmap.yaml
└── deployment.yaml
```

---

# Files

## namespace.yaml

Creates a namespace named `development`.

```yaml
apiVersion: v1
kind: Namespace

metadata:
  name: development
```

---

## configmap.yaml

Creates a ConfigMap containing a YAML configuration file.

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config
  namespace: development

data:
  app.yaml: |
    application:
      name: Kubernetes Demo
      version: v1
      message: Hello Maihar
      environment: Development
```

---

## deployment.yaml

Creates an Nginx Deployment and mounts the ConfigMap inside the Pod.

```yaml
volumeMounts:
- name: config-volume
  mountPath: /config

volumes:
- name: config-volume
  configMap:
    name: app-config
```

---

# Deployment Steps

## 1. Create Namespace

```bash
kubectl apply -f namespace.yaml
```

---

## 2. Create ConfigMap

```bash
kubectl apply -f configmap.yaml
```

---

## 3. Create Deployment

```bash
kubectl apply -f deployment.yaml
```

---

## 4. Verify Resources

Check Pods:

```bash
kubectl get pods -n development
```

Example:

```
NAME                                READY   STATUS
nginx-deployment-799dd5db44-rjpb8   1/1     Running
```

---

# Access the Pod

Login to the Pod:

```bash
kubectl exec -it <pod-name> -n development -- sh
```

Example:

```bash
kubectl exec -it nginx-deployment-799dd5db44-rjpb8 -n development -- sh
```

---

# Verify Mounted YAML File

Go to the mounted directory:

```bash
cd /config
```

List files:

```bash
ls
```

Output:

```
app.yaml
```

Display the file:

```bash
cat app.yaml
```

Output:

```yaml
application:
  name: Kubernetes Demo
  version: v1
  message: Hello Maihar
  environment: Development
```

---

# Pod Self-Healing

Delete the Pod:

```bash
kubectl delete pod <pod-name> -n development
```

Example:

```bash
kubectl delete pod nginx-deployment-799dd5db44-rjpb8 -n development
```

Watch Kubernetes recreate the Pod:

```bash
kubectl get pods -n development -w
```

Example:

```
NAME                                READY   STATUS
nginx-deployment-799dd5db44-rjpb8   Terminating

nginx-deployment-799dd5db44-x7k9m   Running
```

The Deployment automatically creates a new Pod to maintain the desired state.

---

# Verify After Self-Healing

Login to the newly created Pod:

```bash
kubectl exec -it <new-pod-name> -n development -- sh
```

Navigate to the ConfigMap mount:

```bash
cd /config
```

Read the YAML file:

```bash
cat app.yaml
```

Output:

```yaml
application:
  name: Kubernetes Demo
  version: v1
  message: Hello Maihar
  environment: Development
```

The YAML file is available again because it is mounted from the ConfigMap.

---

# How It Works

```
configmap.yaml
        │
        ▼
ConfigMap
        │
        ▼
Deployment
        │
        ▼
Pod
        │
        ▼
/config/app.yaml
        │
Delete Pod
        ▼
Deployment creates a new Pod
        │
        ▼
ConfigMap mounts app.yaml again
```

---

# Key Concepts

- **ConfigMap** stores configuration data.
- **Deployment** manages Pods and provides self-healing.
- **Volume Mount** makes the ConfigMap available as a file inside the Pod.
- **Pod Self-Healing** ensures a new Pod is created automatically if the current Pod is deleted.

---

# Learning Outcomes

After completing this project, you will understand:

- Kubernetes Namespace
- ConfigMap
- Deployment
- Volume Mount
- YAML Configuration Files
- Pod Self-Healing
- Accessing files inside a Pod
- Kubernetes resource management