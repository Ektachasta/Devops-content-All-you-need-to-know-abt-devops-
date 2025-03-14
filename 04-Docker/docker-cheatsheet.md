# Docker Cheat Sheet

## Build, Ship, Run

### Build

- **Build an image from the Dockerfile in the current directory and tag the image**  
  ```sh
  docker build -t myapp:1.0 .
  ```
- **List all images that are locally stored with the Docker engine**  
  ```sh
  docker images
  ```
- **Delete an image from the local image store**  
  ```sh
  docker rmi alpine:3.4
  ```

### Swarm

- **Initialize swarm mode and listen on a specific interface**  
  ```sh
  docker swarm init --advertise-addr 10.1.0.2
  ```
- **Join an existing swarm as a manager node**  
  ```sh
  docker swarm join --token <manager-token> 10.1.0.2:2377
  ```
- **Join an existing swarm as a worker node**  
  ```sh
  docker swarm join --token <worker-token> 10.1.0.2:2377
  ```
- **List the nodes participating in a swarm**  
  ```sh
  docker node ls
  ```

### Service

- **Create a service from an image exposed on a specific port and deploy 3 instances**  
  ```sh
  docker service create --replicas 3 -p 80:80 --name web nginx
  ```
- **List the services running in a swarm**  
  ```sh
  docker service ls
  ```
- **Scale a service**  
  ```sh
  docker service scale web=5
  ```
- **List the tasks of a service**  
  ```sh
  docker service ps web
  ```

### Image Management

- **Pull an image from a registry**  
  ```sh
  docker pull alpine:3.4
  ```
- **Retag a local image with a new image name and tag**  
  ```sh
  docker tag alpine:3.4 myrepo/myalpine:3.4
  ```
- **Log in to a registry (the Docker Hub by default)**  
  ```sh
  docker login my.registry.com:8000
  ```
- **Push an image to a registry**  
  ```sh
  docker push myrepo/myalpine:3.4
  ```

### Running Containers

- **Run a container with specific options**  
  ```sh
  docker run --rm -it --name web -p 5000:80 -v ~/dev:/code alpine:3.4 /bin/sh
  ```
- **Stop a running container through SIGTERM**  
  ```sh
  docker stop web
  ```
- **Stop a running container through SIGKILL**  
  ```sh
  docker kill web
  ```

### Networking

- **Create an overlay network and specify a subnet**  
  ```sh
  docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 -d overlay mynet
  ```
- **List the networks**  
  ```sh
  docker network ls
  ```

### Containers

- **List the running containers**  
  ```sh
  docker ps
  ```
- **Delete all running and stopped containers**  
  ```sh
  docker rm -f $(docker ps -aq)
  ```
- **Create a new bash process inside the container and connect it to the terminal**  
  ```sh
  docker exec -it web bash
  ```
- **Print the last 100 lines of a container’s logs**  
  ```sh
  docker logs --tail 100 web
  ```

---

For more details, visit: [Docker](https://www.docker.com/getdocker)
