# Extending, Securing and Dockerizing Spring Boot Microservices
"Extending, Securing and Dockerizing Spring Boot Microservices"



Final Product requires External MySql Database.
Install Docker For Mac/Windows/Linux
#### Setup
Set JAVA_HOME
Set M2_HOME
Add M2_HOME/bin to the execution path
mvn package -DskipTests
#### Docker Commands
##### Start MySql Container (downloads image if not found)
``
docker run  --detach   --name ec-mysql -p 6604:3306 -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=explorecali -e MYSQL_USER=cali_user -e MYSQL_PASSWORD=cali_pass -d mysql
``

##### view all images
``
docker images
``

##### view all containers (running or not)
``
docker ps -a
``
##### Interact with Database (link to ec-mysql container) with mysql client
``
docker run -it --link ec-mysql:mysql --rm mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
``
##### Stop ec-mysql container
``
docker stop ec-mysql
``
##### (ReStart) ec-mysql container
``
docker start ec-mysql
``
##### Remove ec-mysql container (must stop it first)
``
docker rm ec-mysql
``
##### Remove image (must stop and remove container first)
``
docker rmi mysql:latest
``
#### Startup with Profile settings
##### Default profile, H2 database
``
mvn spring-boot:run
``

or

``
java  -jar target/explorecali-2.0.0-SNAPSHOT.jar
``
##### mysql profile, MySql database (requires running container ec-mysql)
``
mvn spring-boot:run -Dspring.profiles.active=mysql 
``

or

``
java  -Dspring.profiles.active=mysql -jar target/explorecali-2.0.0-SNAPSHOT.jar
``
#### Dockerize Explore California
##### Build jar
``
mvn package -DskipTests
``
##### Build Docker image
``
docker build -t explorecali .
``
###### Run Docker container with default property set in Dockerfile
``
docker run --name ec-app -d explorecali
``
##### Run Docker container with mysql profile set in Dockerfile
``
docker run    --name ec-app -p 8080:8080   --link ec-mysql:mysql -d explorecali
``
##### Run Docker container with docker profile set in Dockerfile and migration scripts on host
``
docker run --name ec-app -p 8080:8080 --rm -v ~/db/migration:/var/migration -e server=ec-mysql -e port=3306 -e dbuser=cali_user -e dbpassword=cali_pass --link ec-mysql:mysql -d explorecali
or
docker run --name ec-app -p 8080:8080 --rm -v ${pwd}/db/migration:/var/migration -e server=ec-mysql -e port=3306 -e dbuser=cali_user -e dbpassword=cali_pass --link ec-mysql:mysql -d explorecali
``
#### Shared D: for Windows
``
  ...      -v "/d/Projects DevOps/Docker-Explore-California/db/migration:/var/migration"  ...
``
##### Enter Docker container
``
docker exec -t -i ec-app /bin/bash
``


##### But if you tag your image as you build it, it will be easier to run:
``
docker build -t foo . && docker run -it foo
``