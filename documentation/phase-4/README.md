# Phase 4: Helm Chart Creation & Templating

## 🎯 Objective
Create a reusable and scalable Helm chart to manage Kubernetes deployments for the Smart Expense Tracker application.

---

## ✅ What Was Accomplished

### Helm Chart Setup
- ✅ Created Helm chart structure using `helm create`
- ✅ Defined `Chart.yaml` with proper metadata
- ✅ Configured `values.yaml` for centralized configuration
- ✅ Created environment-specific overrides (`values-dev.yaml`, `values-prod.yaml`)
- ✅ Validated chart using `helm lint`
- ✅ Rendered templates using `helm template`

---

## 📦 Helm Chart Structure

helm/expense-tracker/
├── Chart.yaml
├── values.yaml
├── values-dev.yaml
├── values-prod.yaml
└── templates/
├── namespace.yaml
├── postgres-secret.yaml
├── django-secret.yaml
├── postgres-pvc.yaml
├── postgres-deployment.yaml
├── postgres-service.yaml
├── backend-deployment.yaml
├── backend-service.yaml
├── frontend-deployment.yaml
├── frontend-service.yaml
└── ingress.yaml

markdown
Copy code

---

## 📄 Templates Implemented

### Core Infrastructure
- **Namespace** – logical isolation for the application  
- **PostgreSQL PVC** – persistent database storage  
- **Secrets** – database password and Django secret key  

### Application Components
- **Backend Deployment (Django)**
  - Gunicorn server
  - Health checks (`/health`)
  - Environment variables via values.yaml

- **Frontend Deployment (React)**
  - Served via NGINX
  - Health checks enabled

- **Services**
  - ClusterIP services for backend, frontend, and database

- **Ingress**
  - Optional ingress configuration
  - Host-based routing
  - Compatible with NGINX ingress controller

---

## ⚙️ Configuration Highlights

### Dynamic Replica Control
```yaml
replicaCount:
  backend: 2
  frontend: 2
  postgres: 1
Environment Variables
yaml
Copy code
django:
  debug: false
  allowedHosts: "*"
Resource Limits
yaml
Copy code
resources:
  backend:
    requests:
      cpu: 250m
      memory: 256Mi
    limits:
      cpu: 500m
      memory: 512Mi
Health Checks
yaml
Copy code
livenessProbe:
  path: /health/
  initialDelaySeconds: 30

readinessProbe:
  path: /health/
  initialDelaySeconds: 10
🌍 Environment-Specific Configurations
Environment	Backend Replicas	Frontend Replicas	Storage	Autoscaling
Dev	1	1	5Gi	Disabled
Prod	3	3	20Gi	Enabled

🧪 Validation & Testing
Helm Validation
bash
Copy code
helm lint .
✅ Passed without errors

Template Rendering
bash
Copy code
helm template expense-tracker .
✅ All Kubernetes manifests generated successfully

Deployment Test
bash
Copy code
helm install expense-tracker . -n expense-tracker --create-namespace
✅ All pods started successfully
✅ Services reachable
✅ Ingress created correctly

🔁 Upgrade & Rollback
bash
Copy code
# Upgrade release
helm upgrade expense-tracker .

# Rollback
helm rollback expense-tracker
📊 Verified Results
✅ Namespace created

✅ Secrets applied correctly

✅ PostgreSQL running with PVC

✅ Backend pods healthy

✅ Frontend pods healthy

✅ Services and ingress working

✅ Helm upgrade and rollback tested

🎯 Outcome
✔ Helm chart fully functional
✔ Environment-based configuration working
✔ Ready for GitOps deployment (Argo CD)
✔ Production-ready structure

🚀 Next Phase
Phase 5 – CI/CD with GitHub Actions & Argo CD

Phase 4 completed successfully.
Chart Version: 1.0.0