# CONTAINER MIGRATION PROJECT FROM VM(EC2) TO CONTAINERS

Discover the art of container migration from VM (EC2) to containers in this project, modernizing your application deployment and achieving scalability, portability, and resource efficiency

## MIGRATION TO THE СLOUD WITH CONTAINERIZATION

Until now, you have been using VMs (AWS EC2) in Amazon Virtual Private Cloud (AWS VPC) to deploy your web solutions, and it works well in many cases. You have learned how easy to spin up and configure a new EC2 manually or with such tools as Terraform and Ansible to automate provisioning and configuration. You have also deployed two different websites on the same VM; this approach is scalable, but to some extent; imagine what if you need to deploy many small applications (it can be web front-end, web-backend, processing jobs, monitoring, logging solutions, etc.) and some of the applications will require various OS and runtimes of different versions and conflicting dependencies - in such case you would need to spin up serves for each group of applications with the exact OS/runtime/dependencies requirements. When it scales out to tens/hundreds and even thousands of applications (e.g., when we talk of microservice architecture), this approach becomes very tedious and challenging to maintain.

In this project, we will learn how to solve this problem and begin to practice the technology that revolutionized application distribution and deployment back in 2013! We are talking of Containers and imply Docker. Even though there are other application containerization technologies, Docker is the standard and the default choice for shipping your app in a container.

PREREQUISITES

Install Docker and prepare for migration to the Cloud
First, we need to install Docker Engine, which is a client-server application that contains:

A server with a long-running daemon process dockerd.
APIs that specify interfaces that programs can use to talk to and instruct the Docker daemon.
A command-line interface (CLI) client docker.
You can learn how to install Docker Engine on your PC [here](https://docs.docker.com/engine/install/)

### Deploying Application Using Docker

MySQL in container

Let us start assembling our application from the Database layer - we will use a pre-built MySQL database container, configure it, and make sure it is ready to receive requests from our PHP application.

- Step 1: Pull MySQL Docker Image from [Docker Hub Registry](https://hub.docker.com/)

Start by pulling the appropriate Docker image for MySQL. You can download a specific version or opt for the latest release, as seen in the following command:

`docker pull mysql/mysql-server:latest`


If you are interested in a particular version of MySQL, replace latest with the version number. 

List the images to check that you have downloaded them successfully:

`docker images ls`

![docker-image](./images/docker%20images.png)

- Step 2: Deploy the MySQL Container to your Docker Engine

1. Once you have the image, move on to deploying a new MySQL container with:

Copy Below Code

`docker run --name <container_name> -e MYSQL_ROOT_PASSWORD=<my-secret-pw> -d mysql/mysql-server:latest`

- Replace <container_name> with the name of your choice. If you do not provide a name, Docker will generate a random one
- The -d option instructs Docker to run the container as a service in the background
- Replace <my-secret-pw> with your chosen password
- In the command above, we used the latest version tag. This tag may differ according to the image you downloaded

2. Then, check to see if the MySQL container is running: Assuming the container name specified is mysql-server

`docker ps -a`

![docker-run](./images/docker%20run.png)

You should see the newly created container listed in the output. It includes container details, one being the status of this virtual environment. The status changes from health: starting to healthy, once the setup is complete.

- Step 3: Connecting to the MySQL Docker Container
We can either connect directly to the container running the MySQL server or use a second container as a MySQL client. Let us see what the first option looks like.

Approach 1

Connecting directly to the container running the MySQL server:

`docker exec -it <container_name> mysql -uroot -p`

![docker-exec](./images/docker%20exec.png)

Provide the root password when prompted. With that, you have connected the MySQL client to the server.

Finally, change the server root password to protect your database.

Approach 2

First, create a network:

Copy Below Code

`docker network create --subnet=172.18.0.0/24 tooling_app_network `

![docker-network](./images/docker%20network.png)

Creating a custom network is not necessary because even if we do not create a network, Docker will use the default network for all the containers you run. By default, the network we created above is of DRIVER Bridge. So, also, it is the default network. You can verify this by running the docker network ls command.

But there are use cases where this is necessary. For example, if there is a requirement to control the cidr range of the containers running the entire application stack. This will be an ideal situation to create a network and specify the --subnet

For clarity's sake, we will create a network with a subnet dedicated for our project and use it for both MySQL and the application so that they can connect.

Run the MySQL Server container using the created network.

First, let us create an environment variable to store the root password:

`export MYSQL_PW=<root-secret-password>`

Then, pull the image and run the container, all in one command like below:

`docker run --network tooling_app_network -h mysqlserverhost --name=mysql-server -e MYSQL_ROOT_PASSWORD=$MYSQL_PW  -d mysql/mysql-server:latest `

Flags used

- -d runs the container in detached mode
- --network connects a container to a network
- -h specifies a hostname

If the image is not found locally, it will be downloaded from the registry.

Verify the container is running:

`docker ps -a`

![ps](./images/docker%20ps1.png)

As you already know, it is best practice not to connect to the MySQL server remotely using the root user. Therefore, we will create an SQL script that will create a user we can use to connect remotely.

Create a file and name it create_user.sql and add the below code in the file:

```
CREATE USER '<user>'@'%' IDENTIFIED BY '<client-secret-password>';
GRANT ALL PRIVILEGES ON * . * TO '<user>'@'%';
```
Run the script:

`docker exec -i mysql-server mysql -uroot -p$MYSQL_PW < ./create_user.sql`

![user](./images/docker%20create%20user.png)


### Container Registry and Image Push

Connecting to the MySQL server from a second container running the MySQL client utility

Run the MySQL Client Container:

`docker run --network tooling_app_network --name mysql-client -it --rm mysql mysql -h mysqlserverhost -u <user-created-from-the-SQL-script> -p`


![client](./images/docker%20client.png)

Flags used:

- --name gives the container a name
- -it runs in interactive mode and Allocate a pseudo-TTY
- --rm automatically removes the container when it exits
- --network connects a container to a network
- -h a MySQL flag specifying the MySQL server Container hostname
- -u user created from the SQL script
- -p password specified for the user created from the SQL script


