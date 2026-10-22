# EMS Kubernetes Deployment

This project deploys the Employee Management System on Kubernetes using Helm.

Jenkins builds the frontend and backend Docker images, pushes them to Docker Hub, and deploys the application using Helm.

## Tools Used

* Docker 
* Kubernetes (kind)
* Helm
* Jenkins
* MongoDB

## How It Works

1. Jenkins gets the application code from GitHub.
2. It builds the frontend and backend images and are pushed to Docker Hub.
3. Helm updates the Kubernetes deployment.
4. Jenkins checks if the application rollout is successful.

## Current Setup

* Frontend: 2 replicas
* Backend: 2 replicas
* MongoDB: 1 replica with persistent storage
* Helm chart: `ems/`
* Older Kubernetes manifests: `k8s/`

Helm is used for deployment. The older manifests are kept for reference.

## Run Locally

The project currently uses a local kind cluster.

Open the application at:

`http://localhost:30080`

The local setup uses Docker Desktop and Jenkins on Windows. Moving the project to a Linux environment is a planned improvement.

For problems and future improvements,
see [DECISIONS.md](Decisions.md).
