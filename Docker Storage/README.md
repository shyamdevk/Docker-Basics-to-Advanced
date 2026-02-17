# 🐳 Docker Storage – Complete Notes (4 Types)
![Docker](https://github.com/shyamdevk/Docker-Basics-to-Advanced/blob/image/volume.gif)

Docker provides multiple storage mechanisms to handle data inside containers.
Some data is **temporary (ephemeral)**, and some is **persistent** even after container deletion.

This guide explains all **4 types of Docker storage** in a simple and clear way.

---

## 📌 **1. Ephemeral Storage (Container Filesystem)**

Also called: *Storage Driver Layer*

✔ Default storage available **inside every container**
✔ Managed by Docker storage drivers: `overlay2`, `AUFS`, `btrfs`, etc.
❌ **Temporary / Ephemeral** → **data is lost** when container stops or is removed

### 🔹 **Use Case**

* Temporary files
* Cache
* Runtime working directory

### 🔹 **Simple Meaning**

➡️ *Temporary storage inside the container. Deleted when container is removed.*

### 🔹 Example

```bash
docker run ubuntu touch /test.txt
# After container stops → file disappears
```

---

## 📌 **2. Docker Volumes (Persistent Storage)**

✔ Most recommended method for saving data
✔ Managed completely by Docker
✔ Stored in Docker's directory on the host (/var/lib/docker/volumes/ on Linux)
✔ Stored on host machine under `/var/lib/docker/volumes/`
✔ **Persists even if the container is deleted**

### 🔹 **Use Case**

* Databases
* Logs
* Application data

### 🔹 **Simple Meaning**

➡️ *Permanent storage created by Docker. Safe even if container is deleted.*

### 🔹 Commands

Create a volume:

```bash
docker volume create mydata
```

Use volume in a container:

```bash
docker run -v mydata:/var/lib/mysql mysql
```

List volumes:

```bash
docker volume ls
```

# 🐳 Docker Volumes: Named vs Anonymous

This document explains the difference between **Named Volumes** and **Anonymous Volumes** in Docker in a simple and clear way.

---

## ✅ Named Volume

A **named volume** is a volume that **you assign a name to**.
It is easy to manage, reuse, and identify.

### **Syntax**

```bash
docker run -v myvolume:/path/in/container image
```

### **Best Use**

* Persistent data
* Databases
* When you want to reuse the volume later

---

## ✅ Anonymous Volume

An **anonymous volume** is created **automatically by Docker** when no name is given.
The name is a long random string.

### **Syntax**

```bash
docker run -v /path/in/container image
```

### **Best Use**

* Temporary containers
* When you don’t need to track the volume

---

## 📊 Comparison Table

| Type                 | Meaning                             | Syntax           | Best Use                   |
| -------------------- | ----------------------------------- | ---------------- | -------------------------- |
| **Named Volume**     | Volume with a name you choose       | `-v myvol:/data` | Persistent data, databases |
| **Anonymous Volume** | Docker creates a random-name volume | `-v /data`       | Temporary containers       |

---

---

## 📌 **3. Bind Mounts**

✔ Directly maps a **host system folder or file** into a container
✔ You define the **exact path**, Docker doesn’t manage it
✔ Great for developers → auto-reload source code

### 🔹 **Use Case**

* Local development
* Sharing configuration files
* Real-time code changes

### 🔹 **Simple Meaning**

➡️ *Links a folder from your laptop/server directly into the container.*

### 🔹 Example

```bash
docker run -v /home/user/app:/app python:3.10
```

---

## 📌 **4. tmpfs Mounts (In-memory)**

✔ Stored in **RAM**, not on disk
✔ Very fast
✔ Automatically deleted when container stops
✔ Good for sensitive data → never written to disk

### 🔹 **Use Case**

* Sensitive keys
* High-speed temporary operations

### 🔹 **Simple Meaning**

➡️ *Storage in RAM. Fast, temporary, and not saved to disk.*

### 🔹 Example

```bash
docker run --tmpfs /secure_data:rw nginx
```

---

# 📊 Quick Comparison Table

| Storage Type                 | Persistent? | Location                 | Best Use Case                |
| ---------------------------- | ----------- | ------------------------ | ---------------------------- |
| **Ephemeral (Container FS)** | ❌ No        | Inside container         | Temporary data, cache        |
| **Volume**                   | ✔ Yes       | Host (Docker-managed)    | Databases, production data   |
| **Bind Mount**               | ✔ Yes       | Host (user-defined path) | Development, config files    |
| **tmpfs**                    | ❌ No        | RAM                      | Sensitive or high-speed data |

---

# 🧠 Summary (Easy to Remember)

* **Ephemeral** → Temporary → *lost when container dies*
* **Volume** → Persistent → *Docker-managed*
* **Bind Mount** → Persistent → *Host folder directly mapped*
* **tmpfs** → RAM → *fast + secure but temporary*

---
Here is your **README.md–ready version**, clean and properly formatted:

---

# 📦 Docker Volume Commands Table

| **Command**                       | **Description**                                                   |
| --------------------------------- | ----------------------------------------------------------------- |
| `docker volume create my-volume`  | Creates a new volume named **my-volume**.                         |
| `docker volume ls`                | Lists all existing volumes on your system.                        |
| `docker volume inspect my-volume` | Shows detailed information about a specific volume.               |
| `docker volume rm my-volume`      | Deletes a specific volume. *Fails if the volume is in use.*       |
| `docker volume prune`             | Deletes all **unused** volumes not associated with any container. |

---

# 📦 Docker Storage Types – Full Guide with Examples

This README explains the **4 types of Docker storage** with **beginner-friendly, step-by-step examples**:
✔️ Ephemeral Storage
✔️ Volumes
✔️ Bind Mounts
✔️ tmpfs (in-memory)

---

# 🗂️ **1. Ephemeral Storage (Container Filesystem)**

Temporary storage **inside** the container.
⚠️ Data is deleted when the container is deleted.

---

## 🔹 **Create container**

```bash
docker run -it --name test-ephemeral ubuntu bash
```

## 🔹 **Create a file inside the container**

```bash
echo "hello" > /myfile.txt
cat /myfile.txt
```

## 🔹 **Exit**

```bash
exit
```

## 🔹 **Start the container again**

```bash
docker start -i test-ephemeral
cat /myfile.txt
```

✔️ File still exists (container still alive)

## 🔹 **Remove container**

```bash
docker rm -f test-ephemeral
```

### ✔ Result

Ephemeral storage is **gone forever** when the container is removed.

---

# 🗄️ **2. Docker Volume (Persistent Storage)**

Volumes store data **outside the container**, on the host.
✔ Data survives container restarts & deletion
✔ Best for databases, logs, app data

---

## 🔹 **Create a volume**

```bash
docker volume create myvolume
```

## 🔹 **Inspect the volume**

```bash
docker volume inspect myvolume
```

## 🔹 **Run container with volume**

```bash
docker run -it --name test-volume -v myvolume:/data ubuntu bash
```

## 🔹 **Create file in volume**

```bash
echo "volume data" > /data/file.txt
cat /data/file.txt
```

## 🔹 **Remove the container**

```bash
exit
docker rm -f test-volume
```

## 🔹 **Run a new container using the same volume**

```bash
docker run -it -v myvolume:/data ubuntu bash
```

## 🔹 **Verify data**

```bash
cat /data/file.txt
```

### ✔ Result

Volume data **persists**, even after deleting the first container.

---

# 🗂️ **3. Bind Mount (Host Directory → Container)**

Bind Mount connects a **real host folder** into a container.
✔ Real-time sync between host & container
✔ Perfect for development

---

## 🔹 **Create folder on host**

```bash
mkdir /home/user/myapp
echo "hello from host" > /home/user/myapp/hostfile.txt
```

## 🔹 **Run container with bind mount**

```bash
docker run -it --name test-bind \
  -v /home/user/myapp:/app ubuntu bash
```

## 🔹 **Check files inside container**

```bash
ls /app
cat /app/hostfile.txt
```

## 🔹 **Create a file from inside the container**

```bash
echo "created inside container" > /app/containerfile.txt
```

## 🔹 **Check file on host**

```bash
cat /home/user/myapp/containerfile.txt
```

### ✔ Result

Host ↔ Container file sync works perfectly.

## 🔹 **Inspect the container**

```bash
docker inspect test-bind
```

---

# ⚡ **4. tmpfs Storage (RAM Storage)**

tmpfs stores data **only in RAM**, not on disk.
✔ Super fast
✔ Good for sensitive data
⚠ Data disappears when container restarts

---

## 🔹 **Run container with tmpfs**

```bash
docker run -it --name test-tmpfs \
  --tmpfs /ramdisk ubuntu bash
```

## 🔹 **Create file in RAM**

```bash
echo "in-memory data" > /ramdisk/temp.txt
cat /ramdisk/temp.txt
```

## 🔹 **Restart the container**

```bash
exit
docker start -i test-tmpfs
```

## 🔹 **Check RAM directory**

```bash
ls /ramdisk
```

### ❌ Result

File disappears → stored only in RAM.

## 🔹 **Inspect tmpfs mount**

```bash
docker inspect test-tmpfs
```

---

# 📘 **Summary Table**

| Storage Type   | Persistent? | Location                   | Use Case                   |
| -------------- | ----------- | -------------------------- | -------------------------- |
| **Ephemeral**  | ❌ No        | Inside container           | Temporary files            |
| **Volume**     | ✔ Yes       | Host (Docker-managed)      | Databases, logs, user data |
| **Bind Mount** | ✔ Yes       | Host (user-defined folder) | Development, local files   |
| **tmpfs**      | ❌ No        | RAM                        | Sensitive or fast data     |

---

