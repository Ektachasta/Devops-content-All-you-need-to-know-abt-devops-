# 🐳 Containerization with Docker  

This module covers everything you need to know about **Docker**, from basic concepts to advanced topics like networking and volumes. Whether you're a beginner or an experienced developer, this guide will help you master containerization.  
Imagine a situation where your application works fine on your local machine but fails to run on a different system because of software conflicts. Docker **solves this problem** by bundling everything (code, libraries, dependencies) into one package that runs consistently across different environments.  

---

## 🚀 What is Docker?  

Docker is a **containerization platform** that enables developers to package applications and their dependencies into **lightweight, portable containers**. These containers ensure that applications run consistently across different environments, eliminating issues like "it works on my machine" problems.  

### 🛑 The Problem Without Docker  
Imagine a situation where:  
- Your app works on your machine but **fails on another developer’s machine**.  
- You need to manually **install dependencies** every time you set up a project.  
- Your app **breaks** when moving from **development to production** due to version mismatches.  

### ✅ The Solution With Docker  
Docker solves these issues by **bundling everything** (code, libraries, dependencies) into one package that runs **identically across all environments**. 

### ✨ Key Benefits of Docker:  
✅ **Portability** – Run your application anywhere (Windows, macOS, Linux, cloud).  
✅ **Isolation** – Avoid conflicts between different dependencies.  
✅ **Efficiency** – Uses fewer resources than virtual machines.  
✅ **Scalability** – Easily scale applications using container orchestration.  
✅ **Speed** – Containers start almost instantly compared to traditional VMs.

---

## 📖 Topics Covered  

### 📌 [Docker Basics]  
**Introduction to Docker and Core Commands**  

🔹 **What is Docker?** A deep dive into how Docker works.  
🔹 **Installing Docker** on Windows, macOS, and Linux.  
🔹 **Core Commands:**  
   - `docker run <image>` → Run a container.  
   - `docker ps` → List running containers.  
   - `docker stop <container>` → Stop a running container.  
   - `docker rm <container>` → Remove a stopped container.  
🔹 **Working with Images:**  
   - Pulling images from **Docker Hub** (`docker pull <image>`).  
   - Creating custom images.  
   - Managing and removing images.  

🔹 **Common Issues & Fixes:**  
🚨 *Error: "docker: command not found"* → Ensure Docker is installed and running.  
🚨 *Error: "Cannot connect to Docker daemon"* → Run Docker with admin privileges.

---

### 📌 [Writing a Dockerfile]  
**Guide to Writing and Optimizing Dockerfiles**  

🔹 **What is a Dockerfile?**  
   - A script that automates the creation of Docker images.  
   - Defines the environment and dependencies for an application.  

🔹 **Essential Dockerfile Instructions:**  
   - `FROM <image>` → Specifies the base image.  
   - `RUN <command>` → Executes commands inside the container.  
   - `COPY <src> <dest>` → Copies files into the container.  
   - `CMD ["command"]` → Defines the default command to run.  
   - `ENTRYPOINT ["command"]` → Specifies a fixed entry point.  

🔹 **Best Practices for Optimized Dockerfiles:**  
   - Use **small base images** (e.g., `alpine` instead of `ubuntu`).  
   - Minimize the number of layers.  
   - Reduce the size of the final image.  

🔹 **Example Dockerfile:**  
```Dockerfile
# Use the official Python image as the base
FROM python:3.9

# Set the working directory inside the container
WORKDIR /app

# Copy all files from the local folder to the container
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Run the application
CMD ["python", "app.py"]
```

🔹 **Building and Running an Image from a Dockerfile:**  
```sh
docker build -t my-python-app .
docker run my-python-app
```

🔹 **Best Practices for Optimized Dockerfiles:**  
   - Use **small base images** (e.g., `alpine` instead of `ubuntu`).  
   - Minimize the number of layers.  
   - Reduce the size of the final image.  

---

### 📌 [Docker Compose]  
**Managing Multi-Container Applications**  

🔹 **What is Docker Compose?**  
   - A tool for defining and managing multi-container applications.  
   - Uses a **YAML file** (`docker-compose.yml`) to configure services.  

🔹 **Writing a Basic `docker-compose.yml` File:**  
```yaml
version: "3"
services:
  app:
    image: my-app
    ports:
      - "5000:5000"
    volumes:
      - ./data:/app/data
```

🔹 **Running Docker Compose Commands:**  
   - `docker-compose up -d` → Start the application in detached mode.  
   - `docker-compose down` → Stop and remove all containers.  
   - `docker-compose logs` → View logs for running containers.  

🚨 *Troubleshooting:*  
⚠️ *Error: "No such service"* → Ensure the service name matches your `docker-compose.yml`.
---

### 📌 [Docker Networking]  
**Understanding Docker Networks**  

🔹 **Types of Docker Networks:**  
   - **Bridge (default):** Allows containers to communicate with each other.  
   - **Host:** Removes network isolation; container uses the host network.  
   - **Overlay:** Used in Docker Swarm for multi-host communication.  

🔹 **Networking Commands:**  
   - `docker network ls` → List available networks.  
   - `docker network create my_network` → Create a new network.  
   - `docker network connect my_network my_container` → Connect a container to a network.  

🔹 **Linking Containers:**  
   - Use **container names** instead of IP addresses.  
   - Example: `docker run --network my_network --name web nginx`.

🔹 **Example: Connecting Containers in a Custom Network:**  
```sh
docker network create my_custom_network
docker run --network my_custom_network --name web nginx
docker run --network my_custom_network --name app my-app
```
🚨 *Troubleshooting:*  
⚠️ *Error: "network not found"* → Check with `docker network ls`.

---

### 📌 [Docker Volumes]  
**Persistent Storage with Docker**  

🔹 **What is a Docker Volume?**  
   - A method for persisting data outside of a container.  
   - Ensures data is not lost when a container is removed.  

🔹 **Using Volumes in Docker:**  
   - Creating a volume: `docker volume create my_volume`  
   - Running a container with a volume:  
     ```sh
     docker run -v my_volume:/data my_image
     ```
   - Mounting a host directory:  
     ```sh
     docker run -v /path/on/host:/path/in/container my_image
     ```
     
🔹 **Types of Storage in Docker:**  
   - **Volumes:** Managed by Docker, stored outside the container’s filesystem.  
   - **Bind Mounts:** Directly maps a host directory to a container.  
   - **Tmpfs Mounts:** Temporary storage in memory.

🔹 **Managing Volumes:**  
   - List all volumes: `docker volume ls`  
   - Inspect a volume: `docker volume inspect my_volume`  
   - Remove a volume: `docker volume rm my_volume`  

🚨 *Troubleshooting:*  
⚠️ *Error: "volume is in use"* → Stop running containers before deleting. 
---

## 🔗 Additional Resources  
📌 [Official Docker Documentation](https://docs.docker.com/)  
📌 [Docker Hub](https://hub.docker.com/)  
📌 [Docker Cheat Sheet](https://dockerlabs.collabnix.com/docker/cheatsheet/)  

🚀 **Happy Containerizing!** 🐳  
