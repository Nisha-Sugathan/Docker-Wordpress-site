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
* -d          ---> Docker runs container in the background.
* --name      -----> assign aunique name to container.
* ---network  ----> The container should be within the "wp-network".
* -p          ----> asks Docker to forward traffic incoming on the host’s port xxxx to the container’s port xxxx.
* -e          ----> environment variables for configuring the WordPress instance.

We need to create a separate container for Wordpress frontend
```

docker container run -d --name wordpress --network wp-network \
-p 80:80 \
-e WORDPRESS_DB_HOST="mysql-server"  \
-e WORDPRESS_DB_USER="wpuser"  \
-e WORDPRESS_DB_PASSWORD="wpuser123"  \
-e WORDPRESS_DB_NAME="wordpress" \
wordpress:latest

```

### Screenshot of list of containers

![image](https://github.com/Nisha-Sugathan/Docker-Wordpress-site/assets/134600837/d58ccc68-0345-44bd-b303-563c6228a38f)


### Connecting mysql Database from ec2 instance

We can get the IP address from the docker inspect command and then we can connect with mysqlmysql -u root -pmysqlroot123 -h 172.18.0.2

```
docker container inspect mysql-server
mysql -u root -pmysqlroot123 -h 172.18.0.2

```

