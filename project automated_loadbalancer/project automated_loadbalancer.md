# AUTOMATING LOADBALANCER CONFIGURATION AND SHELL SCRIPTING

This project shows how to automate the setup and maintenance of your load balancer using a freestyle job,enhancing efficiency and reducing manual effort.

## AUTOMATE THE DEVELOPMENT OF WEBSERVERS

We will be writing a shell script to automate the entire process of seting up our backend webservers.

### DEPLOYING AND CONFIGURING THE WEBSERVERS

All the process we need to deploy has been codified in a shell script,

```
#!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

sudo systemctl restart apache2

```

 and we will create an implement using the steps below to run the script.

*Step 1:* Provision 2 EC2 instances running ubunto 20.04 to act as our servers

*Step 2:* Open port 8000 to allow traffic form anywhere using the security group.

*Step 3:* connect to the web server using SSH client

*Step 4:* Open a file, paste the script and close the file using the command:

`sudo vi install.sh`

![apacheconfig](./images/config.jpg)

*Step 5:* Change the permission on the file to make it executable

`sudo chmod +x install.sh`

*Step 6:* Run the shell script using the command below

`./install.sh PUBLIC_IP`

![server1](./images/server1.jpg)
![server2](./images/server2.jpg)

Ensure to run the script on the two servers.

### DEPLOYING AND CONFIGURING NGINX LOAD BALANCER   

All steps in the implementing of a load balancer has also been codified into a shell script,

```

#!/bin/bash

######################################################################################################################
##### This automates the configuration of Nginx to act as a load balancer
##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
#####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
############################################################################################################# 

PUBLIC_IP=$1
firstWebserver=$2
secondWebserver=$3

[ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

[ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

[ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure


sudo apt update -y && sudo apt install nginx -y
sudo systemctl status nginx

if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
fi

sudo nginx -t

sudo systemctl restart nginx

```

 We shall also create the script and run it using the steps below:

*Step 1:* on the terminal run 

`sudo vi nginx.sh`

*Step 2:* Copy and paste the script into the file save and close the file

![config](./images/nginx%20config.jpg)

*Step 3:* Change the permission on the file to make it executable

`sudo chmod +x nginx.sh`

*Step 4:* Run the script with the following command

`./nginx.sh PUBLIC_IP Webserver-1 Webserver-2`

![shell](./images/load1.jpg)
![shell2](./images/load2.jpg)

you will also notice the load balancer toggles between the two webservers, shown by the change in public IP pictured above.

Thank you.












