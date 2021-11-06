KUBERNETES COMMANDS EXECUTED


# Kaniko (Google) to create Images
kubectl create secret docker-registry dockercred --docker-server=https://index.docker.io/v1/ --docker-username=omartini --docker-password=Martini!383940 --docker-email=osvaldo.martini@gmail.com

### Accessing Docker Inside Minikube
#### Start minikube
minikube start
#### Set docker env
eval $(minikube docker-env)             # unix shells
minikube docker-env | Invoke-Expression # PowerShell
#### Build image
docker build -t foo:0.0.1 .
#### Run in minikube
kubectl run hello-foo --image=foo:0.0.1 --image-pull-policy=Never
#### Check that it's running
kubectl get pods

### Kompose
curl -L https://github.com/kubernetes/kompose/releases/download/v1.24.0/kompose-windows-amd64.exe -o kompose.exe

## As Administrator
minikube start --driver=docker --vm-driver=hyperv --memory 4096

dism.exe /Online /Disable-Feature:Microsoft-Hyper-V
and
dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

## As Administrator
minikube start --vm-driver=hyperv --hyperv-virtual-switch --cpus=4 --memory=6000 eval $(minikube docker-env)

minikube start --driver=docker --hyperv-virtual-switch "Minikube Switch" --cpus=4 --memory=6000 eval $(minikube docker-env)

minikube start --hyperv-virtual-switch "Minikube Switch"

```
	I recommend before starting this section that you set up minikube with plenty of RAM. To do this:
	Stop minikube with "minikube stop"
	Delete your existing minikube with "minikube delete"
	Remove any config files with 
	"rm -rf ~/.kube" and "rm -rf ~/.minikube". Or, 
	delete these folders using your file explorer. 
	The folders are stored in your home directory, 
	which under windows will be "c:\Users\<your username>"
	Now restart minikube with "minikube start --memory 4096".
```

kubectl version
kubectl apply -f first-pod.yaml
kubectl describe pod webapp
kubectl exec webapp ls   ->  executes cmds inside the pod  -> list of directories
kubectl -it exec webapp sh -> it prompts the shell line command inside the pod
INSIDE POD: # wget http://localhost:80
INSIDE POD: # cat index.html
INSIDE POD: # exit
It proves we already have a webserver inside the POD


PODs: come and go, it can be constantly restarted - > Short life
		kind: Pod/ReplicaSet/Deployment
		
Services: are Long Live Entities, is the front for PODs , Stable Ip Addresses

Services:   
Important:
	labels: and  selector: to the service indentifies the pod configs
	type: ClusterIP Acessible Only from Inside the Cluster 
	type: NodePort Acessible from Outside  the Cluster  
	"ZERO down time" -> Changing the Pods without stopping the application

kubectl apply -f webapp-service.yaml
minikube ip  ->  get the ip from the cluster  
	172.23.159.176

kubectl describe service fleetman-webapp 
kubectl get pods
kubectl get po --show-labels
kubectl get po --show-labels -l release=0
kubectl delete pod "name"

#### Apply all yaml files within specific folder
kubectl apply -f .
kubectl delete po --all
kubectl delete pods --all
kubectl delete replicaset --all
kubectl describe rs

### ReplicatSet it was created Chapeter 8
 

### Deployment we get rolling Updates with Zero downtime
	By Defaul stall 10 last rollouts (3 -> I can see only three)
	
 Deploymment is the Entity that manages the replica sets for us, with possibility to roll back, zero downtime
	Something wrong, we can ressurect the old version of the replicas sets
 Deployment we can have: 
		* A Zero downtime rolling update
		* We can do Rollback if something goes wrong
		
		## ROLLOUT Status
		kubectl rollout status deploy webapp  -> it show the progress
		kubectl rollout history deploy webapp -> it whiows the revisions
		## Go to Revisions
		kubectl rollout undo deploy webapp --to-revision=2
	
## Namespaces
			kubectl get ns
					default
					kube-public
					kube-system
			
			kubectl get po -n kube-system
			kubectl get all -n kube-system
				...
				service/kube-dns
				service/kubernetes-dashboard		
				daemonset.apps/kube-proxy		
				...
				deployment.apps/kube-dns  -> it keeps the dns pods running
				deployment.apps/kubernetes-dashboard
				
				kubectl describe svc kube-dns -n kube-system
				...
		

## Winpty Install
 CGWin
	1) Download it
	```
		wget https://github.com/rprichard/winpty/releases/download/0.4.3/winpty-0.4.3-cygwin-2.8.0-x64.tar.gz
		or
		//Redirect/Donwload a/to another file
		culr -L https://github.com/rprichard/winpty/releases/download/0.4.3/winpty-0.4.3-cygwin-2.8.0-x64.tar.gz > winpty.tar.gz
		
		it copies to local within CGWin
		
	```
2) Unpack it
	```
		//it's unpack and creates a folder /winpty-0.4.3-cygwin-2.8.0-x64
		tar xvf winpty.tar.gz
	```
2) Check the sub folder
	```
	  ls
		 cd winpty-0.4.3-cygwin-2.8.0-x64
			ls
			cd bin
			ls
			> winpty.dll winpty.exe .....
	```		
3) copy all filed into folder "/usr/local/bin/"    -> as the "path" environment within CGWin
```
	cp * /usr/bin/   or  (linux -> /usr/local/bin/)
```
3) Use Winpty
	winpty kubectl -it exec	webapp-858957ccc9-pdp6f sh

## CAT  
kubectl -it exec	webapp-858957ccc9-pdp6f sh
/# cat /etc/resolv.conf
```
	nameserver 10.96.0.10
	search default.svc.cluster.local svc.cluster.local cluster.local
	options ndots:5
```

## NSLookUp for DNS(s)
```
	nslookup google.com
	
	nslookup database
```

### Connection "Pod WebApp" to another "pod container with MySql"	
 kubectl -it exec webapp-858957ccc9-pdp6f sh
	sh commands:
	```
			# mysql
	   // if response is:
	  sh: mysql: not found
			
			// then type:
			# apk update
			
			IT SHOULD UPDATE MYSQL TO CONNECT
			
			# mysql -h database -uroot -ppassword -fleetman
			> Welcome to the MariaDB monitor
```
### Fullly Qualified Domain Name	 (FQDN)
   kubectl -it exec  webapp-858957ccc9-pdp6f sh
			# cat /etc/resolv.conf
			default.svc.cluster.local svc.cluster.local cluster.local
			# nslookup default.svc.cluster.local 



	

