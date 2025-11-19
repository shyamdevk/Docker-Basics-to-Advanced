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
![Docker Initialization](unt.png)

# 🐳 Docker Initialization (Non-Root User Setup)

When you install Docker on Linux, it usually requires **sudo** to run Docker commands.  
This setup lets your user run Docker **without using `sudo`** every time.

---

## 🧭 Steps to Enable Docker for a Non-Root User

### 1️⃣ Switch to the Root User
```bash
sudo su
````

Used to switch to the **root account** to perform administrative actions.

---

### 2️⃣ Add Your User to the Docker Group

```bash
usermod -aG docker <username>
```

**Example:**

```bash
usermod -aG docker shyamdevk
```

✅ This adds the user **`shyamdevk`** to the **`docker` group**, giving permission to access the Docker daemon.

---

### 3️⃣ Exit Root Mode

```bash
exit
```

Leaves root mode and returns to your normal user session.

---

### 4️⃣ Test Docker Access

```bash
docker ps
```

❌ If you see:

```
permission denied while trying to connect to the Docker daemon socket
```

it means your group permissions haven’t refreshed yet.

---

### 5️⃣ Refresh Group Membership

```bash
newgrp docker
```

This command refreshes your session with the new group permissions — no need to log out.

---

### 6️⃣ Verify Docker Access

```bash
docker ps
```

✅ Now this should work — listing all running containers without needing `sudo`.

---

## 🧠 Summary

| Command                         | Purpose                   |
| ------------------------------- | ------------------------- |
| `sudo su`                       | Switch to root user       |
| `usermod -aG docker <username>` | Add user to docker group  |
| `exit`                          | Return to normal user     |
| `docker ps`                     | Test Docker access        |
| `newgrp docker`                 | Refresh group permissions |


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

# 🐳 Docker — ARG, ENV, and Port Mapping

This document explains **Docker ARG and ENV instructions**, how to **override environment variables**, and how **port mapping** works.  
It also includes a **lab example** on containerizing a simple Flask app.

---

## 🧱 1. Understanding ARG

- `ARG` defines **build-time variables** — available **only while building** the Docker image.  
- You can pass values using the `--build-arg` flag when running `docker build`.

### 🧩 Example: Using ARG to Change the Base Image Version

**Dockerfile**
```dockerfile
# Define an ARG variable with a default value
ARG PYTHON_VERSION=3.10

# Use the ARG variable in the FROM instruction
FROM python:${PYTHON_VERSION}

# Set working directory
WORKDIR /app

# Copy files into container
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Default command
CMD ["python", "app.py"]
````

### 🧩 Build the Image Using a Different Python Version

```bash
# Build with default version (3.10)
docker build -t myapp .

# Build with a different version (3.12)
docker build -t myapp:3.12 --build-arg PYTHON_VERSION=3.12 .
```

✅ **Result:**

* First build → uses `python:3.10` as the base image.
* Second build → uses `python:3.12` as the base image.

> **Note:** After the build, ARG values are **not available** inside the running container — they’re only used during image creation.

---

## 🧱 2. Understanding ENV

* `ENV` sets **environment variables** that are available **inside the container at runtime**.
* You can use them in your app or shell commands inside the container.

### 🧩 Example: Using ENV Inside a Dockerfile

**Dockerfile**

```dockerfile
FROM ubuntu:latest

# Set environment variables
ENV APP_ENV=production
ENV PORT=8080

# Use environment variables inside RUN
RUN echo "Environment: $APP_ENV, Port: $PORT" > info.txt

# Set working directory
WORKDIR /app

# Copy app files
COPY . .

# Expose the port from ENV variable
EXPOSE $PORT

# Default command
CMD ["bash"]
```

### 🧩 Run Container and View ENV Variables

```bash
docker build -t env-example .
docker run -it env-example bash
```

**Inside the container:**

```bash
echo $APP_ENV
echo $PORT
```

**Output:**

```
production
8080
```

---

## 🧱 3. Overriding ENV at Runtime Using `-e`

* You can override environment variables **when starting the container** using the `-e` flag.

**Example**

```bash
docker run -it -e APP_ENV=development -e PORT=9090 env-example bash
```

**Inside the container:**

```bash
echo $APP_ENV   # Output: development
echo $PORT      # Output: 9090
```

✅ This overrides values from the Dockerfile **temporarily for this container only**.

---

## 🧱 Port Mapping in Docker

* **Port mapping** connects a **container’s internal port** to a **port on the host machine**.
* It allows access to applications running inside containers from your local system or browser.
* Containers are isolated — their internal ports aren’t visible outside unless mapped.

### 🧩 How Port Mapping Works

When mapping ports, Docker forwards traffic from your **host machine’s port** → to the **container’s port**.
The format is:

```bash
-p <host_port>:<container_port>
```

**Example**

```bash
docker run -p 8080:80 nginx
```

**Explanation**

* `8080` → port on your host machine
* `80` → port inside the container (used by Nginx)

When you open [http://localhost:8080](http://localhost:8080), Docker forwards the request to port `80` inside the container.

---

### 🧩 Syntax

```bash
docker run -p <host_port>:<container_port> <image_name>
```

---

### 🧩 EXPOSE vs Port Mapping

| Concept               | Purpose                                                          |
| --------------------- | ---------------------------------------------------------------- |
| **EXPOSE**            | Documents which port the container listens on (inside the image) |
| **-p (Port Mapping)** | Actually publishes the container’s port to the host              |

**Example**

```dockerfile
EXPOSE 5000
```

To access externally:

```bash
docker run -p 5000:5000 myapp
```

---

## 📘 Docker Lab — Containerizing a Flask App

In this lab, we containerize a simple **Flask application** using Docker.

### 🎯 Objective

* Write a `Dockerfile`
* Install dependencies from `requirements.txt`
* Copy application files into the container
* Expose the application port
* Run the container using port mapping
* Access the Flask app from the browser

The lab demonstrates how to build and run a Python-based application using
`FROM`, `ARG`, `ENV`, `WORKDIR`, `COPY`, `RUN`, `EXPOSE`, and `CMD`.

---

## 🧪 Lab Steps

### 1️⃣ Create Project Directory

```bash
mkdir dock
```

Inside this folder, place:

```
app.py
requirements.txt
Dockerfile
```

---

### 2️⃣ Write the Dockerfile

```dockerfile
FROM python
ARG APP_PORT=5000
ENV PORT=$APP_PORT
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
EXPOSE $PORT
CMD ["python", "app.py"]
```

---

### 3️⃣ Build the Docker Image

```bash
docker build -t myflaskapp .
```

This creates a Docker image named **`myflaskapp`**.

---

### 4️⃣ Run the Docker Container

```bash
docker run -p 5000:5000 myflaskapp
```

This maps your host’s port `5000` to the container’s port `5000`.

---

### 5️⃣ Access the Flask Application

Open your browser and visit:
👉 [http://localhost:5000](http://localhost:5000)

You should see the Flask app’s response.

> ⚠️ **Note:**
> If another container is already running on the same port, stop it before starting this one.

---

✅ **In summary:**

* `ARG` → Build-time variable
* `ENV` → Runtime variable
* `-e` → Override environment variables
* `-p` → Map container port to host port
* `EXPOSE` → Document internal port

---
# 🏗️ Monolithic vs Microservices Architecture

This guide explains the **difference between Monolithic and Microservices architectures** — two major approaches in modern software development and DevOps.

---

## 🧱 **Monolithic Architecture**

A **Monolithic Architecture** means the **entire application is built as one single unit** — all components are tightly coupled and run together.

### 🧩 **Key Points**
- 🧠 All features (**UI**, **API**, **business logic**, **database access**) are **tightly integrated**.  
- ⚙️ Everything runs as **one large application**.  
- 🔁 Updating one part requires **redeploying the entire app**.  
- 🗃️ Uses **one codebase** and usually **one database**.  
- 🚀 Easy to start but becomes **hard to manage and scale** as it grows.

### ✅ **Advantages**
- 🧩 Simple to build and deploy.  
- 👶 Easier for beginners.  
- ⚡ Suitable for **small applications**.  

### ❌ **Disadvantages**
- 📉 Hard to scale specific features independently.  
- 🐞 One small bug can affect the **entire application**.  
- 🐢 Slower development as codebase grows larger.  

---

## ☁️ **Microservices Architecture**

A **Microservices Architecture** divides an application into **small, independent services**, each responsible for a specific function.

### 🧩 **Key Points**
- 🔹 The app is split into **independent services** (e.g., login, payment, product).  
- 🚀 Each service can be **developed, deployed, and scaled separately**.  
- 🔗 Services communicate via **APIs** (REST, gRPC, or messaging queues).  
- 🧰 Each service can use **different programming languages** or **databases**.  

### ✅ **Advantages**
- 📈 Scale only the required services.  
- ⚡ Faster development — teams work independently.  
- 🧩 A failure in one service **does not crash the entire system**.  
- 🔄 Easier to update, modify, or replace services.  

### ❌ **Disadvantages**
- ⚙️ More complex to design and manage.  
- 🧱 Requires **DevOps tools** like Docker, Kubernetes, and CI/CD pipelines.  
- 🌐 Network communication between services may introduce **latency**.  

---

## 🔍 **Comparison Overview**

| Feature | Monolithic Architecture | Microservices Architecture |
|----------|-------------------------|-----------------------------|
| **Structure** | Single unified application | Multiple independent services |
| **Deployment** | Whole app redeployed together | Each service deployed independently |
| **Scalability** | Scales as a whole | Scales per service |
| **Technology Stack** | Usually one language/database | Can mix different languages & databases |
| **Maintenance** | Harder as size grows | Easier to maintain & update individual services |
| **Failure Impact** | One crash can bring down entire app | Failure isolated to specific service |
| **Best For** | Small/simple projects | Large, complex, scalable systems |

---

## 🧠 **In Short**
> **Monolithic:** One big block — simple but rigid.  
> **Microservices:** Many small blocks — complex but flexible.

---

📘 **Example Use Cases**
- 🏫 **Monolithic:** School management app, small blog site, portfolio app  
- 🏢 **Microservices:** E-commerce platform, banking system, Netflix, Uber  

---
# 🌐 Docker Networking — Explained

Docker Networking allows containers to **communicate** with each other and with the **outside world**.  
It provides **isolated environments** where containers can connect, share data, and send/receive traffic safely.

Think of Docker networking as giving each container its own **virtual network connection** — just like computers in a LAN.

---

## 🧩 1. Bridge Network

### 🔹 Description
The **Bridge Network** is the **default** network type in Docker.  
Containers connected to the same bridge can **communicate with each other** using their **container names**.

### 🧠 Simple Meaning
> Containers on the same bridge network can talk to each other internally.

### 🧰 Use Case
- Running multiple containers on a **single host** (one machine).  
- Example: A **web app container** communicating with a **database container**.  

---

## ⚙️ 2. Host Network

### 🔹 Description
In a **Host Network**, the container uses the **host machine’s network directly**.

### 🧠 Simple Meaning
> The container **shares the host’s IP address** — no isolation.

### 🧰 Use Case
- When you need **high network performance**.  
- Applications that must listen on **system ports** without additional NAT translation.

---

## 🚫 3. None Network

### 🔹 Description
A **None Network** gives the container **no network access** at all.

### 🧠 Simple Meaning
> The container is **completely isolated** — no internet or inter-container communication.

### 🧰 Use Case
- For **high-security tasks**.  
- When networking must be **manually configured**.

---

## ☁️ 4. Overlay Network

### 🔹 Description
An **Overlay Network** connects containers running on **different Docker hosts (machines)**.

### 🧠 Simple Meaning
> Containers on multiple servers can communicate as if they’re on the **same local network**.

### 🧰 Use Case
- 🐝 **Docker Swarm** deployments.  
- ⚙️ **Multi-node** or **distributed applications**.  

---

## 🧬 5. Macvlan Network

### 🔹 Description
**Macvlan** gives each container its **own MAC address and IP address** on the physical network.

### 🧠 Simple Meaning
> The container behaves like a **real device** directly connected to your LAN.

### 🧰 Use Case
- When containers need to be **directly accessible** on your local network (LAN).  
- Useful for **legacy systems** requiring unique MAC/IP addresses.

---

## 🧱 Docker Bridge Network — In Depth

The **Bridge Network** is the most commonly used network type.  
Here’s how it works internally 👇

---

### 🖼️ *Docker Bridge Network Diagram*
*(You can upload your image later and link it here)*  

![Screenshot](https://github.com/shyamdevk/Docker-Basics-to-Advanced/blob/image/dock.png)

### ⚙️ What the Diagram Shows

* There is **one Docker Host** (your computer).
* Two containers are running:

  * 🧩 `Container Test1`
  * 🧩 `Container Test2`
* Each container has its own **virtual network interface** (`eth0` inside the container).

---

### 🔸 1. veth Pair (Virtual Ethernet)

* Each container connects to the Docker network using a **veth (virtual ethernet) pair**.
* One end is inside the container (`eth0`).
* The other end (`veth1`, `veth2`, etc.) connects to the **`docker0` bridge** on the host.

💡 Think of it like a **virtual cable** between the container and Docker’s virtual switch.

---

### 🔸 2. docker0 Bridge

* `docker0` is a **virtual switch** created automatically by Docker.
* It connects all container `veth` interfaces on the same bridge network.
* Containers can communicate using **private IP addresses**.

✅ Example:
`Container Test1 (172.17.0.2)` ↔ `Container Test2 (172.17.0.3)` via `docker0`.

---

### 🔸 3. Host Network Interface (eth0)

* The host machine has its own **network interface** (`eth0`) that connects to the **external network** (LAN/Internet).
* Docker uses **NAT (Network Address Translation)** through `docker0` to route traffic from containers to the outside world.

💡 This enables containers to **access the internet**, even though they use **private IPs**.

---

## 📡 Reserved IPs in Bridge Network

In a bridge network with CIDR `172.17.0.0/16`, the following IPs are reserved:

| IP Address             | Description                        |
| ---------------------- | ---------------------------------- |
| `172.17.0.0`           | Network address (reserved)         |
| `172.17.0.1`           | Gateway (used by `docker0` bridge) |
| `172.17.0.2` → onwards | Assigned to containers             |
| `172.17.255.255`       | Broadcast address (reserved)       |

---

## 🧠 Summary

| Network Type | Description                                                 | Use Case                          |
| ------------ | ----------------------------------------------------------- | --------------------------------- |
| **Bridge**   | Default Docker network; containers on same host communicate | Web app ↔ Database                |
| **Host**     | Shares host’s network; no isolation                         | High performance apps             |
| **None**     | No network access                                           | Security isolation                |
| **Overlay**  | Connects containers across multiple hosts                   | Docker Swarm, multi-node systems  |
| **Macvlan**  | Gives each container its own MAC/IP                         | Direct LAN access, legacy systems |

---

✅ **In Short:**

> Docker networking creates virtual connections between containers, hosts, and the internet — enabling **flexible, isolated, and scalable communication** for containerized applications.

---
# 📚 **10. Important Docker Network Commands**

Below are essential commands used in Docker networking:

---

### **🔸 List All Docker Networks**

```bash
docker network ls
```

### **🔸 Inspect a Specific Network**

```bash
docker network inspect <network-name>
```

### **🔸 Create a User-Defined Bridge Network**

```bash
docker network create my-bridge
```

> Note: If `--driver` is not provided, Docker defaults to **bridge**.

### **🔸 Run a Container on a Specific Network**

```bash
docker run -dit --name container1 --network my-bridge ubuntu
```

### **🔸 Remove a User-Defined Network**

```bash
docker network rm my-bridge
```

### **🔸 Connect an Existing Container to a Network**

```bash
docker network connect my-bridge container2
```

### **🔸 Disconnect a Container from a Network**

```bash
docker network disconnect my-bridge container2
```

# 🚀 Docker LAN Lab: Custom Bridge Network & Container Networking

This lab demonstrates how to:

* Create a **custom Docker bridge network**
* Run an Nginx container
* **Detach** the Nginx container from its default network
* **Attach** the same Nginx container to the new custom network
* Create a new container directly connected to the custom network
* Verify and test communication between containers

Perfect for DevOps, Docker networking, and practical exam notes.

---

## 📘 **1. Overview**

Docker provides networking so that containers can communicate with:

* Each other
* The host
* The outside world

The **bridge network** is the default network that Docker creates (`docker0`).
In this lab, we will **manually create a custom bridge**, attach containers to it, and test communication.

---

## 🛠 **2. Create a Custom Bridge Network**

Create a new Docker network of type **bridge**:

```bash
docker network create my-bridge
```

### ✔ What this does:

* Creates a virtual LAN inside Docker
* All containers added to this network can communicate with each other
* Provides its own IP range, gateway, and subnet

---

## 🐳 **3. Run an Nginx Container (Initially on Default Bridge)**

Run Nginx normally (it attaches to the default `bridge`):

```bash
docker run -d --name mynginx nginx
```

### ✔ What this does:

* Starts an Nginx server in detached mode
* Connects it to the default Docker bridge network automatically

---

## 🔍 **4. Check Existing Networks for the Container**

View details of the default bridge network:

```bash
docker network inspect bridge
```

### ✔ Why?

To confirm that **mynginx** is connected to the default network before disconnecting it.

---

## 🔌 **5. Disconnect Nginx from the Default Bridge Network**

```bash
docker network disconnect bridge mynginx
```

### ✔ What this does:

* Removes Nginx from the default Docker network
* Now Nginx is not part of any network
* It cannot communicate with other containers until attached to another network

---

## 🔗 **6. Connect Nginx to the New Custom Bridge Network**

```bash
docker network connect my-bridge mynginx
```

### ✔ Why?

This attaches the existing Nginx container to our new network **without restarting the container**.

---

## 🆕 **7. Create a New Container on the Custom Bridge Network**

Create a new Alpine container and attach it to **my-bridge** at creation time:

```bash
docker run -d --name alpine1 --network my-bridge alpine sleep infinity
```

### ✔ Explanation:

* Uses Alpine Linux (lightweight)
* Stays running using `sleep infinity`
* Automatically connected to **my-bridge**

---

## 🔍 **8. Verify Both Containers Are Connected to the New Bridge**

```bash
docker network inspect my-bridge
```

You should see:

* `mynginx`
* `alpine1`

Inside the network’s **Containers** section.

This confirms both are on the same LAN-like virtual network.

---

## 📡 **9. Test Connectivity Between Containers**

From `alpine1` container, ping the `mynginx` container:

```bash
docker exec -it alpine1 ping mynginx
```

### ✔ Expected Output:

You will see ping replies, meaning:

* Containers can resolve each other by **name**
* Both are on the same DHCP/subnet
* Communication is successful

---

## 🧠 **10. Summary**

| Step                 | Description                                                              |
| -------------------- | ------------------------------------------------------------------------ |
| Create network       | `docker network create my-bridge`                                        |
| Run nginx            | `docker run -d --name mynginx nginx`                                     |
| Disconnect           | `docker network disconnect bridge mynginx`                               |
| Connect              | `docker network connect my-bridge mynginx`                               |
| Create new container | `docker run -d --name alpine1 --network my-bridge alpine sleep infinity` |
| Inspect              | `docker network inspect my-bridge`                                       |
| Test                 | `docker exec -it alpine1 ping mynginx`                                   |

---

## 🎯 **Final Result**

* You have created a **custom isolated LAN** inside Docker
* Successfully moved an existing container into it
* Created a new container directly inside the network
* Verified network communication works perfectly

---
# 🐳 Docker Storage – Complete Notes (4 Types)

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


