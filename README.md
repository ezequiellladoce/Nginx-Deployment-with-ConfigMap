# Simple k8s Nginx Deploymet with ConfigMaps

## Objetives

- Deploy a simple stateless nginx application with a Deploymet 
- Add a LoadBalancer  service expose the app
- Use a ConfigMap to configure the index page  

## Prerequisites 📋

- A k8s cluster v1.9 or later, for testing purposes can be minikube (https://minikube.sigs.k8s.io/docs/). You can check de k8s server with the command kubectl version
- kubectl configured to communicate with the cluster
  
## Starting 🚀

## Repo description

- deployment.yaml this YAML file describes a Deployment that runs the nginx with 3 replicas
- service.yaml this YAML file describes a load balancer service that  exposes the pods created by the deployment 
- configmap.yaml this YAML is used to configure the index page for the Nginx

## Clone the repository 

  ```
  git clone https://github.com/ezequiellladoce/Nginx-Deployment-with-ConfigMap.git
  ```
  
## Deploy 📦  

Enter to the repo folder

### Create namespace

```
kubectl create namespace nginx-deployment

kubectl get namespace
```
### Create the  Nginx deployment

```
kubectl apply -f deployment.yaml -n nginx-deployment
```
Check the deployment
```
kubectl describe deployment nginx-deployment  -n nginx-deployment
```
The information of the deployment is displayed, the output is similar to that:

```
Name:                   nginx-stateless-deployment
Namespace:              nginx-deployment
CreationTimestamp:      Fri, 04 Nov 2022 15:17:24 +0100
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 3
Selector:               app=nginx
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:latest
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:
      /usr/share/nginx/html/ from nginx-index-file (rw)
  Volumes:
   nginx-index-file:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      configmap
    Optional:  false
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-stateless-deployment-5597b49c9c (3/3 replicas created)
Events:          <none>
```
List the pods created:

```
kubectl get pods -n nginx-deployment
```
The information of the pods created is displayed, the output is similar to that:
```
NAME                                          READY   STATUS    RESTARTS       AGE
nginx-stateless-deployment-5597b49c9c-cwd66   1/1     Running   1 (127m ago)   22h
nginx-stateless-deployment-5597b49c9c-j4nkg   1/1     Running   1 (127m ago)   23h
nginx-stateless-deployment-5597b49c9c-txtjw   1/1     Running   1 (127m ago)   22h
```
### Create the LoadBalancer service

```
kubectl apply -f service.yaml -n nginx-deploy
```
Check the service
```
kubectl get svc -n nginx-deployment
```
```
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
nginx-service   LoadBalancer   10.104.203.136   localhost     80:31788/TCP   25h
```
```
kubectl describe svc nginx-service -n nginx-deployment
```
The information of the service is displayed, the output is similar to that:
```
Name:                     nginx-service
Namespace:                nginx-deployment
Labels:                   <none>
Annotations:              <none>
Selector:                 app=nginx
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.104.203.136
IPs:                      10.104.203.136
LoadBalancer Ingress:     localhost
Port:                     nginx-service  80/TCP
TargetPort:               80/TCP
NodePort:                 nginx-service  31788/TCP
Endpoints:                10.1.1.156:80,10.1.1.160:80,10.1.1.161:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

### Create the ConfigMap

```
kubectl apply -f configmap.yaml -n nginx-deployment
```
```
kubectl describe configmap configmap -n nginx-deployment
```
```
Name:         configmap
Namespace:    nginx-deployment
Labels:       <none>
Annotations:  <none>

Data
====
index.html:
----
<html>
<h1>Hi! This is a Nginx deployment with configmaps!  </h1>
</html
```



