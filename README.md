Digilians - Learning Platform

# Learning Platform Project

A full-stack learning platform built with **React (frontend)**, **FastAPI (backend)**, and **PostgreSQL (database)**.  
The project is containerized with Docker and deployed on **Kubernetes**.

---

## 📦 Project Structure

- **frontend/** → React app served via Nginx
- **backend/** → FastAPI app with SQLAlchemy + Postgres
- **postgres/** → StatefulSet with persistent storage

---

## 🚀 Features

- Frontend React SPA with routing
- Backend FastAPI REST API
  - CRUD for courses (`/api/courses`)
- PostgreSQL database with persistent volume
- Kubernetes manifests for deployment

---

## 🛠️ Setup

### 1. Build Docker Images
```bash
# Frontend
docker build -t mokhattab/project:frontend ./frontend

# Backend
docker build -t mokhattab/project:backend ./backend


Push images to your registry if needed:
docker push mokhattab/project:frontend
docker push mokhattab/project:backend



2. Apply Kubernetes Manifests
kubectl apply -f k8s/postgres.yaml
kubectl apply -f k8s/backend.yaml
kubectl apply -f k8s/frontend.yaml



3. Verify Pods & Services
kubectl get pods
kubectl get svc


Expected services:
- frontend-service → NodePort (default 3000)
- backend-service → ClusterIP (port 8000)
- postgres → ClusterIP (port 5432)

🔎 Testing
Backend
Port-forward the backend service:
kubectl port-forward svc/backend-service 8000:8000


Test API:
curl http://localhost:8000/
# {"message": "Learning Platform API"}

curl http://localhost:8000/api/courses
# []


Frontend
Port-forward the frontend service:
kubectl port-forward svc/frontend-service 3000:3000


Open in browser:
http://localhost:3000


The frontend will call the backend via /api/....
Database
Connect to Postgres pod:
kubectl exec -it postgres-0 -- psql -U admin -d learning_platform


Check tables:
\dt
SELECT * FROM courses;



⚙️ Environment Variables
Backend expects:
- DB_USER
- DB_PASSWORD
- DB_HOST
- DB_PORT
- DB_NAME
These are provided via Secrets and ConfigMaps in Kubernetes.

📂 Kubernetes Resources
- Secrets → db-secret (DB credentials)
- ConfigMap → backend-config (DB host/port)
- StatefulSet → postgres
- Deployment → backend-deployment, frontend-deployment
- Services → postgres, backend-service, frontend-service

🧪 Example Workflow
- Add a new course via frontend form or API:
curl -X POST http://localhost:8000/api/courses \
  -H "Content-Type: application/json" \
  -d '{"title":"Kubernetes Basics","description":"Intro course","instructor":"Mohamed"}'
- Verify course appears in frontend UI.
- Check DB:
SELECT * FROM courses;



👨‍💻 Author
- Mohamed Khattab
DevOps Engineer | Mechatronics background | Passionate about Kubernetes, Docker, and reproducible labs.
