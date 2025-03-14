# Docker Notes

## Monolithic Architecture

- A single server and database handle all services.
- Example: E-commerce SAAS applications, Paytm (movie tickets, bookings, etc.).

### Drawbacks:
- If one service fails, the entire application must be shut down.
- Tightly coupled architecture.

## Microservices Architecture

- Each service has its own database and server.
- Loosely coupled architecture.

### Drawbacks:
- Higher maintenance cost due to multiple servers and databases.

## Why Docker?

- Helps in overcoming issues with monolithic architecture.
- Enables **virtualization** and **containerization**.

### Virtualization:

- Monolithic: All services run on a single server.
- Microservices: Each service runs on a separate server.

### Containerization:

- Packages the application along with dependencies.
- Containers are lightweight virtual machines.

## Docker Architecture

### Key Components:

1. **Docker Client** - Sends commands to Docker Daemon.
2. **Docker Host** - Stores containers, images, volumes, and networks.
3. **Docker Daemon** - Runs containers and manages Docker services.
4. **Docker Registry** - Stores and shares images (e.g., Docker Hub).

### Advantages of Docker:

- OS-level virtualization.
- Platform-independent (Docker Engine runs on Linux).
- Solves the **"Works on my machine"** problem.
- Uses containers instead of full VMs.

## Basic Docker Commands

- Install Docker:  
  ```sh
  yum install docker -y
  ```
- Check Docker version:  
  ```sh
  docker --version
  ```
- Start Docker:  
  ```sh
  systemctl start docker
  ```
- List images:  
  ```sh
  docker images
  ```
- Run a container:  
  ```sh
  docker run -it --name my_container ubuntu
  ```
- List running containers:  
  ```sh
  docker ps
  ```
- Stop a container:  
  ```sh
  docker stop my_container
  ```
- Remove a container:  
  ```sh
  docker rm my_container
  ```
- Remove an image:  
  ```sh
  docker rmi ubuntu
  ```

## Deploying a Web Server with Docker

- Pull an Apache HTTPD image:  
  ```sh
  docker pull httpd
  ```
- Run a container with Apache:  
  ```sh
  docker run -itd --name web_server -p 8081:80 httpd
  ```
- Check running containers:  
  ```sh
  docker ps -a
  ```
- Stop the web server:  
  ```sh
  docker stop web_server
  ```

## Docker Networks

- Default networks: **Bridge, Host, None, Overlay**.
- Create a custom network:  
  ```sh
  docker network create my_network
  ```
- Connect a container to a network:  
  ```sh
  docker network connect my_network my_container
  ```
- Disconnect a container from a network:  
  ```sh
  docker network disconnect my_network my_container
  ```
- Remove a network:  
  ```sh
  docker network rm my_network
  ```

## Docker Compose

- Use **docker-compose.yml** to define multi-container applications.
- Start all services:  
  ```sh
  docker-compose up -d
  ```
- Stop services:  
  ```sh
  docker-compose down
  ```

## Docker Swarm

- Orchestration service for managing multiple Docker containers.
- Initialize Swarm:  
  ```sh
  docker swarm init --advertise-addr <PrivateIP>
  ```
- List Swarm nodes:  
  ```sh
  docker node ls
  ```
- Deploy a service:  
  ```sh
  docker service create --name my_service --replicas 3 --publish 8081:80 httpd
  ```
- Scale service:  
  ```sh
  docker service scale my_service=5
  ```
- Remove a service:  
  ```sh
  docker service rm my_service
  ```

## Docker Registry

- Store images in **Docker Hub** or **private registry**.
- Login to Docker Hub:  
  ```sh
  docker login
  ```
- Push an image:  
  ```sh
  docker tag my_image dockerhub_user/repository_name
  docker push dockerhub_user/repository_name
  ```
- Pull an image:  
  ```sh
  docker pull dockerhub_user/repository_name
  ```

## Docker Portainer (GUI for Docker Management)

- Install Portainer:  
  ```sh
  docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
  ```
- Access via browser: `http://localhost:9000`

---

This Markdown file summarizes key concepts and commands from the **Docker Notes** PDF.
