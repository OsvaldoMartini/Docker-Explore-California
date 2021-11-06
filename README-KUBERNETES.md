Kubernetes Commands Get all Contairners or Images
https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/

kubectl get pods --all-namespaces


Get all Images Parsing the Json File

kubectl get all --all-namespacesdocker

kubectl get pods --all-namespaces -o jsonpath="{..image}" |tr -s '[[:space:]]' '\n' | sort | uniq -c

kubectl get pods --all-namespaces -o jsonpath="{.items[*].spec.containers[*].image}"

kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.containers[*]}{.image}{", "}{end}{end}' | sort


Install KVM and config Bridge
https://www.cyberciti.biz/faq/installing-kvm-on-ubuntu-16-04-lts-server/

Get-NetAdapter

New-VMSwitch –Name "minikube" –AllowManagement $True –NetAdapterName "INSERT_HERE_ADAPTER"

minikube stop

New-VMSwitch –Name "minikube" –AllowManagement $True –NetAdapterName "Primary Virtual Switch"

New-VMSwitch –Name "minikube" –AllowManagement $True –NetAdapterName "vEthernet (DockerNAT)"

New-VMSwitch –Name "minikube" –AllowManagement $True –NetAdapterName "DockerNA"

minikube start --vm-driver=hyperv --hyperv-virtual-switch=minikube

minikube delete


New-VMSwitch –Name "minikube" –AllowManagement $True –NetAdapterName "WiFi"

From the course FleetMan
minikube start --vm-driver="hyperv" --hyperv-virtual-switch="minikube"


"Minikube Virtual Switch"

Create as External Network Switch
See the Video:  https://www.youtube.com/watch?v=YiODgG1tneI
minikube start --kubernetes-version="v1.10.11" --vm-driver="hyperv" --memory=1024 --hyperv-virtual-switch="Minikube Virtual Switch" --v=7 --alsologtostderr

Create as Inernal Network Switch
See the Video: https://www.youtube.com/watch?v=E8VxEkyWZG8

New-VMSwitch –Name "minikube" –AllowManagement $True –NetAdapterName "WiFi"

From the course FleetMan
minikube start --vm-driver="hyperv" --hyperv-virtual-switch="minikube"

minikube start --kubernetes-version="v1.10.11" --vm-driver="hyperv" --memory=1024 --hyperv-virtual-switch="minikube" --v=7 --alsologtostderr


DOCKER_REGISTRY_SERVER=docker.io
DOCKER_USER=omartini
DOCKER_EMAIL=osvaldo.martini@gmail.com
DOCKER_PASSWORD=martini383940

kubectl create secret docker-registry myregistrykey \
  --docker-server=$DOCKER_REGISTRY_SERVER \
  --docker-username=$DOCKER_USER \
  --docker-password=$DOCKER_PASSWORD \
  --docker-email=$DOCKER_EMAIL


kubectl create secret docker-registry myregistrykey --docker-server=docker.io --docker-username=omartini --docker-password=martini383940 --docker-email=osvaldo.martini@gmail.com



Remove all Unsued Containers:
	docker image prune
Remove All Ununsed Images
	docker image prune -a


Remove All Ununsed Columes
docker system prune --volumes


docker container prune --filter "until=12h"

minikube access:
  login:    docker
  password: tcuser


VT-x is not enabled in the BIOS
The CPU doesn't support VT-x
Hyper-V virtualization is enabled in Windows
Since the user already eliminated the first two possible culprits, the next step is to open a command prompt as administrator and run the following command:

dism.exe /Online /Disable-Feature:Microsoft-Hyper-V
Afterwards, reboot the PC and try VirtualBox again.

dism.exe /Online /Disable-Feature:Microsoft-Hyper-V

and

dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All


Installing Kubernetes:

Create a Directory "D:\Kubernetes"

Install-Script -Name install-kubectl -Scope CurrentUser -Force
install-kubectl.ps1 [-DownloadLocation <path>]



Install-Script -Name install-kubectl -Scope CurrentUser -Force
install-kubectl.ps1 -DownloadLocation D:\Kubernetes






DOCKER 
	Install SOA suite

	docker login

	user: omartini
	pwd: martini383940

	docker network create -d bridge SOANet

	Kubernets CMD

swarm (enxame/cardume/bando)

   //get nodes
	kubectl get nodes   

	ActiveMQ
	//Get Latest
	docker pull webcenter/activemq:latest




	Create a Docker file:  "Dockerfile"  without extension

	
FROM openjdk:8-jre-alpine 
RUN wget -O activemq.tar.gz http://archive.apache.org/dist/activemq/5.15.6/apache-activemq-5.15.6-bin.tar.gz
RUN tar -xzf activemq.tar.gz
CMD ["apache-activemq-5.15.6/bin/activemq", "console"]


go to the docker file directory and run:

From inside of the directory that contias the "Dockerfile" 

docker build .

docker images


Note that initially you will notice <none> under repository. In order to tag the image, run command docker tag <IMAGE_ID> <yourdockerhubname>/image-name.

TAG
docker tag 316120e512d3 development/k8s-make-activemq


 docker run <name-of-the-repository>

 docker run development/k8s-make-activemq

Get All IPs Address running
 docker ps  

STOP
docker container ls
docker container stop <NAME>


Minikube
minikube version

 To find the minikube ip
	
	minikube ip

Start the cluster, by running the minikube start command:

minikube start

minikube ip

kubectl version

kubectl cluster-info

kubectl get nodes


