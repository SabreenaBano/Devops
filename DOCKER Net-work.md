# Docker commands :
## container commands
Docker PS (-l flag: shows us the latest container) , (-q flag: shows only the Id of the containers). 
# Docker rm:
  docker rm {options} <container_name or ID>:
  Some important flags:
      -f flag: remove the container forcefully.
       -v flag: remove the volumes.
      -l flag: remove the specific link mentioned.
 docker stop <container_id1> <container_id2> (to stop multiple containers simultaneously)
 docker stop $(docker ps -q)  (stop all running containers at once)
 docker rm $(docker ps -a -q) (remove all containers at once)
 docker run -d --name my_nginx_container nginx (run and adding tag to container)

 
 ## image commands
     docker rmi -f $(docker images -q) (remove all images at once)
     docker images (to list the images)
      docker tag image_id_or_name1 new_image_name1:new_tag1 && docker tag image_id_or_name2 new_image_name2:new_tag2 (assign tag to images at once)
# DOCKER NETWORKING,,,,,,,,,,,,,,,,,
## Drivers:
   The following network drivers are available by default, and provide core networking functionality:
   Driver	Description
   bridge:	The default network driver.
   host  :	Remove network isolation between the container and the Docker host.
   none :	Completely isolate a container from the host and other containers.
   overlay :	Overlay networks connect multiple Docker daemons together.
   ipvlan :	IPvlan networks provide full control over both IPv4 and IPv6 addressing(creates own n/w with customized ip range )
   macvlan :	Assign a MAC address to a container.
   
   1.docker exec -it nc bash (getting insite the container)
   2.Update the package list (if not done yet) and install iputils-ping, which provides the ping     command. Since sudo is not available, you’ll need to use apt-get directly as the root user.
         apt-get update
         apt-get install -y iputils-ping
   3. ping google.com.
   4. to see ip addr : hostname -i
   
   ##  Bridge Network
   1. docker network create my_bridge_network (creating bridge-n/w)
   2. Run containers on the bridge network:
           docker run -d --name my_nginx --network my_bridge_network nginx
           docker run -it --name my_ubuntu --network my_bridge_network ubuntu
   3.Test communication between containers:
           docker exec -it my_ubuntu /bin/bash
           ping my_nginx (got error)
           apt-get update && apt-get install -y iputils-ping (sol.first need to install ping)
           ping my_nginx (successfully pinging)

# HOST NETWORKING: (rm n/w isolation b/w host and container)
     
  NOTE: In Docker, if you want an existing container to use the host network mode, you need to recreate the container with the --network host option. This is because Docker does not allow you to change the network mode of a running or stopped container directly.
      1.docker run -d --name <container_name> --network host <image_name>
      2.docker run -d --name my_nginx --network host nginx (eg)
         docker run -d --network host --name my_nginx nginx
         docker run -it --network host --name container2 ubuntu /bin/bash
      3.docker ps (error: not able to start the container because port was already in use)docker 4.logs my_nginx (sol: see logs and stop service using the same port)
      5.sudo service apache2 stop (eg)
     
# Create a Bridge Network: (for existing containers)
              docker network create my_bridge_network
Connect Existing Containers:
              docker network connect my_bridge_network container1
              docker network connect my_bridge_network container2
Inspect the Bridge Network:
              docker network inspect my_bridge_network
Test Communication:
Enter container1:
               docker exec -it container1 /bin/bash
Ping container2:
               ping container2 (apt-get update,apt-get install -y iputils-ping)
)
If container2 has a web server:
             curl http://container2:80 (apt-get update, apt-get install -y curl)

## Docker Integeration with jenkins:
Jenkins User Docker Access:
    sudo usermod -aG docker jenkins
pre-requisites
1.install docker on jenkins server.
2.jdk
3.maven (sudo apt install maven -y)
4.sudo apt install nodejs npm -y (nmp -v, node -v)

Plugins:
 1. Eclipse Temurin installer
 2. openJDK-native-plugin
 3. docker (integerates jenkins with docker)
 4. docker pipeline (Build and use Docker containers from pipelines.)
 5. docker-build-step(This plugin allows to add various docker commands to your job as build step)
 6. Docker Slaves(Uses Docker containers to run Jenkins build agents).


    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    




   
 

            
 