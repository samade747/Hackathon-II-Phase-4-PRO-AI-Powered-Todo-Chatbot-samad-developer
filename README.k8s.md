# Kubernetes Deployment Guide for AI-Powered Todo Chatbot

This guide provides instructions for deploying the AI-Powered Todo Chatbot application on a Kubernetes cluster using Helm charts.

## Prerequisites

Before deploying the application, ensure you have the following tools installed:

- Docker
- kubectl
- Helm 3
- Minikube (for local deployment) or access to a Kubernetes cluster

## Local Deployment with Minikube

### 1. Starting Minikube

```bash
# Start Minikube with sufficient resources
minikube start --cpus=4 --memory=8192 --disk-size=20g

# Enable required addons
minikube addons enable ingress
minikube addons enable metrics-server
```

### 2. Building Docker Images

First, build the Docker images for both frontend and backend:

```bash
# Build frontend image
cd frontend
docker build -t todo-chatbot-frontend:latest .
cd ..

# Build backend image
cd backend
docker build -t todo-chatbot-backend:latest .
cd ..
```

For Minikube, load the images directly into the Minikube cluster:

```bash
# Load images into Minikube
minikube image load todo-chatbot-frontend:latest
minikube image load todo-chatbot-backend:latest
```

### 3. Deploying with Helm

Deploy the application using the provided Helm chart:

```bash
# Deploy the application
helm upgrade --install todo-chatbot ./charts/todo-chatbot \
    --namespace todo-chatbot \
    --create-namespace \
    --set global.namespace=todo-chatbot \
    --set frontend.image.repository=todo-chatbot-frontend \
    --set frontend.image.tag=latest \
    --set backend.image.repository=todo-chatbot-backend \
    --set backend.image.tag=latest \
    --set ingress.hosts[0].host=todo-chatbot.local \
    --set ingress.hosts[0].paths[0].path='/' \
    --set ingress.hosts[0].paths[0].pathType=Prefix
```

### 4. Accessing the Application

To access the application locally:

1. Add the Minikube IP to your hosts file:
   ```bash
   echo "$(minikube ip) todo-chatbot.local" | sudo tee -a /etc/hosts
   ```

2. Access the application at:
   - Frontend: http://todo-chatbot.local
   - Backend API: http://todo-chatbot.local/api
   - Health check: http://todo-chatbot.local/health

## Production Deployment

For production deployment, you'll need to:

1. Push your Docker images to a container registry
2. Update the image repository and tag in the values file
3. Configure secrets for your environment
4. Set up SSL certificates for ingress

### Using Custom Values File

Create a custom values file for your environment:

```yaml
# production-values.yaml
global:
  namespace: todo-chatbot-prod

frontend:
  replicaCount: 3
  image:
    repository: your-registry/todo-chatbot-frontend
    tag: v1.0.0

backend:
  replicaCount: 3
  image:
    repository: your-registry/todo-chatbot-backend
    tag: v1.0.0

external:
  supabase:
    url: "https://your-project.supabase.co"
    key: "your-anon-key"
    anonKey: "your-anon-key"
  auth:
    jwtSecretKey: "your-jwt-secret"
    betterAuthSecret: "your-better-auth-secret"
  ai:
    openaiApiKey: "your-openai-api-key"
    openrouterApiKey: "your-openrouter-api-key"
    groqApiKey: "your-groq-api-key"
    geminiApiKey: "your-gemini-api-key"
  database:
    url: "your-database-url"

ingress:
  hosts:
    - host: your-domain.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: todo-chatbot-tls
      hosts:
        - your-domain.com
```

Deploy with custom values:

```bash
helm upgrade --install todo-chatbot ./charts/todo-chatbot \
    --namespace todo-chatbot-prod \
    --create-namespace \
    -f production-values.yaml
```

## Managing the Deployment

### Checking Status

```bash
# Check Helm release status
helm status todo-chatbot --namespace todo-chatbot

# Check Kubernetes resources
kubectl get all -n todo-chatbot
kubectl get ingress -n todo-chatbot
```

### Scaling Applications

```bash
# Scale frontend
kubectl scale deployment todo-chatbot-frontend -n todo-chatbot --replicas=3

# Scale backend
kubectl scale deployment todo-chatbot-backend -n todo-chatbot --replicas=3
```

### Viewing Logs

```bash
# View frontend logs
kubectl logs -l app=todo-chatbot-frontend -n todo-chatbot

# View backend logs
kubectl logs -l app=todo-chatbot-backend -n todo-chatbot
```

### Upgrading the Application

```bash
# Upgrade with new image
helm upgrade todo-chatbot ./charts/todo-chatbot \
    --namespace todo-chatbot \
    --set frontend.image.tag=new-version \
    --set backend.image.tag=new-version

# Upgrade with reuse of previous values
helm upgrade todo-chatbot ./charts/todo-chatbot \
    --namespace todo-chatbot \
    --reuse-values
```

### Rolling Back

```bash
# Rollback to previous version
helm rollback todo-chatbot --namespace todo-chatbot
```

## Using the Deployment Scripts

The project includes convenient scripts for common operations:

### Minikube Setup

```bash
# Run the minikube setup script
./scripts/minikube-setup.sh
```

### General Deployment Operations

```bash
# Deploy the application
./scripts/deploy.sh deploy

# Check status
./scripts/deploy.sh status

# View logs
./scripts/deploy.sh logs frontend
./scripts/deploy.sh logs backend

# Scale deployments
./scripts/deploy.sh scale frontend 3
./scripts/deploy.sh scale backend 2

# Upgrade the application
./scripts/deploy.sh upgrade

# Delete the application
./scripts/deploy.sh delete
```

## Security Considerations

1. **Secrets Management**: Store sensitive information (API keys, database credentials) in Kubernetes Secrets, not in configuration files.

2. **Network Policies**: Implement network policies to restrict traffic between services.

3. **Resource Limits**: Set appropriate resource limits to prevent resource exhaustion.

4. **RBAC**: Implement Role-Based Access Control for proper access management.

5. **Image Security**: Use trusted base images and scan for vulnerabilities.

## Troubleshooting

### Common Issues

1. **Ingress not working**: Ensure the ingress controller is running and the host is correctly configured in your DNS or hosts file.

2. **Application not starting**: Check the logs for error messages and verify that all required environment variables are set.

3. **Image pull errors**: Ensure the image repository and tag are correct and that the image exists in the registry.

### Useful Commands

```bash
# Describe a pod to see detailed information
kubectl describe pod <pod-name> -n todo-chatbot

# Get detailed status of a service
kubectl describe service todo-chatbot-frontend-service -n todo-chatbot

# Check ingress configuration
kubectl describe ingress todo-chatbot-ingress -n todo-chatbot

# Port forward to access services directly
kubectl port-forward -n todo-chatbot svc/todo-chatbot-frontend-service 3000:80
kubectl port-forward -n todo-chatbot svc/todo-chatbot-backend-service 8000:80
```

## Architecture Overview

The deployment consists of:

- **Frontend**: Next.js application serving the user interface
- **Backend**: FastAPI application handling API requests and business logic
- **Ingress**: Routing external traffic to appropriate services
- **ConfigMaps**: Storing non-sensitive configuration
- **Secrets**: Storing sensitive information like API keys
- **HPA**: Auto-scaling deployments based on CPU usage
- **Services**: Internal networking between components
- **Deployments**: Managing application instances