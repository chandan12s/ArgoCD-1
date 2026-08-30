# Kubernetes Project Deployment

## 1. Problem Statement Overview
This project demonstrates how to deploy a simple web application in a Kubernetes cluster and expose it through a Service. The goal is to run a containerized NGINX application with multiple replicas and make it reachable from outside the cluster using a NodePort.

The project also includes an ArgoCD Application manifest to show how GitOps can manage the deployment from a repository. This helps automate synchronization between the source repository and the target Kubernetes namespace.

## 2. Solution Approach
The solution uses Kubernetes resources to manage the application lifecycle:

- Deployment: Creates 3 replicas of an NGINX container using the `nginx:1.30` image.
- Service: Exposes the application through a `NodePort` service and maps traffic to the application pods.
- ArgoCD Application: Declares the application in GitOps style so ArgoCD can sync it from the repository to the cluster.

This approach provides a scalable and declarative deployment model using Kubernetes manifests.

## 3. Dependencies and Setup Instructions
Before running this project, ensure the following are available:

### Required tools
- Kubernetes cluster (for example, Minikube, Kind, Docker Desktop Kubernetes, or a cloud-managed cluster)
- `kubectl` installed and configured to the cluster
- Optional: ArgoCD installed in the cluster if you plan to use the Application manifest

### Repository files
- `my-deployment.yaml` — deployment definition
- `my-service.yaml` — Kubernetes service definition
- `application.yaml` — ArgoCD application definition

### Setup steps
1. Start or connect to your Kubernetes cluster.
2. Verify your cluster is reachable:
   ```bash
   kubectl cluster-info
   ```
3. Confirm the Kubernetes context is correctly selected:
   ```bash
   kubectl config current-context
   ```
4. If ArgoCD is not installed, install it before applying the `application.yaml` file.

## 4. Execution Steps
Follow these steps to deploy the application.

### Deploy the application manually
Run:
```bash
kubectl apply -f my-deployment.yaml
kubectl apply -f my-service.yaml
```

### Verify the resources
Check that the deployment and service were created successfully:
```bash
kubectl get deployment
kubectl get pods
kubectl get svc
```

### Access the application
Because the service type is `NodePort`, the application is exposed on a port in the range `30000-32767`.

To find the assigned NodePort, use:
```bash
kubectl get svc my-service
```

Then open the application using the node IP or localhost on the mapped NodePort, for example:
```bash
http://<node-ip>:30007
```

### Deploy with ArgoCD (GitOps flow)
If ArgoCD is configured, apply the ArgoCD application:
```bash
kubectl apply -f application.yaml
```

Then, from the ArgoCD UI or CLI, sync the application to deploy resources from the repository to the cluster.

## Notes
- The project is based on a simple web application deployment pattern suitable for learning Kubernetes fundamentals.
- For production workloads, add health checks, resource limits, persistent storage, and secure networking configuration.
