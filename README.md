# 🐳 Docker & Containerization — Complete Guide

---

## 🖥️ 1. Virtual Machine (VM)

A **Virtual Machine (VM)** is a software-based computer that runs inside a physical computer.  
It uses a **hypervisor** (like VirtualBox, VMware, or Hyper-V) to share hardware resources such as CPU, RAM, and Disk among multiple VMs.  
Each VM has its own **operating system** and runs applications independently.

### 🔻 Disadvantages of Virtual Machines

- ⚙️ **Heavy resource usage** – Each VM needs a full OS → consumes more CPU, memory, and storage.  
- 🐢 **Slower performance** – Because of the extra OS layer and hypervisor.  
- ⏱️ **Long startup time** – Takes longer to boot compared to containers.  
- 💾 **Less efficient** – Difficult to scale quickly due to size and resource needs.

---

## 📦 2. Container

A **Container** is a lightweight, portable package that contains everything needed to run an application — code, libraries, and dependencies.  
It shares the same OS kernel as the host, so it doesn’t need a full OS like a VM.

### ✅ Advantages of Containers over Virtual Machines

- ⚡ **Lightweight** – Uses less memory and storage.  
- 🚀 **Fast startup** – Containers start in seconds.  
- 🔁 **Portable** – Runs the same on any system (no OS dependency).  
- 📈 **Easy to scale** – Quickly start or stop multiple containers.

---

## 🐳 3. Docker

**Docker** is a popular containerization platform that helps developers build, package, and run applications in containers easily.

### 🔹 In Simple Terms

> **Docker = Tool used to create and manage containers.**

### 🔹 What Docker Provides

- 🧩 **Docker Images** – Blueprints for containers.  
- 📦 **Docker Containers** – Running instances of images.  
- 🔄 **Docker Hub** – Online repository to share and download images.

---

## 🧠 In Short

| Feature | Virtual Machine | Container | Docker |
|----------|----------------|------------|---------|
| **OS** | Each VM has its own OS | Shares host OS | Platform to manage containers |
| **Speed** | Slow to start | Fast to start | Helps create/manage containers |
| **Resource Use** | Heavy | Lightweight | Lightweight (via containers) |
| **Example** | VMware, VirtualBox | Kubernetes, Podman | Docker Engine |

---

## 🐳 Docker Initialization  
📸 *Mark here to upload image later*

---

## 🐳 Docker Architecture  
![Docker Architecture](https://cdn.prod.website-files.com/681e366f54a6e3ce87159ca4/687d7a52cccb7374efbbf8ca_image2-49.png)

### 1️⃣ Client
The user interacts with Docker using commands like:
```bash
docker build     # builds a Docker image  
docker pull      # downloads an image from a registry  
docker run       # runs a container using an image
````

These commands are sent to the **Docker Daemon**.

### 2️⃣ Docker Host

The system where Docker actually runs. It contains:

* **Docker Daemon** → builds, runs, and manages containers.
* **Images** → read-only templates used to create containers.
* **Containers** → running instances of Docker images.

### 3️⃣ Registry

A storage place for Docker images (like Docker Hub).

* `docker pull` → downloads images.
* `docker push` → uploads images.

---

## 🏷️ Popular Container Registries

| Registry                                  | Description                                        |
| ----------------------------------------- | -------------------------------------------------- |
| **GHCR (GitHub Container Registry)**      | Store and manage Docker images directly in GitHub. |
| **GitLab Container Registry**             | Integrated with GitLab CI/CD pipelines.            |
| **Quay.io**                               | Red Hat’s secure and reliable registry.            |
| **GCR (Google Container Registry)**       | Google Cloud’s image registry.                     |
| **ECR (Elastic Container Registry)**      | AWS service for image storage.                     |
| **ACR (Azure Container Registry)**        | Microsoft Azure’s registry for private images.     |
| **IBM CR (IBM Cloud Container Registry)** | IBM Cloud’s image storage service.                 |

---

## 🐳 Dockerfile

A **Dockerfile** is a text file containing instructions to build a Docker image.
It defines what goes inside the image — base OS, software, files, and commands.

When you run:

```bash
docker build .
```

Docker reads the Dockerfile and creates an image step-by-step.

---

## 🐳 Docker Image

A **Docker Image** is a read-only template used to create Docker containers.
It includes:

* Operating System
* Application code
* Libraries and dependencies
* Configuration settings

### ⚙️ Key Points

* Images are **immutable**.
* You can pull from registries like Docker Hub:

  ```bash
  docker pull ubuntu
  ```
* Run an image to create a container:

  ```bash
  docker run image_name
  ```
* Images are built in **layers**, improving reusability and speed.

---

## ⚙️ Docker Engine vs Docker Daemon

| Component                   | Description                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------- |
| **Docker Engine**           | The full Docker system that includes the Daemon, CLI, and REST API.                |
| **Docker Daemon (dockerd)** | The background service responsible for building, running, and managing containers. |

🧠 **In short:**

> Docker Engine = Full system
> Docker Daemon = The core process inside it

---

## 🐳 Common Docker Commands

| Command                               | Description                             |
| ------------------------------------- | --------------------------------------- |
| `docker pull <image>`                 | Download an image from registry         |
| `docker images`                       | List all images                         |
| `docker rmi <image>`                  | Remove an image                         |
| `docker run <image>`                  | Create & run a container (foreground)   |
| `docker create <image>`               | Create container without running        |
| `docker run -d <image>`               | Run container in background             |
| `docker run -d --name <name> <image>` | Run with custom name                    |
| `docker ps`                           | List running containers                 |
| `docker ps -a`                        | List all containers (including stopped) |
| `docker stop <container>`             | Stop a running container                |
| `docker start <container>`            | Start a stopped container               |
| `docker rm <container>`               | Remove container                        |
| `docker exec -it <container> bash`    | Access container shell                  |

---

## 🐳 Docker Container Lifecycle

| State       | Description                                        |
| ----------- | -------------------------------------------------- |
| **Created** | Made using `docker create`, but not started yet.   |
| **Running** | Actively executing its process (`docker run`).     |
| **Paused**  | Temporarily stopped processes (`docker pause`).    |
| **Stopped** | Container stopped but data exists (`docker stop`). |
| **Exited**  | Finished running or manually stopped.              |
| **Dead**    | Failed/unresponsive container needing cleanup.     |

🧠 **In short:**

> Created → Running → (Paused / Stopped / Exited) → Dead

---

## 🐳 Docker Build

To build images from a Dockerfile:

```bash
docker build -t myimage .
```

Builds from default Dockerfile.

```bash
docker build -t myimage -f Dockerfile.dev .
```

Builds using a custom Dockerfile.

```bash
docker build -t myimage -f ./docker/dev.Dockerfile ./app
```

Builds from a subfolder and uses a custom build context.

---

## 🧩 Important Dockerfile Instructions

| Instruction    | Description                                 | Example                                            |
| -------------- | ------------------------------------------- | -------------------------------------------------- |
| **FROM**       | Defines the base image                      | `FROM ubuntu:latest`                               |
| **WORKDIR**    | Sets working directory                      | `WORKDIR /app`                                     |
| **COPY**       | Copies files into container                 | `COPY . /app`                                      |
| **ADD**        | Similar to COPY but supports URLs/tar files | `ADD myapp.tar /app`                               |
| **LABEL**      | Adds metadata                               | `LABEL maintainer="shyamdevk677@gmail.com"`        |
| **ENV**        | Sets environment variables                  | `ENV PORT=8080`                                    |
| **RUN**        | Executes commands during build              | `RUN apt-get update && apt-get install -y python3` |
| **CMD**        | Default command when container starts       | `CMD ["python3", "app.py"]`                        |
| **ENTRYPOINT** | Main executable for container               | `ENTRYPOINT ["python3"]`                           |
| **EXPOSE**     | Documents port                              | `EXPOSE 5000`                                      |
| **USER**       | Sets user to run container                  | `USER appuser`                                     |
| **VOLUME**     | Creates mount point for persistent data     | `VOLUME ["/data"]`                                 |

---

## 🧩 Additional Dockerfile Instructions

### 13. 🧠 HEALTHCHECK

Used to check if the container is running properly.

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:5000 || exit 1
```

With options:

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --retries=3 CMD curl --fail http://localhost:8080 || exit 1
```

Disable health check:

```dockerfile
HEALTHCHECK NONE
```

---

### 14. ⚙️ ARG

Defines build-time variables.

```dockerfile
ARG VERSION=3.10
FROM python:${VERSION}
```

Override during build:

```bash
docker build -t myimage --build-arg VERSION=3.12 .
```

Combine with ENV:

```dockerfile
ARG APP_PORT=5000
ENV PORT=$APP_PORT
```

---

### 15. 🧩 ONBUILD

Sets triggers for future builds when the image is used as a base.

```dockerfile
FROM python:3.10
ONBUILD COPY . /app
ONBUILD RUN pip install -r /app/requirements.txt
```

Useful for creating base images that automate setup for child images.

---

## 🏁 Conclusion

Docker revolutionizes application deployment by making it **lightweight**, **portable**, and **scalable**.
Understanding **VMs vs Containers**, **Docker architecture**, and **Dockerfile instructions** is essential for DevOps and modern cloud development.

---

> ✍️ **Author:** Shyamdev K
> 📧 *[shyamdevk677@gmail.com](mailto:shyamdevk677@gmail.com)*
> 🐙 *For educational and DevOps learning purposes.*

```

---

Would you like me to:
- 🪄 add **GitHub badges** (like Docker, Linux, DevOps, etc.), or  
- 📷 insert **image placeholders** (with Markdown syntax) for where you’ll upload images?
```
