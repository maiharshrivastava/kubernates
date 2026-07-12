

# Docker Setup and Custom Docker Image

## Step 1: Install Required Software

Install the following software on your local machine:

* Docker Desktop
* Git
* Visual Studio Code
* Minikube
* kubectl

Verify the installation:

```bash
docker --version
kubectl version --client
minikube version
git --version
```

---

## Step 2: Create a GitHub Repository

Create a GitHub repository.

Example:

```text
kubernates
```

Clone the repository:

```bash
git clone https://github.com/maiharshrivastava/kubernates.git
```

Navigate to the project:

```bash
cd kubernates
```

Open the project in Visual Studio Code.

---

## Step 3: Pull the Official Nginx Image

Download the official Nginx image from Docker Hub.

```bash
docker pull nginx
```

Verify the image:

```bash
docker images
```

---

## Step 4: Create Your First Docker Container

Run a container using the Nginx image.

```bash
docker run -d --name nginx-demo -p 8080:80 nginx
```

Verify the running container:

```bash
docker ps
```

Open your browser:

```text
http://localhost:8080
```

Expected Output:

```text
Welcome to nginx!
```

---

## Step 5: View Container Logs

```bash
docker logs nginx-demo
```

---

## Step 6: Access the Running Container

Open a shell inside the running container.

```bash
docker exec -it nginx-demo bash
```

Execute basic Linux commands:

```bash
pwd
ls
exit
```

---

## Step 7: Create the Docker Folder

Inside the project, create a folder named:

```text
docker
```

---

## Step 8: Create Required Files

Inside the `docker` folder, create the following files:

```text
Dockerfile
index.html
commands.md
```

---

## Step 9: Project Structure

After creating the Docker folder, the project structure becomes:

```text
kubernates/
│
├── README.md
│
└── docker/
    ├── Dockerfile
    ├── index.html
    └── commands.md
```

---

## Step 10: Create the Dockerfile

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html
```

---

## Step 11: Create index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Maihar Kubernetes Lab</title>
</head>

<body>
    <h1>Welcome to Maihar's Kubernetes Lab 🚀</h1>
    <p>This page is served from my own Docker image.</p>
</body>
</html>
```

---

## Step 12: Navigate to the Docker Folder

```bash
cd docker
```

Verify the files:

**Windows**

```bash
dir
```

**Linux/macOS**

```bash
ls
```

---

## Step 13: Build the Custom Docker Image

Build the image using the Dockerfile.

```bash
docker build -t maiharshrivastava/my-nginx:v1 .
```

Verify the image:

```bash
docker images
```

Expected output:

```text
REPOSITORY                     TAG
maiharshrivastava/my-nginx     v1
```

---

## Step 14: Run the Custom Docker Image

Create a container using the custom image.

```bash
docker run -d --name my-custom-nginx -p 8081:80 maiharshrivastava/my-nginx:v1
```

Verify the running container.

```bash
docker ps
```

Open your browser:

```text
http://localhost:8081
```

Expected Output:

```text
Welcome to Maihar's Kubernetes Lab 🚀
This page is served from my own Docker image.
```

---

## Step 15: Push the Image to Docker Hub

Login to Docker Hub.

```bash
docker login
```

Push the custom image.

```bash
docker push maiharshrivastava/my-nginx:v1
```

---

## Step 16: Save the Project to GitHub

Commit and push the project.

```bash
git add .
git commit -m " Docker Basics and Custom Docker Image"
git push
```

---

## Step 17: Reproduce the Project on Another Machine

### Clone the repository

```bash
git clone https://github.com/maiharshrivastava/kubernates.git
```

Navigate to the project.

```bash
cd kubernates
```

### Option 1: Build the Image

```bash
cd docker
docker build -t maiharshrivastava/my-nginx:v1 .
docker run -d --name my-custom-nginx -p 8081:80 maiharshrivastava/my-nginx:v1
```

### Option 2: Pull the Image from Docker Hub

```bash
docker pull maiharshrivastava/my-nginx:v1
docker run -d --name my-custom-nginx -p 8081:80 maiharshrivastava/my-nginx:v1
```

Open:

```text
http://localhost:8081
```

---

