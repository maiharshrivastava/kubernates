# Kubernetes Pod Self-Recovery with Persistent Storage

## Objective

To demonstrate that when a pod is deleted, Kubernetes automatically creates a new pod, and the data remains available because it is stored in persistent storage.

---

# Architecture

```text
                    Deployment
                         |
              Monitors the Pod
                         |
                 -----------------
                 |               |
              Pod (Old)      Pod (New)
                 |               |
                 |               |
                 ------ /workspace ------
                           |
                           |
             PersistentVolumeClaim (PVC)
                           |
                           |
                PersistentVolume (PV)
                           |
                           |
                    /storage-area
```

---

# Step 1: Create PersistentVolume

Apply the PersistentVolume.

```bash
kubectl apply -f pv.yaml
```

Verify:

```bash
kubectl get pv
```

Expected:

```text
NAME         STATUS
storage-pv   Available
```

---

# Step 2: Create PersistentVolumeClaim

Apply the PersistentVolumeClaim.

```bash
kubectl apply -f pvc.yaml
```

Verify:

```bash
kubectl get pvc
```

Expected:

```text
NAME          STATUS
storage-pvc   Bound
```

---

# Step 3: Create Deployment

Deploy the application.

```bash
kubectl apply -f deployment.yaml
```

Verify:

```bash
kubectl get deployments
kubectl get pods
```

Example:

```text
storage-deployment-74699d9d7-7h8fz   Running
```

---

# Step 4: Create a File

Open the pod:

```bash
kubectl exec -it storage-deployment-74699d9d7-7h8fz -- sh
```

Create a file:

```bash
echo "Persistent Data" > /workspace/sample.txt
```

Verify:

```bash
cat /workspace/sample.txt
```

Output:

```text
Persistent Data
```

Exit:

```bash
exit
```

---

# Step 5: Delete the Pod

Delete the running pod.

```bash
kubectl delete pod storage-deployment-74699d9d7-7h8fz
```

Output:

```text
pod "storage-deployment-74699d9d7-7h8fz" deleted
```

---

# Step 6: Observe Self-Recovery

Watch the pods.

```bash
kubectl get pods -w
```

Example:

```text
storage-deployment-74699d9d7-s6nqc   Running
```

A new pod is created automatically by the Deployment.

---

# Step 7: Verify the Data

Read the file from the new pod.

```bash
kubectl exec -it storage-deployment-74699d9d7-s6nqc -- cat /workspace/sample.txt
```

Output:

```text
Persistent Data
```

---

# Explanation

* The Deployment continuously monitors the pod.
* When the pod is deleted, Kubernetes automatically creates a replacement pod.
* The file is stored in the PersistentVolume through the PersistentVolumeClaim.
* The new pod mounts the same storage at `/workspace`.
* Since the data is stored outside the pod, it remains available after the pod is recreated.

---

# Result

* PersistentVolume created successfully.
* PersistentVolumeClaim bound successfully.
* Deployment created one pod.
* Pod deleted successfully.
* Kubernetes automatically created a new pod.
* The file `sample.txt` was still available in the new pod.
* Pod self-recovery and persistent storage were successfully demonstrated.
