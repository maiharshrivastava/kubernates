# Docker Commands - Day 1

## Check Docker Version

```bash
docker --version
```

## Download nginx Image

```bash
docker pull nginx
```

## List Images

```bash
docker images
```

## Run Container

```bash
docker run -d --name nginx-demo -p 8080:80 nginx
```

## List Running Containers

```bash
docker ps
```