# Symfony Backend Application - DevOps Challenge

## Overview

This project is a simple Symfony-based backend application with a health check endpoint, designed for production-ready deployment with automation and scalability.

---

## Features

- **Health Check Endpoint:** `GET /health` returns HTTP 200 OK.
- **Dockerized Symfony Application:** Runs inside a Docker container.
- **Production Deployment:** Deployed on Fly.io using Docker.
- **CI/CD Automation:** GitHub Actions pipeline for testing and deployment.

---

## Architecture

- Symfony 6 PHP backend application.
- Docker container for environment consistency.
- Fly.io for scalable cloud hosting.
- GitHub Actions for CI/CD automation.

---

## Setup & Run Locally

1. Clone the repo:
    ```bash
    git clone https://github.com/yourusername/yourrepo.git
    cd yourrepo
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

## Deployment with Fly.io

1. Install Fly CLI from [https://fly.io/docs/hands-on/install-flyctl/](https://fly.io/docs/hands-on/install-flyctl/).

2. Login to Fly.io:
    ```bash
    flyctl auth login
    ```

3. Create a new app (skip if already created):
    ```bash
    flyctl apps create your-fly-app-name
    ```

4. Deploy the app:
    ```bash
    flyctl deploy
    ```

5. View the app URL:
    ```bash
    flyctl info
    ```

---

## CI/CD Pipeline

- The `.github/workflows/deploy.yml` file automates:
  - Code checkout
  - Composer validation
  - Fly.io deployment on pushes to the `main` branch.

Make sure to add your Fly.io API token in the repository secrets as `FLY_API_TOKEN`.

---

## Future Enhancements

- Add database integration with managed Postgres.
- Add caching layer using Redis.
- Implement background workers with Symfony Messenger.
- Configure autoscaling and monitoring.

---

## Contact

For questions or support, please open an issue or contact the maintainer.
