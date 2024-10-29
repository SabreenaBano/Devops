## Docker 
# Docker installation
1 Update the System:
      sudo apt update
      sudo apt upgrade -y
Step 2: Install Required Packages(Install packages that allow apt to use repositories over HTTPS:)
       sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
Step 3: Add Docker’s Official GPG Key
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
Step 4: Add Docker’s Repository
        echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee           /etc/apt/sources.list.d/docker.list > /dev/null

5.Install Docker
   sudo apt update
   sudo apt install -y docker-ce docker-ce-cli containerd.io
6.Verify Docker Installation
   docker --version
7.verify docker is working
   docker run hello-world
## Docker commands:
      docker pull nginx
      docker images
      docker run nginx (cant't run any other commands on terminal if we exit the app will stop)
      so we need to run the app in detached mode 
        docker run -d nginx (detached- mode runs in background, creates new container)
        docker ps -a: Shows all containers, including those that are stopped.
        docker ps -q: Lists only container IDs, which is useful for scripting (e.g., stopping all containers).
        docker stop container name
        docker remove container name/id (to remove a cotainer)
        docker remove -f container name (forcefully stops and removes container)
        docker run -d --rm nginx (creates container and if stoped automatically gets removed)
        docker run -d --rm --name nc nginx (assigns name to the container)
        docker run -d --rm --name nc -p 8080:80 nginx (port binding)
        docker logs con name
        docker logs -f contain name (to see live logs)
        docker attach container name (attached mode)
        docker inspect container name (command provides detailed information about Docker objects, such as containers, images, networks, and volumes)
        docker exec -it <container-name> <action> command allows you to run commands inside a running Docker container
           eg docker exec -it nc bash
              ls
        cd usr/share/nginx/html (loc of website)
        exit (to come out of container)
        docker exec nc ls / (lists files in root dir of the container)
        docker build -t my-app-image . (to build image)
# Docker volumes:
Docker volumes are used for persisting and managing data outside of the container's filesystem. Unlike the filesystem of a container, which is ephemeral (i.e., data is lost when the container is removed), volumes provide a way to store data persistently, allowing you to share data between containers or keep it intact even when containers are recreated.
       docker volume create my-volume
       docker run -d --name my-container -v my-volume:/data nginx
       docker volume ls
       docker volume inspect my-volume
       docker volume rm my-volume
  DOCKER LOGIN 
       docker login (enter docker hub cred)
       docker push saba377/docker-repo:01 (Assuming you have already built your Docker image (e.g., saba377/docker-repo:01), you can push it to Docker Hub using the following command:)

# Docker Compose:
Docker Compose is a tool used for defining and running multi-container Docker applications. With Docker Compose, you can use a YAML file (docker-compose.yml) to configure your application's services, networks, and volumes, making it easier to manage the deployment of multiple containers as a single application.

# DOCKER NETWORKS:
Docker networks provide a way for containers to communicate with each other and with the host system. By default, Docker containers can communicate with each other if they are on the same network. Networks help to isolate containers and manage how they connect to one another and to external systems.
       docker network ls
       docker network create my-network
Run Two Containers on the Same Network: For example, run two containers, a web server and a database, on the same network:
       docker run -d --name my-db --network my-network mongo
       docker run -d --name my-web --network my-network -p 8080:80 nginx
       docker network inspect my-network (detailed info)

 



    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    




   
 

            
 