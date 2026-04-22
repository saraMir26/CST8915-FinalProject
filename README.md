# Best Buy Cloud-Native Application (CST8915 Final Project)

## Overview
This project is a cloud-native microservices-based application for Best Buy.  
It is deployed on Azure Kubernetes Service (AKS) and uses MongoDB as a stateful database.

The system consists of 5 microservices and demonstrates containerization, Kubernetes deployment, and CI/CD pipelines using GitHub Actions.

---

## Microservices

| Service | Description | Repo |
|--------|------------|------|
| Store-Front | Customer-facing web app | https://github.com/SaraMir26/CST8915-FinalProject-Store-Front |
| Store-Admin | Admin dashboard | https://github.com/SaraMir26/CST8915-FinalProject-StoreAdmin |
| Product-Service | Product API | https://github.com/SaraMir26/CST8915-FinalProject-ProductService |
| Order-Service | Order API (MongoDB) | https://github.com/SaraMir26/CST8915-FinalProject-OrderService |
| Makeline-Service | Order processing worker | https://github.com/SaraMir26/CST8915-FinalProject-MakelineService |

---

## Architecture

The system follows a microservices architecture:

- Store-Front → Product-Service
- Store-Front → Order-Service → MongoDB
- Store-Admin → Makeline-Service → MongoDB
- Product-Service → MongoDB
- MongoDB deployed as a StatefulSet

![Diagram](/Diagram/diagram.png)

---

## Technologies Used

- Azure Kubernetes Service (AKS)
- Docker
- Kubernetes (Deployments, Services, StatefulSets)
- MongoDB
- Node.js (Store-Front, Store-Admin, Order-Service)
- Go (Makeline-Service)
- Rust (Product-Service)
- GitHub Actions (CI/CD)

---

## Kubernetes Deployment

All resources are deployed using:

- namespace.yaml
- config-maps.yaml
- secrets.yaml
- bestbuy-all-in-one.yaml
---

### Deployment steps:


kubectl apply -f namespace.yaml
kubectl apply -f secrets.yaml
kubectl apply -f config-maps.yaml
kubectl apply -f bestbuy-all-in-one.yaml

---

## GitHub Actions Configuration

Each microservice repository uses GitHub Actions for CI/CD.

Before running the workflow, GitHub repository **Secrets** and **Variables** must be configured for each microservice repo.

----

### Common GitHub Secrets
The following secrets must be added to **each** microservice repository:

- `DOCKER_USERNAME` → Docker Hub username
- `DOCKER_PASSWORD` → Docker Hub access token
- `KUBE_CONFIG_DATA` → Base64-encoded kubeconfig for AKS access

----

### Common GitHub Variables
The following variables must be added to each repository:

- `DOCKER_IMAGE_NAME`
- `DEPLOYMENT_NAME`
- `CONTAINER_NAME`
- `K8S_NAMESPACE`

----

### Repository-Specific CI/CD Setup Guides
See the following files for the exact values required for each repo:

- [Store-Front CI/CD Setup](https://github.com/saraMir26/CST8915-FinalProject-StoreFront/blob/main/STORE-FRONT-CIC.md))
- [Store-Admin CI/CD Setup](https://github.com/saraMir26/CST8915-FinalProject-StoreAdmin/blob/main/Store-Admin%20CI/%20STORE-ADMIN-CICD.md)
- [Order-Service CI/CD Setup](./ORDER-SERVICE-CICD.md)
- [Product-Service CI/CD Setup](./PRODUCT-SERVICE-CICD.md)
- [Makeline-Service CI/CD Setup](./MAKELINE-SERVICE-CICD.md)

### Generating `KUBE_CONFIG_DATA`
Run one of the following commands locally after connecting to AKS:

#### Linux/macOS
```bash
cat ~/.kube/config | base64 -w 0
