````markdown
# Kubernetes Volume Mount Example

## 📌 Project Overview

This project demonstrates how to create and attach a custom volume mount inside a Kubernetes Pod.

In this example:

- Created an `emptyDir` volume
- Mounted the volume inside the container
- Verified the mount using Linux commands
- Stored data inside the mounted directory

---

# 📂 Project Structure

```text
kubernates/
│
└── volumes/
    │
    ├── volume-pod.yaml
    └── README.md
````

---

# 📝 Kubernetes YAML Configuration

## volume-pod.yaml

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: volume-pod
  namespace: hr

spec:
  containers:
  - name: nginx-container
    image: nginx

    volumeMounts:
    - name: maihar-volume
      mountPath: /maihar

  volumes:
  - name: maihar-volume
    emptyDir: {}
```

---

# 🚀 Deployment Steps

## 1. Create Namespace

```bash
kubectl create namespace hr
```

---

## 2. Deploy Pod

Apply the YAML file:

```bash
kubectl apply -f volumes/volume-pod.yaml
```

Output:

```text
pod/volume-pod created
```

---

## 3. Check Pod Status

```bash
kubectl get pods -n hr
```

Example output:

```text
NAME          READY   STATUS
volume-pod    1/1     Running
```

---

# 🔑 Access Container

Login inside the pod:

```bash
kubectl exec -it volume-pod -n hr -- sh
```

Now you are inside the container.

---

# 📦 Understanding Volume Configuration

## Volume Definition

```yaml
volumes:
- name: maihar-volume
  emptyDir: {}
```

Explanation:

| Field           | Meaning                                   |
| --------------- | ----------------------------------------- |
| `maihar-volume` | Kubernetes volume name                    |
| `emptyDir`      | Temporary storage created when Pod starts |

---

## Volume Mount

```yaml
volumeMounts:
- name: maihar-volume
  mountPath: /maihar
```

Explanation:

| Field       | Meaning                                             |
| ----------- | --------------------------------------------------- |
| `name`      | Connects container with volume                      |
| `mountPath` | Directory inside container where volume is attached |

---

# 🔍 Verify Volume Mount

Inside the pod run:

```bash
df -Ph
```

Example output:

```text
Filesystem      Size   Used   Avail   Mounted on
/dev/sdd        1007G  49G    907G    /maihar
```

Explanation:

| Value      | Meaning                                    |
| ---------- | ------------------------------------------ |
| `/dev/sdd` | Storage device created by Linux/Kubernetes |
| `/maihar`  | Mount point inside container               |

---

# 🧪 Test Volume Storage

Go to mounted directory:

```bash
cd /maihar
```

Create a file:

```bash
echo "Hello Maihar" > test.txt
```

Read the file:

```bash
cat test.txt
```

Output:

```text
Hello Maihar
```

---

# 🔗 Check Mount Information

Command:

```bash
mount | grep maihar
```

Example output:

```text
/dev/sdd on /maihar type ext4
```

---

# 🔄 Volume Mount Flow

```text
Kubernetes Volume
        |
        |
        v
 maihar-volume
        |
        |
        v
    /dev/sdd
        |
        |
        v
     /maihar
        |
        |
        v
    test.txt
```

---

# ⚠️ Important Note

Kubernetes Pods are immutable.

After Pod creation, you cannot modify:

* Volume name
* Volume type
* Mount path
* Container name

If changes are required, delete and recreate the Pod.

---

## Delete Existing Pod

```bash
kubectl delete pod volume-pod -n hr
```

---

## Create Pod Again

```bash
kubectl apply -f volumes/volume-pod.yaml
```

---

# 🧹 Cleanup

Delete Pod:

```bash
kubectl delete pod volume-pod -n hr
```

Delete Namespace:

```bash
kubectl delete namespace hr
```

---

# 🎯 Learning Outcomes

After completing this project, you understand:

* ✅ Kubernetes Volumes
* ✅ `emptyDir` Volume
* ✅ `volumeMounts`
* ✅ `mountPath`
* ✅ Linux mount concept
* ✅ Checking storage using `df -Ph`
* ✅ Checking mount details using `mount`

---

# 📚 Commands Summary

| Command                                   | Purpose                     |
| ----------------------------------------- | --------------------------- |
| `kubectl apply -f file.yaml`              | Create Kubernetes resources |
| `kubectl get pods -n hr`                  | Check Pod status            |
| `kubectl exec -it pod -n namespace -- sh` | Access container shell      |
| `df -Ph`                                  | Check mounted storage       |
| `mount`                                   | Display mount information   |
| `kubectl delete pod`                      | Delete Pod                  |

```
```

Add this section to your `README.md` after **Volume Configuration** section:

````markdown
---

# 📏 Defining Volume Size

By default, an `emptyDir` volume uses the available storage space on the Kubernetes node.

Example:

```yaml
volumes:
- name: maihar-volume
  emptyDir: {}
````

In this case, Kubernetes does not set any storage limit.

---

## Set Volume Size Using sizeLimit

We can define a storage limit using `sizeLimit`.

Example:

```yaml
volumes:
- name: maihar-volume
  emptyDir:
    sizeLimit: 1Gi
```

Now the volume can use maximum:

```
1Gi = 1 Gigabyte
```

---

## Common Storage Units

| Unit    | Meaning       |
| ------- | ------------- |
| `500Mi` | 500 Megabytes |
| `1Gi`   | 1 Gigabyte    |
| `5Gi`   | 5 Gigabytes   |
| `10Gi`  | 10 Gigabytes  |
| `100Gi` | 100 Gigabytes |

---

# 💾 Persistent Storage (PV/PVC)

`emptyDir` is temporary storage.

If the Pod is deleted:

```
Pod Deleted
     |
     ↓
emptyDir Data Deleted
```

For permanent storage, Kubernetes uses:

```
PersistentVolume (PV)
          |
          ↓
PersistentVolumeClaim (PVC)
          |
          ↓
Pod
```

---

## PersistentVolumeClaim Example

```yaml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: maihar-pvc
  namespace: hr

spec:
  accessModes:
  - ReadWriteOnce

  resources:
    requests:
      storage: 5Gi
```

Here Kubernetes requests:

```
Storage = 5Gi
```

---

# 🔍 Volume Types Comparison

| Volume Type | Size Configuration | Data After Pod Delete |
| ----------- | ------------------ | --------------------- |
| `emptyDir`  | `sizeLimit`        | ❌ Deleted             |
| `hostPath`  | Uses Node Disk     | ✅ Remains             |
| `PV/PVC`    | `storage: 5Gi`     | ✅ Remains             |

---

# 📌 Key Points

* `emptyDir` is mainly used for temporary storage.
* `sizeLimit` controls maximum space for `emptyDir`.
* PV/PVC is used for permanent application data.
* Volume size is controlled during storage provisioning.

```

This will make your README cover **volume creation + mount + size management + persistent storage concepts**.
```
