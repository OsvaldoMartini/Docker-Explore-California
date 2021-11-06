minikube start --cpus=4 --memory=6000 eval $(minikube docker-env)

docker run --name='activemq' -it --rm -e 'ACTIVEMQ_MIN_MEMORY=512' -e 'ACTIVEMQ_MAX_MEMORY=2048' -P	webcenter/activemq:latest