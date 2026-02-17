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
