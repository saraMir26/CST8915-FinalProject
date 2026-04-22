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
- MongoDB deployed as StatefulSet

// I need to insert the diagram 

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

### Deployment steps:

```bash
kubectl apply -f namespace.yaml
kubectl apply -f secrets.yaml
kubectl apply -f config-maps.yaml
kubectl apply -f bestbuy-all-in-one.yaml
