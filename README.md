# Django--App-Deployment
DevOps Project of Django App Deployment on EC2 using Docker Kubernetes Minikube Pods.


Steps
SSH to Instance
git clone project
cd django
ls
docker build -t amitchoudhary47/djago-todo:latest .
docker run -d -p 8000:8000 amitchoudhary47/djago-todo:latest
curl -L http://public ip address:8000

Creating Kubernetes Cluster
mkdir k8s
cd k8s
ls
docker images
docker push trainwithsubham/django-todo

Pod
vi pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: todo-pod
spec:
  containers:
  - name: toto-app
    image: amitchoudhary47/django-todo:latest
    ports:
    - containerPort: 8000

save file
doker ps
docker kill current running at 8000

kubectl apply -f pod.yaml

kubectl get pods

kubectl apply -f pod.yaml

vim deployment.yaml
apiVersion: apps/v1

kind: Deployment

metadata:

  name: todo-deployment

  labels:

    app: todo-app

spec:

  replicas: 3

  selector:

    matchLabels:

      app: todo-app

  template:

    metadata:

      labels:

        app: todo-app

    spec:

      containers:

      - name: todo-app

        image: amitchoudhary47/django-todo:latest

        ports:

        - containerPort: 8000

Save the file

kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods
kubectl delete pods todo-pod

kubectl delete pods todo-deployment

Checking the Running Services
kubectl get pods -o wide

curl -L http://ipaddress:8000
but it will fail
Creating a Node Pod
vim service.yaml
apiVersion: v1
kind: Service
metadata:
  name: todo-service
spec:
  type: NodePort
  selector:
    app :  todo-app
  ports:
      # By default and for convenience, the `targetPort` is set to the same value as the `port` field.
    - port: 80
      targetPort: 8000
      # Optional field
      # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
      nodePort: 30007
Save the file
kubectl apply -f service.yaml
kubectl get svc
minikube service todo-service --url
curl -L url
App Must have been deployed till now

sudo vim /etc/hosts
insert the ip
192.168.49.2 todo-app.com

save
curl -L todo-app.com:3000

kubectl get pods

Achivement : Reduced Downtime by 75% on Production Environment
