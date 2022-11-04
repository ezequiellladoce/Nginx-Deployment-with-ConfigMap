# k8s
## Deploy a statless nginx app

### Create namespace

kubectl create namespace nginx-deployment

kubectl get namespace

kubectl apply -f deployment.yaml -n nginx-deployment

kubectl get pods -n nginx-deployment

### create service

kubectl apply -f service.yaml -n nginx-deployment