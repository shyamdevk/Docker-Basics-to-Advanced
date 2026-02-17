# 🐳 Docker Image Push Test

This guide shows the **fastest and easiest** way to create a small Docker image,  
push it to **Docker Hub**, and pull it back from anywhere.  
Perfect for **testing Docker Hub uploads** with **zero complexity**.

---

## ✅ 1️⃣ Create a Very Simple Dockerfile

Create a new folder and add a file named `Dockerfile`:

```dockerfile
FROM busybox
CMD echo "Hello from my test Docker image!"
````

### 💡 Why BusyBox?

* It's one of the **smallest Linux images**
* Downloads extremely fast
* Ideal for learning Docker image creation

---

## ✅ 2️⃣ Build the Docker Image

Navigate to the folder containing your Dockerfile:

```bash
docker build -t simple-test-image .
```

* `-t simple-test-image` → names the image
* `.` → tells Docker to use the current folder

---

## ✅ 3️⃣ Run the Image to Test It

```bash
docker run simple-test-image
```

Expected output:

```
Hello from my test Docker image!
```

✔️ If you see that message, your image works.

---

## ✅ 4️⃣ Tag the Image for Docker Hub

Replace **yourname** with your Docker Hub username:

```bash
docker tag simple-test-image yourname/simple-test-image:1.0
```

Example:

```bash
docker tag simple-test-image scarecr0w90/simple-test-image:1.0
```

### 💡 Why tag?

Docker Hub requires the image name to follow:

```
username/repository:tag
```

---

## ✅ 5️⃣ Login to Docker Hub

```bash
docker login
```
- If in wont login Generate PAT From DockerHub 
Enter your:

* Docker Hub **username**
* Docker Hub **password** (or Access Token if 2FA enabled)

---

## ✅ 6️⃣ Push the Image to Docker Hub

```bash
docker push yourname/simple-test-image:1.0
```

✔ Once done, your image becomes public on your Docker Hub profile.

---

## ✅ 7️⃣ Pull the Image From Anywhere

```bash
docker pull yourname/simple-test-image:1.0
docker run yourname/simple-test-image:1.0
```

The output will appear again:

```
Hello from my test Docker image!
```

---

# 🎉 Done!

You have now:

* Built a Docker image
* Tested it locally
* Tagged it properly
* Pushed it to Docker Hub
* Pulled it back from the cloud


# 🐳 Push Docker Image to GHCR (GitHub Container Registry)

This guide explains the **simplest and beginner-friendly method** to log in to GHCR, tag an image, push it, and verify it on your GitHub account.

---

## ✅ 1️⃣ Generate a GitHub Token

Before pushing images, you need a **GitHub Personal Access Token (PAT)**.

Create one here:  
👉 https://github.com/settings/tokens?type=beta

### Make sure the token has these permissions:
- `read:packages`
- `write:packages`
- (Optional) `delete:packages`

Copy the token — you will use it during login.

---

## ✅ 2️⃣ Login to GHCR

Use the following command:

```bash
docker login ghcr.io -u shyamdevk
````

Docker will ask for your **password** →
Paste your **GitHub Token** (NOT your GitHub password).

---

## 🏷️ 3️⃣ Tag Your Docker Image for GHCR

Format:

```
ghcr.io/USERNAME/IMAGE_NAME:TAG
```

Example for your username:

```bash
docker tag simple-test-image ghcr.io/shyamdevk/simple-test-image:1.0
```

---

## 🚀 4️⃣ Push the Image to GHCR

```bash
docker push ghcr.io/shyamdevk/simple-test-image:1.0
```

---

## 🔍 5️⃣ Verify Image on GitHub

Visit your packages page:

```
https://github.com/shyamdevk?tab=packages
```

You will see your pushed image listed there.

---

# 🎯 Quick Summary (Copy-Paste Friendly)

```bash
# Login to GHCR
docker login ghcr.io -u shyamdevk

# Tag Image
docker tag simple-test-image ghcr.io/shyamdevk/simple-test-image:1.0

# Push Image
docker push ghcr.io/shyamdevk/simple-test-image:1.0
```

---




