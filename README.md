# Symfony Backend Application - DevOps Challenge

## Overview

This project is a simple Symfony-based backend application with a health check endpoint, designed for production-ready deployment with automation and scalability.

---

## Features

- **Health Check Endpoint:** `GET /health` returns HTTP 200 OK.
- **Dockerized Symfony Application:** Runs inside a Docker container.
- **Production Deployment:** Deployed on render using Docker.
- **CI/CD Automation:** GitHub Actions pipeline for testing and deployment.
- **k8s:** Kubernetes Deployment with a Horizontal Pod Autoscaler (HPA) and Health check.

---

## Architecture

- Symfony 6 PHP backend application.
- Docker container for environment consistency.
- render for scalable cloud hosting.
- GitHub Actions for CI/CD automation.
- kustomization for k8s deployment.
---

## Setup & Run Locally

1. Clone the repo:
    ```bash
    git clone https://github.com/sazibsust10/symfony-be-application.git
    cd symfony-be-application
    ```

2. Install dependencies:
    ```bash
    composer install
    ```

3. Run Symfony built-in server:
    ```bash
    php -S 127.0.0.1:8000 -t public
    ```

4. Access the health check:
    ```
    curl http://127.0.0.1:8000/health
    ```

---

## Deployment with render

To deploy this Symfony application using Docker on [Render](https://render.com/), follow these steps:

1. **Login to Render Dashboard**  
   Navigate to [https://dashboard.render.com/](https://dashboard.render.com/) and log in.

2. **Create a New Web Service**
   - Click on **"New" → "Web Service"**
   - Choose **"Deploy from a Git repository"**
   - Connect your GitHub repository containing this project

3. **Configure Deployment Settings**
   - **Environment**: `Docker`
   - **Build Command**: _(leave empty — Dockerfile manages the build)_
   - **Start Command**: _(leave empty — handled by `CMD` in Dockerfile)_
   - **Port**: `8000`

4. **Set Environment Variables**
   Add the required environment variables under the "Environment" section:
   - `APP_ENV=prod`
   - Any additional variables from your `.env` file (e.g., `DATABASE_URL`, `SECRET_KEY`, etc.)

5. **Deploy**
   - Click **Create Web Service**
   - Render will automatically build and deploy your app using the Dockerfile provided

6. **Access the Application**
   - Once deployed, Render will provide a public URL (e.g., `https://symfony-be-application.onrender.com`)
   - Visit `/health` to verify the app is running:
     ```
     https://symfony-be-application.onrender.com/health
     ```
   

---

## CI/CD Pipeline

- The `.github/workflows/deploy.yml` file automates:
  - Code checkout
  - Composer validation
  - Render.com deployment on pushes to the `main` branch.



---

## k8s
   ```bash
   git clone https://github.com/sazibsust10/symfony-be-application.git
   cd symfony-be-application

   make build 
   docker build -t ghcr.io/sazibsust10/symfony-be:1.0.1 .
   make push 
   docker push ghcr.io/sazibsust10/symfony-be:1.0.1

    # Validate base manifest
    kubectl apply --dry-run=client -k k8s/base

    # Apply per environment
    kubectl apply -k k8s/overlays/dev
    kubectl apply -k k8s/overlays/staging
    kubectl apply -k k8s/overlays/prod

    # Verify
    kubectl get all -n dev
    kubectl get ingress -n dev
  ```
     




---

## Future Enhancements

- Integrate a managed PostgreSQL database to handle persistent data storage reliably.
- Add a Redis caching layer to improve performance and reduce database load.
- Use Symfony Messenger with background workers to handle asynchronous tasks efficiently.
- Set up autoscaling and monitoring to ensure the app can handle varying workloads and remain stable.
- I used plain Kubernetes manifests to keep things simple, but tools like Helm would make templating and environment-specific configs easier.
- I’d add security scanning on container images to catch vulnerabilities early.
- Kubernetes RBAC and service accounts would tighten security around cluster access.
- Automating TLS certificates with cert-manager would improve security.
- Adding observability tools like Prometheus and Grafana would help with monitoring.
- Finally, I’d integrate secrets management for sensitive data like API keys.

---


