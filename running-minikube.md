# Running services in Kubernetes Cluster locally with Minikube

* Install docker 🐋

* create docker image, add tag, push to hub 🖼️

* Install kubectl 🧰

* Install & start minikube 🖥️

* Create deployment

* Expose Service

* Add prometheus & run locally

* Run prometheus & run in Minikube


####  KUBECTX

kubectx     # see which context your in
kubectx <name-of-context>   # change to a certain context


#### KUBENS

kubens    # see which namespace your in


#### KUBECTL
https://kubernetes.io/docs/tasks/tools/install-kubectl/


#### Create deployment:
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node

kubectl get deployments
kubectl get pods

#### Create service:
kubectl expose deployment hello-node --type=LoadBalancer --port=8080

"The --type=LoadBalancer flag indicates that you want to expose your Service outside of the cluster."

kubectl get services
kubectl cluster-info

#### Run service:
minikube service hello-node

## MINIKUBE
minikube start
kubectl cluster-info
kubectl get nodes
minikube status
minikube dashboard / kubectl proxy  (http://localhost:8001/ui.)
minikube addons list
minikube addons enable heapster
kubectl delete service hello-node
kubectl delete deployment hello-node
minikube stop
minikube delete (delete minikube VM)
minikube ssh
minikube get-k8s-versions
minikube start --kubernetes-version v1.7.0


## KUBECTL 
kubectl get pod,svc -n kube-system

kubectl create deployment webserver-minikube --image=sleepypioneer/webserver-in-go

kubectl get deployments


kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080

kubectl run <my.app> --labels="app=<my.app>" --image=<my.dockerhub.username>/<my.app>:latest --replicas=2 --port=8080

kubectl get deployments

kubectl expose deployment my-app --type=LoadBalancer --port=8080 --target-port=3000

kubectl patch svc webserver-minkube -n default -p '{"spec": {"type": "LoadBalancer", "externalIPs":["172.31.71.218"]}}'

kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080

"Just remember that the target-port and port arguments are synonymous to port forwarding 8080:3000 (port:target-port) as you’d do with docker run -p 8080:3000 my-app (external:internal)"

kubectl expose deployment webserver-minikube --type="NodePort" --port 8000

"Let’s run again the get services command:

kubectl get services

We have now a running Service called kubernetes-bootcamp. Here we see that the Service received a unique cluster-IP, an internal port and an external-IP (the IP of the Node).

To find out what port was opened externally (by the NodePort option) we’ll run the describe service command:

kubectl describe services webserver-minikube

Create an environment variable called NODE_PORT that has the value of the Node port assigned:

export NODE_PORT=$(kubectl get services webserver-minikube -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT

Now we can test that the app is exposed outside of the cluster using curl, the IP of the Node and the externally exposed port:

curl $(minikube ip):$NODE_PORT

And we get a response from the server. The Service is exposed.

## 🐋 DOCKER
docker container ls  
docker exec -it <container> bash  
docker stop <container>  
docker rm <container>  
docker ps  
 
docker run -P <container-name>  

docker run -p 8000:8000 <container-name>  

docker login   #loginto docker hub account  
 
docker images  

docker tag <image-name> <docker-hub-repo-name>  

docker push <docker-hub-repo-name>  

ie docker push sleepypioneer/webserver_in_go  

https://hub.docker.com  

## Prometheus

#### Run Prometheus locally

https://prometheus.io/docs/introduction/first_steps/


## 📚 RESOURCES:
https://www.linuxnix.com/how-to-push-docker-images-to-docker-hub-repository/

can link to repo so when changes are made in github repo it auto builds it
https://cloud.docker.com/repository/docker/sleepypioneer/webserver_in_go/builds






























