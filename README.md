# elasticsearch-cluster-kubernetes

This project is use to deploy Elastic-search cluster on Kubernetes, Here we are using Elatsic-search version 5.5.0 and follow the below steps for deployment. Kube-DNS should be deployed on Kubernetes and running.

#- build docker image and push on your private registry

docker build --rm -t elasticsearch:5.5.0 .

#- you check checl image build status bu running a sample container and see logs of it, it will create a single node elastci search cluster

docker run --privileged -d --name elastic --hostname elastic elasticsearch:5.5.0 

docker logs elastic


#- create elasticsearch headless service 

kubectl create -f elastic-headless.yaml

#- create statefull set 3 node cluster

modify the [REGISTRY-NAME] value in elastic-statful.yaml with your registry url and also change your Kube-DNS domain name.
kubectl create -f elastic-stateful.yaml


# check health and status of elasticsearch cluster

curl http://elasticsearch-0.els-headless.default.svc.kubernetes:9200/_cat/nodes?v

curl http://elasticsearch-0.els-headless.default.svc.kubernetes:9200/_cat/health?v

# Note

elastic-statful.yaml is only for test purpose, for production, please use persistance storage to store data
