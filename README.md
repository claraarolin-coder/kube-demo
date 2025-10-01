# Demostración de Kubernetes con Minikube

Este repositorio contiene una demostración práctica de Kubernetes utilizando Minikube y Docker Desktop.  
El objetivo es mostrar cómo desplegar una aplicación en contenedores y exponerla mediante un Service.

## Requisitos previos

Antes de comenzar, es necesario tener instalados los siguientes programas:

- Docker Desktop  
- Minikube  
- kubectl  

Se puede verificar la instalación con:

```bash
docker version
minikube version
kubectl version --client

## Pasos de la demostración

Los pasos realizados en la práctica son los siguientes:
Iniciar el clúster de Minikube con Docker como driver: minikube start --driver=docker
Crear dos archivos de configuración: deployment.yaml y service.yaml:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-kube
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-kube
  template:
    metadata:
      labels:
        app: hello-kube
    spec:
      containers:
      - name: hello-kube
        image: nginxdemos/hello:latest
        ports:
        - containerPort: 80

apiVersion: v1
kind: Service
metadata:
  name: hello-kube-service
spec:
  type: NodePort
  selector:
    app: hello-kube
  ports:
    - protocol: TCP
      port: 80
      nodePort: 30001

Aplicar los manifiestos en el clúster con kubectl apply: kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

Verificar que los Pods y Servicios están en ejecución: kubectl get pods
kubectl get svc

Acceder a la aplicación desplegada desde el navegador mediante minikube service: minikube service hello-kube-service


