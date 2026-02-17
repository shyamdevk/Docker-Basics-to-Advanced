# 🌐 Docker Networking — Explained


![Docker](https://github.com/shyamdevk/Docker-Basics-to-Advanced/blob/image/network.gif)

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

  Here is the **README-friendly version**, clean and formatted:

---

## 📎 Reference

For a basic Docker project example, you can refer to the following repository:

🔗 **Docker Word Counter App**
GitHub: *[https://github.com/shyamdevk/Docker-Word-Counter-App](https://github.com/shyamdevk/Docker-Word-Counter-App)*

---

---
