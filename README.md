# Best Buy Cloud-Native Application (CST8915 Final Project)

## Overview
This project is a cloud-native microservices-based application for Best Buy.  
It is deployed on Azure Kubernetes Service (AKS) and uses MongoDB as a stateful database.

The system consists of 5 microservices and demonstrates containerization, Kubernetes deployment, and CI/CD pipelines using GitHub Actions.

- **Store-Front** – Customer-facing web application
- **Store-Admin** – Employee/admin web application
- **Order-Service** – Handles order creation and stores orders in MongoDB
- **Product-Service** – Manages product data
- **Makeline-Service** – Processes and updates order status
- **MongoDB** – Stateful database

---

## Repository Structure

This project is organized into separate repositories:

| Service | Description | Repo |
|--------|------------|------|
| Store-Front | Customer-facing web app | https://github.com/SaraMir26/CST8915-FinalProject-Store-Front |
| Store-Admin | Admin dashboard | https://github.com/SaraMir26/CST8915-FinalProject-StoreAdmin |
| Product-Service | Product API | https://github.com/SaraMir26/CST8915-FinalProject-ProductService |
| Order-Service | Order API (MongoDB) | https://github.com/SaraMir26/CST8915-FinalProject-OrderService |
| Makeline-Service | Order processing worker | https://github.com/SaraMir26/CST8915-FinalProject-MakelineService |

---

## Architecture

### Architecture Summary

The application follows a microservices-based architecture deployed on AKS.

Store-Front allows customers to browse products and place orders.   
Store-Admin allows administrators to view and process orders.     
Product-Service provides product data.    
Order-Service stores submitted orders in MongoDB.   
Makeline-Service reads pending orders from MongoDB and updates their processing status.   
MongoDB is deployed as a StatefulSet to provide persistent storage.    


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
- Kubernetes
  - Deployments
  - Services
  - StatefulSets
  - ConfigMaps
  - Secrets
- MongoDB
- Node.js
  - Store-Front
  - Store-Admin
  - Order-Service
- Go
  - Makeline-Service
- Rust
  - Product-Service
- GitHub Actions
  - CI/CD pipelines for each microservice

---

## Docker Hub Images

The following Docker images are used for deployment:

- sara21167/store-front-finalproject
- sara21167/store-admin-finalproject
- sara21167/order-service-finalproject
- sara21167/product-service-finalproject
- sara21167/makeline-service-finalproject

----
## Kubernetes Deployment 

All Kubernetes resources are deployed using the following files in this repository:

- [namespace.yaml](./deployment-files/bestbuy-namespece.md)
- [config-maps.yaml](./deployment-files/config-maps.yaml)
- [secrets.yaml](./deployment-files/secrets.yaml)
- [bestbuy-all-in-one.yaml](./deployment-files/bestbuy-all-in-one.yaml)
---

### Deployment steps:


kubectl apply -f namespace.yaml   
kubectl apply -f secrets.yaml    
kubectl apply -f config-maps.yaml   
kubectl apply -f bestbuy-all-in-one.yaml   

---
## Kubernetes Verification Commands
<pre>
kubectl get pods -n bestbuy
kubectl get svc -n bestbuy
kubectl get pvc -n bestbuy
</pre>
----

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
- [Order-Service CI/CD Setup](https://github.com/saraMir26/CST8915-FinalProject-OrderService/blob/master/ORDER-SERVICE-CICD.md)
- [Product-Service CI/CD Setup](https://github.com/saraMir26/CST8915-FinalProject-ProductService/blob/main/PRODUCT-SERVICE-CICD.md)
- [Makeline-Service CI/CD Setup](https://github.com/saraMir26/CST8915-FinalProject-MakelineService/blob/master/MAKELINE-SERVICE-CICD.md)

### Generating `KUBE_CONFIG_DATA`
Run one of the following commands locally after connecting to AKS:

#### Git Bash on Windows
<pre>
base64 < ~/.kube/config | tr -d '\n'
</pre>

Copy the output and save it as the GitHub secret KUBE_CONFIG_DATA

----
## CI/CD Pipelines

Each microservice repository has its own GitHub Actions pipeline.

Each pipeline performs the following steps:   
- Checks out the repository source code   
- Builds the Docker image  
- Pushes the image to Docker Hub  
- Authenticates to AKS using kubeconfig  
- Updates the Kubernetes deployment image   
- Verifies rollout success  
- Trigger  

Pipelines are configured to run on push and can also be triggered manually through GitHub Actions.

----

## Application Features
#### Store-Front
- View product catalog
- View product details
- Add products to cart
- Submit orders
#### Store-Admin
- View product list
- View submitted orders
- Open individual order details
- Process orders and update order status

----

## Demo Video

 
[![Watch the demo](https://img.youtube.com/vi/ilEZ0AiMpB0/hqdefault.jpg)](https://www.youtube.com/watch?v=ilEZ0AiMpB0)

-----

## Submission Notes

This project was developed using the Algonquin Pet Store architecture as a starting point and adapted to meet the Best Buy final project requirements.

---- 

<sub>
  <br><strong>Acknowledgment</strong><br>
This project was developed by Sara Mirzaei as part of the CST8915 final project.
During development, AI tools (such as ChatGPT) were used as a support resource for:
- troubleshooting errors in Kubernetes and Docker deployments
- refining code and configuration files
- improving documentation and project structure
All implementation, configuration, and final validation were performed by the author.
</sub>
