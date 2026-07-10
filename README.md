# Build My Custom Docker Image

This project contains a custom Docker image built using the provided `Dockerfile`.

## Navigate to the docker folder

```bash
cd docker
```

## Build the Docker image

```bash
docker build -t maiharshrivastava/my-nginx:v1 .
```

## Verify the image

```bash
docker images
```

Expected output:

```
REPOSITORY                     TAG
maiharshrivastava/my-nginx     v1
```

## Run the custom image

```bash
docker run -d --name my-custom-nginx -p 8081:80 maiharshrivastava/my-nginx:v1
```

Verify that the container is running:

```bash
docker ps
```

Open your browser:

```
http://localhost:8081
```

You should see:

```
Welcome to Maihar's Kubernetes Lab 🚀
```

## Push the image to Docker Hub

Login to Docker Hub:

```bash
docker login
```

Push the image:

```bash
docker push maiharshrivastava/my-nginx:v1
```

Now anyone can download your image using:

```bash
docker pull maiharshrivastava/my-nginx:v1
```

and run it with:

```bash
docker run -d --name my-custom-nginx -p 8081:80 maiharshrivastava/my-nginx:v1
```