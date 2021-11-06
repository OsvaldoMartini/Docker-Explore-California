systemctl restart docker
Failed to restart docker.service: Unit docker.socket not found.

sudo apt-cache showpkg docker-ce

To find Directories or files

find / -xdev 2>/dev/null -name "exampledocs"

To see permissions of the folder

ls -la

Granting acces

chmod 440 /etc/sudoers
chmod -R 755 /root
chmod -R 755 /etc

sudo chmod -R 755 /usr/share/bash-completion/completions/docker

ls -la

env | grep DOCKER_HOST

minikube start --cpus=4 --memory=6000 eval $(minikube docker-env)

sudo docker ps

get the id..

docker inspect container "id"

docker inspect --format '{{ .State.Pid }}' container-id-or-name

docker inspect --format '{{ .State.Pid }}' 0e3c1da71d22

result: 4898

nsenter -t your-container-pid -n ip addr

nsenter -t 4898 -n ip addr
