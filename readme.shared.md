Create a file named **README.md** inside your `shared-data` project folder.

```markdown id="58392"
# Docker Shared Mount Demo

## Overview

This project demonstrates a **shared mount** using Docker.

A single folder from the host machine is mounted into multiple Docker containers. Any file created or modified in one container is immediately visible in another container because both containers use the same shared storage.

---

## Folder Structure

```

kubernates/
│
├── shared-data/
│   ├── test.txt
│   └── shared.txt
│
└── README.md

```

---

## Host Shared Folder

Windows folder:

```

C:\Users\Maihar_nikku\Desktop\kubernates\shared-data

````

This folder is shared with Docker containers.

---

## Run Container 1

Open Terminal 1:

```bash
docker run -it --rm \
-v C:\Users\Maihar_nikku\Desktop\kubernates\shared-data:/data \
ubuntu bash
````

Inside Container 1:

```bash
cd /data
```

Create a file:

```bash
echo "Created from Container 1" > shared.txt
```

Check:

```bash
ls
```

Output:

```
test.txt
shared.txt
```

---

## Run Container 2

Open another terminal:

```bash
docker run -it --rm \
-v C:\Users\Maihar_nikku\Desktop\kubernates\shared-data:/data \
ubuntu bash
```

Inside Container 2:

```bash
cd /data
```

Read the file created by Container 1:

```bash
cat shared.txt
```

Output:

```
Created from Container 1
```

---

## How It Works

```
                 Host Machine
                     |
                     |
        C:\...\shared-data
                     |
        -------------------------
        |                       |
        |                       |
  Container 1              Container 2
     /data                    /data
        |                       |
        -------- same data ------
```

Both containers are connected to the same host directory.

Any change made in one container is reflected in the other.

---

## Verify Sharing

Create a file in Container 1:

```bash
echo "Hello Shared Storage" > demo.txt
```

Check in Container 2:

```bash
cat demo.txt
```

Result:

```
Hello Shared Storage
```

---

## Key Learning

* Docker bind mount connects host storage with containers.
* Multiple containers can use the same mounted directory.
* Changes are immediately visible across containers.
* This concept is similar to Kubernetes shared volumes.

---

## Technologies Used

* Docker
* Ubuntu Docker Image
* Bind Mount
* Shared Storage Concept

```

This README matches the exact Docker shared mount implementation you performed.
```

❌ Not possible: "Everything created anywhere in Container 1 appears in Container 2."
✅ Possible: "Everything created inside a shared directory (for example /shared, /project, or /app) appears in Container 2."