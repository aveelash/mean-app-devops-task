MEAN App CI/CD Deployment

This repository contains a full-stack MEAN (MongoDB, Express, Angular, Node.js) application with Dockerized backend and frontend, deployed on AWS EC2 using a CI/CD pipeline via GitHub Actions. The frontend is served with Nginx, and the backend is exposed via a REST API.

Repository Structure
crud-dd-task-mean-app/
├─ backend/ # Node.js + Express backend
│ ├─ Dockerfile
│ ├─ package.json
│ └─ ...other backend files
├─ frontend/ # Angular frontend
│ ├─ Dockerfile
│ ├─ nginx.conf (if custom)
│ ├─ src/
│ │ ├─ app/
│ │ ├─ environments/
│ │ │ ├─ environment.ts
│ │ │ └─ environment.prod.ts
│ │ └─ ...
│ └─ package.json
├─ .github/workflows/
│ └─ deploy.yml # GitHub Actions CI/CD workflow
├─ screenshots/ # Screenshots for README
└─ README.md

Features

Backend: Node.js + Express API

Frontend: Angular SPA

Database: MongoDB

Containerization: Docker (backend + frontend)

Frontend Server: Nginx

CI/CD: GitHub Actions automatically builds Docker images and deploys to EC2

Environment Handling: Angular uses separate dev (localhost) and prod (EC2) API URLs

EC2 Deployment: Backend on port 8080, frontend on port 80

Setup & Deployment Instructions

1. Clone the repository
   git clone <your-repo-link>
   cd crud-dd-task-mean-app
2. Backend Docker Build & Run (manual)
   cd backend
   docker build -t mean-backend:latest .
   docker run -d --name mean-backend -p 8080:8080 mean-backend:latest

Check backend API:

http://<EC2_PUBLIC_IP>:8080/api/tutorials 3. Frontend Docker Build & Run (manual)
cd frontend
docker build -t mean-frontend:latest .
docker run -d --name mean-frontend -p 80:80 mean-frontend:latest

Check frontend UI:

http://<EC2_PUBLIC_IP>/ 4. Environment Configuration

Development: frontend/src/environments/environment.ts

export const environment = {
production: false,
apiUrl: 'http://localhost:8080/api'
};

Production: frontend/src/environments/environment.prod.ts

export const environment = {
production: true,
apiUrl: 'http://<EC2_PUBLIC_IP>:8080/api'
};

Angular uses the correct API URL depending on the build configuration.

5. CI/CD Pipeline (GitHub Actions)

The workflow (.github/workflows/deploy.yml) runs on every push to the main branch:

Checkout repository

Login to Docker Hub

Build backend and frontend Docker images

Tag frontend with latest and GitHub SHA

Push images to Docker Hub

SSH into EC2

Stop old containers

Run new containers with the updated images

6. Running CI/CD

Push any change to the main branch

GitHub Actions automatically builds and deploys

Hard refresh browser (Ctrl+Shift+R) to see the latest frontend

7. Nginx Setup

Frontend is served by Nginx inside the Docker container

Dockerfile copies the Angular production build to /usr/share/nginx/html

Optionally, a custom nginx.conf can configure caching, routing, and gzip
