# nodejs1-kube
Nodejs with docker container push to Kubernetes

## cara build

###containerize
docker build -t image .
docker run --name app -p 3000:3000 image
docker ps

docker tag image-container user-dockerhub/image

###deploy kube
kubectl create ns namespace

#### deployment create
kubectl apply -f deployment -n namespace

#### service 
kubectl apply -f service -n namespace

#### ingress
kubectl apply -f ingress -n namespace

### check app
curl localhost/

