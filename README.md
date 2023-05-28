# Docker-Wordpress-site
WordPress is a free and open source blogging tool and a content management system (CMS) based on PHP and MySQL, which runs on a web hosting service.

### Reference link
https://hub.docker.com/_/wordpress

### Prerequisites
Need to install docker on your machine
Add your username to docker group

### Docker Installation

```
sudo yum install docker -y &> /dev/null
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo usermod -a -G docker ec2-user
```
![docker_installation](https://github.com/Nisha-Sugathan/Docker-Bind_mounting/assets/134600837/ba7797c4-9a73-4ce6-b593-2befa5850e0d)


### Created a bridge network
```
 docker network create wp-network
 
```
### WORDPRESS installation 
We need to create a separate container for MySQL server

```
docker container run -d --name mysql-server --network wp-network \
-p 3036:3036 \
-e MYSQL_ROOT_PASSWORD="mysqlroot123"  \
-e  MYSQL_DATABASE="wordpress" \
-e MYSQL_USER="wpuser"  \
-e MYSQL_PASSWORD="wpuser123"  \
mysql:8.0.33-debian

```

Below are the list of flags used to create the container
-d          ---> This flag is used to specify conatiner to run on detached mode.
--name      -----> assign aunique nage to container.
---network  ----> The container should be within the "wp-network".
-p          ----> publish the port to specified port.
-e          ----> environment variables for configuring the WordPress instance.
