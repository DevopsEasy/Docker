## Docker Multi Host Networking
* In Docker Overlay network driver provides multi host networking
* [Refer Here](https://directdevops.blog/2019/10/07/docker-networking-series-ii-overlay-networks/) to understand overlay networks
* To create a overlay network in docker we use docker swarm.
* Docker Swarm gives management & Orchestration features
* In Docker Swarm we specify desired state (i want some application to be run in two containers and swarm will try to maintain the state)

![Preview](./Images/docker151.png)

* Lets create 3 nodes in aws and install docker in it

![Preview](./Images/docker152.png)

* Lets understand some key terms in docker swarm
    * Swarm: Multiple Docker hosts run in swarm mode to act as managers and workers
    * Task: The manager distributes the tasks to run inside the nodes. A task consists of a Docker container and the commands to run inside the container
    * Service: This defines the task
    * Node: An individual system with docker installed
* Manager node initializes the swarm cluster by using the ip address of manager

![Preview](./Images/docker153.png)

![Preview](./Images/docker154.png)

* Now lets login into the nodes which would like to join the swarm as a worker node

![Preview](./Images/docker155.png)

![Preview](./Images/docker156.png)

* now lets login into the worker node and execute the following

![Preview](./Images/docker157.png)

* Lets try to create a service where we describe the number of replicas

```
docker service create --replicas 2 --name httpd httpd:latest
```

![Preview](./Images/docker158.png)

![Preview](./Images/docker159.png)

![Preview](./Images/docker160.png)

* Now lets scale the number of replicas

```
docker service scale httpd=4
```

![Preview](./Images/docker161.png)

![Preview](./Images/docker162.png)

* Lets create a service by exposing ports

![Preview](./Images/docker163.png)

![Preview](./Images/docker164.png)

![Preview](./Images/docker165.png)

* Lets login into the container and check the network interfaces

![Preview](./Images/docker166.png)

![Preview](./Images/docker167.png)
