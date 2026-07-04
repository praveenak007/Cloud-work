# Kubernetes Website Deployment Using Minikube (Simple Beginner Notes)

# What is Kubernetes?
**Kubernetes (K8s)** is a tool that helps us **deploy, manage, and scale** containerized applications.

### Simple Definition
Imagine you have many Docker containers.
* **Docker** creates and runs containers.
* **Kubernetes** manages those containers automatically.

# What is Minikube?

**Minikube** is a tool that lets you run a small Kubernetes cluster on your own laptop or PC.

It is mainly used for:

* Learning Kubernetes
* Testing applications
* Practicing projects

# Project Flow

```text
Website
   ↓
Docker Image
   ↓
Minikube
   ↓
Deployment
   ↓
Pod
   ↓
Service
   ↓
Browser

# Prerequisites

Install the following software:

* Docker Desktop
* Minikube
* kubectl

Check if they are installed:

```bash
docker --version
```
```bash
minikube version
```
```bash
kubectl version --client
```

# Step 1: Start Minikube

Start the Kubernetes cluster.

```bash
minikube start
```
Check whether it is running.
```bash
minikube status
```
If everything is working, you will see:

```
Host: Running
Kubelet: Running
APIServer: Running
```

# Step 2: Create a Website

Create a folder called **website**.
Inside it, create:

```
website/
│
├── index.html
└── Dockerfile
```

Example **index.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
</head>
<body>
    <h1>Hello Kubernetes!</h1>
</body>
</html>

# Step 3: Create a Dockerfile

```Dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html

EXPOSE 80

### Explanation

* **FROM** → Uses the Nginx image.
* **COPY** → Copies your website into Nginx.
* **EXPOSE 80** → Opens port 80.

# Step 4: Build Docker Image
```bash
docker build -t mywebsite:v1 .
```
Check the image.

```bash
docker images
```

# Step 5: Load Image into Minikube

```bash
minikube image load mywebsite:v1
```
This makes the Docker image available inside Minikube.

# Step 6: Create Deployment

Create a file called **deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: website-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: website

  template:
    metadata:
      labels:
        app: website

    spec:
      containers:
      - name: website
        image: mywebsite:v1
        ports:
        - containerPort: 80
```

### What is a Deployment?

A **Deployment** tells Kubernetes:

* Which image to run
* How many Pods to create
* How to manage the application

# Step 7: Apply Deployment

```bash
kubectl apply -f deployment.yaml
```
Check the Deployment.
```bash
kubectl get deployments

Check the Pods.

```bash
kubectl get pods

# Step 8: Create Service
Create **service.yaml**
```yaml
apiVersion: v1
kind: Service

metadata:
  name: website-service

spec:
  selector:
    app: website

  ports:
  - port: 80
    targetPort: 80

  type: NodePort
```

### What is a Service?

A **Service** connects users to the Pods.

Without a Service, users cannot access the application.
# Step 9: Apply Service

```bash
kubectl apply -f service.yaml
```

Check the Service.

```bash
kubectl get svc

# Step 10: Open the Website
Run:

```bash
minikube service website-service
```

Your browser will open automatically.

# Common Kubernetes Commands

| Command                                        | Purpose            |
| ---------------------------------------------- | ------------------ |
| `kubectl get pods`                             | View Pods          |
| `kubectl get deployments`                      | View Deployments   |
| `kubectl get svc`                              | View Services      |
| `kubectl logs <pod-name>`                      | View Pod Logs      |
| `kubectl get all`                              | View All Resources |
| `kubectl delete pod <pod-name>`                | Delete a Pod       |
| `kubectl delete deployment website-deployment` | Delete Deployment  |
| `kubectl delete service website-service`       | Delete Service     |

# Important Terms
### Kubernetes
Manages containerized applications.
### Minikube
Runs a local Kubernetes cluster.
### Pod
The smallest unit in Kubernetes. It runs one or more containers.
### Deployment
Creates and manages Pods.
### Service
Allows users to access the application.
### NodePort
Exposes the application outside the cluster using a port.

# Questions

**1. What is Kubernetes?**
A tool used to deploy, manage, and scale containerized applications.

**2. What is Minikube?**
A tool that runs a local Kubernetes cluster for learning and testing.

**3. What is a Pod?**
A Pod is the smallest unit in Kubernetes that runs one or more containers.

**4. What is a Deployment?**
It manages Pods and ensures the required number of Pods are always running.

**5. What is a Service?**
It provides network access to Pods.

**6. What is NodePort?**
A Service type that allows external users to access an application using a port.


# Project Summary
Create Website
      ↓
Create Dockerfile
      ↓
Build Docker Image
      ↓
Start Minikube
      ↓
Load Image into Minikube
      ↓
Create Deployment
      ↓
Create Pods
      ↓
Create Service
      ↓
Open Website in Browser
```
> **Easy way to remember:**
> **Website → Docker Image → Minikube → Deployment → Pod → Service → Browser** 
