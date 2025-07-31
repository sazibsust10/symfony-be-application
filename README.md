# Symfony Backend Application - DevOps Challenge

## Overview

This project is a simple Symfony-based backend application with a health check endpoint, designed for production-ready deployment with automation and scalability.

---

## Features

- **Health Check Endpoint:** `GET /health` returns HTTP 200 OK.
- **Dockerized Symfony Application:** Runs inside a Docker container ( multi-stage).
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

To deploy this Symfony application using Docker on [Render](https://render.com/), we need to follow these steps:

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

##  CI/CD Pipeline: Symfony + GitHub Actions + Render

This project uses GitHub Actions to automate building, testing, and deploying the Symfony backend to [Render](https://render.com) using a deploy hook.

### Workflow Overview

- **Trigger:** Runs automatically on every push to the `main` branch.
- **Environment:** Uses latest Ubuntu runner with PHP 8.4.
- **Key Steps:**
  - ✅ Checkout the source code.
  - ✅ Set up PHP with required extensions.
  - ✅ Install Composer dependencies (optimized for production).
  - ✅ Lint PHP files in the `src` directory to ensure syntax correctness.
  - ✅ Run PHPUnit tests (if configured).
  - ✅ Trigger deployment to Render via secure deploy hook.

###  Quality Checks

- PHP linting ensures syntax correctness.
- PHPUnit (optional) validates application logic.

### Deployment

- A deploy is triggered via a **secure HTTP POST** request to Render using a **Deploy Hook URL** stored in GitHub Secrets:
  ```bash
  curl -X POST "${{ secrets.RENDER_DEPLOY_HOOK_URL }}"
  ```

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
   
  ```
     




---

## Rollback Strategy

To ensure fast recovery in case of faulty deployments or bugs in production, this project includes a simple and effective rollback strategy using Render.

### Manual Rollback via Render Dashboard

- Navigate to your service on the [Render Dashboard](https://dashboard.render.com/).
- Go to the **Deploys** tab to view recent deployments.
- Locate a previously successful deployment.
- Click the **“Rollback”** button to instantly redeploy that version.

**Benefits:**
- No code changes required.
- Restores last known-good Docker image and codebase.
- Keeps environment variables and config unchanged.

---

###  Rollback Using Git Revert

If the issue is code-related:

```bash
git revert <commit_hash>
git push origin main
```

---

## Future Enhancements

- Integrate a managed PostgreSQL database to handle persistent data storage reliably.
- Add a Redis caching layer to improve performance and reduce database load.
- Set up autoscaling and monitoring to ensure the app can handle varying workloads and remain stable.
- I used plain Kubernetes manifests to keep things simple, but tools like Helm would make templating and environment-specific configs easier.
- I’d add security scanning on container images to catch vulnerabilities early.
- Kubernetes RBAC and service accounts would tighten security around cluster access.
- Automating TLS certificates with cert-manager would improve security.
- Adding observability tools like Prometheus and Grafana would help with monitoring.
- Finally, I’d integrate secrets management for sensitive data like API keys.

---


