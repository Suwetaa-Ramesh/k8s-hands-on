# Kubernetes Microservices Project

This repository contains a Kubernetes-based microservices application with three distinct services:

1. **Users API** (`users-api`): Handles user-related operations.
2. **Auth API** (`auth-api`): Manages authentication and JWT token generation.
3. **Tasks API** (`tasks-api`): Provides task management functionalities.

Each service is containerized using Docker and deployed in a Kubernetes cluster with individual deployments, services, and Persistent Volumes (for the `users-api` service).

## Prerequisites

Before running this project, make sure you have the following tools installed:

1. **Docker** - To build and run containers.
2. **Kubernetes** - To deploy and manage the application in a cluster (Minikube, Docker Desktop, or a cloud provider's Kubernetes service).
3. **Kubectl** - To interact with the Kubernetes cluster.
4. **AWS CLI** (optional) - If you are using AWS for the persistent storage with EFS.

## Running the Project

Follow these steps to deploy the microservices to a Kubernetes cluster:

### 1. Clone the Repository

### 2. Build Docker Images

# Build Users API Docker image

```bash
cd users-api
docker build -t your-dockerhub-username/users-api .
```

# Build Auth API Docker image

```bash
cd ../auth-api
docker build -t your-dockerhub-username/auth-api .
```

# Build Tasks API Docker image

```bash
cd ../tasks-api
docker build -t your-dockerhub-username/tasks-api .
```

# Push images to DockerHub

```bash
docker push your-dockerhub-username/users-api
docker push your-dockerhub-username/auth-api
docker push your-dockerhub-username/tasks-api
```

### 3. Deploy the Application to Kubernetes

```bash
cd ../kubernetes
kubectl apply -f auth-deployment.yaml
kubectl apply -f auth-service.yaml

kubectl apply -f tasks-deployment.yaml
kubectl apply -f tasks-service.yaml

kubectl apply -f users-deployment.yaml
kubectl apply -f users-service.yaml
```

### 4. Set Up Persistent Storage (Optional)

```bash
kubectl apply -f efs-storage.yaml

```

### 5. Access the Services

Once all services are deployed, you can access them through their respective services. If you are using a cloud provider's Kubernetes service or Minikube, you can use kubectl get services to get the external IP or URL.

For example:

```bash
kubectl get services

```

This will display something like:

```bash
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
auth-service    ClusterIP      10.96.0.1        <none>        3000/TCP         1m
tasks-service   LoadBalancer   10.96.0.2        192.168.1.1   80:30000/TCP     1m
users-service   LoadBalancer   10.96.0.3        192.168.1.2   80:30001/TCP     1m

```

## Cleaning Up

```bash
kubectl delete -f auth-deployment.yaml
kubectl delete -f auth-service.yaml

kubectl delete -f tasks-deployment.yaml
kubectl delete -f tasks-service.yaml

kubectl delete -f users-deployment.yaml
kubectl delete -f users-service.yaml

kubectl delete -f efs-storage.yaml  # If EFS is used

```
