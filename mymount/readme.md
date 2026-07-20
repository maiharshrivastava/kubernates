# Manual Linux Bind Mount using Minikube

## Objective

Create a manual bind mount in a Linux environment and verify that it is working.

## Environment

* Windows 11
* Docker Desktop
* Minikube
* Linux (Minikube SSH)

## Steps

### 1. Connect to Minikube

```bash
minikube ssh
```

### 2. Create the source directory

```bash
sudo mkdir -p /data
```

### 3. Create the mount point

```bash
sudo mkdir -p /mymount
```

### 4. Create a sample file

```bash
echo "Hello" | sudo tee /data/file.txt
```

### 5. Verify the file

```bash
cat /data/file.txt
```

Output:

```
Hello
```

### 6. Create the bind mount

```bash
sudo mount --bind /data /mymount
```

### 7. Verify the mount

List the files:

```bash
ls /mymount
```

Output:

```
file.txt
```

Check the mounted filesystem:

```bash
mount | grep mymount
```

Example Output:

```
/dev/sdd on /mymount type ext4 (rw,relatime,discard,errors=remount-ro,data=ordered)
```

Check disk information:

```bash
df -Ph
```

### 8. Test the bind mount

Append new data:

```bash
echo "Second Line" | sudo tee -a /data/file.txt
```

Read the file from the mount point:

```bash
cat /mymount/file.txt
```

Output:

```
Hello
Second Line
```

## Conclusion

* Successfully created a manual bind mount using the Linux `mount --bind` command.
* Verified that `/mymount` points to the same data as `/data`.
* Confirmed that changes made in the source directory are immediately visible from the mounted directory.
* Performed the practical successfully inside the Minikube Linux environment.
