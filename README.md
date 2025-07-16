# Tasks Management API

A RESTful API built with the MERN stack (MongoDB, Express.js, React.js, Node.js) for managing tasks. This application is containerized using Docker and can be deployed to Kubernetes.

## Application Overview

### Features
- Create, Read, Update, and Delete tasks
- Task status management (pending, in-progress, completed)
- RESTful API endpoints
- MongoDB database integration
- Docker containerization
- Kubernetes deployment support

### Tech Stack
- Backend: Node.js + Express.js
- Database: MongoDB
- Containerization: Docker
- Orchestration: Kubernetes
- Environment Management: dotenv

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tasks` | Get all tasks |
| GET | `/api/tasks/:id` | Get a specific task |
| POST | `/api/tasks` | Create a new task |
| PUT | `/api/tasks/:id` | Update a task |
| DELETE | `/api/tasks/:id` | Delete a task |

## Local Development Setup

1. **Install Dependencies:**
```bash
npm install
```

2. **Set up Environment Variables:**
Create a `.env` file:
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/tasks-db
```

3. **Run the Application:**
```bash
# Development mode
npm run dev

# Production mode
npm start
```

## Docker Setup

1. **Build Docker Image:**
```bash
# Replace yourusername with your Docker Hub username
docker build -t yourusername/tasks-backend:latest .
```

2. **Run Container Locally:**
```bash
docker run -d -p 5000:5000 \
  -e MONGODB_URI=mongodb://host.docker.internal:27017/tasks-db \
  yourusername/tasks-backend:latest
```

3. **Push to Docker Hub:**
```bash
# Login to Docker Hub
docker login

# Push image
docker push yourusername/tasks-backend:latest
```

## Kubernetes Deployment

### Prerequisites
- Kubernetes cluster (Minikube, Docker Desktop Kubernetes, or cloud provider)
- kubectl CLI tool
- MongoDB instance (local or Atlas)

### Deployment Steps

1. **Create MongoDB URI Secret:**
```bash
# First, encode your MongoDB URI
echo -n "your_mongodb_uri" | base64

# Apply the secret
kubectl apply -f k8s/mongodb-uri-secret.yaml
```

2. **Deploy Backend:**
```bash
# Apply backend deployment and service
kubectl apply -f k8s/tasks-backend-deployment.yaml
```

3. **Verify Deployment:**
```bash
# Check pods status
kubectl get pods

# Check service status
kubectl get services
```

### Accessing the Application

#### Local Kubernetes (Docker Desktop/Minikube)
```bash
# Get service URL (Minikube)
minikube service tasks-backend-service --url

# Or use port-forwarding
kubectl port-forward service/tasks-backend-service 8080:80
# Access via http://localhost:8080
```

#### Cloud Provider
```bash
# Get external IP
kubectl get service tasks-backend-service
# Access via http://<EXTERNAL-IP>
```

## Kubernetes Commands Reference

### Deployment Management
```bash
# Scale deployment
kubectl scale deployment tasks-backend --replicas=3

# Rollout restart
kubectl rollout restart deployment tasks-backend

# Check rollout status
kubectl rollout status deployment/tasks-backend
```

### Monitoring and Debugging
```bash
# Get pods
kubectl get pods

# Get pod logs
kubectl logs -f <pod-name>

# Execute shell in pod
kubectl exec -it <pod-name> -- /bin/sh

# Describe pod
kubectl describe pod <pod-name>
```

### Service Management
```bash
# List services
kubectl get services

# Describe service
kubectl describe service tasks-backend-service

# Delete service
kubectl delete service tasks-backend-service
```

### Clean Up
```bash
# Delete deployments
kubectl delete -f k8s/tasks-backend-deployment.yaml

# Delete secrets
kubectl delete -f k8s/mongodb-uri-secret.yaml
```

## Testing the API

### Using cURL

1. **Create Task:**
```bash
curl -X POST \
  http://your-api-url/api/tasks \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "Test Task",
    "description": "Testing API",
    "status": "pending"
}'
```

2. **Get All Tasks:**
```bash
curl http://your-api-url/api/tasks
```

3. **Update Task:**
```bash
curl -X PUT \
  http://your-api-url/api/tasks/:id \
  -H 'Content-Type: application/json' \
  -d '{
    "status": "completed"
}'
```

4. **Delete Task:**
```bash
curl -X DELETE http://your-api-url/api/tasks/:id
```

## Troubleshooting

1. **Pods not starting:**
- Check pod logs: `kubectl logs <pod-name>`
- Check pod description: `kubectl describe pod <pod-name>`
- Verify MongoDB URI secret is correct

2. **Cannot access service:**
- Verify service is running: `kubectl get services`
- Check endpoints: `kubectl get endpoints tasks-backend-service`
- Try port-forwarding: `kubectl port-forward service/tasks-backend-service 8080:80`

3. **MongoDB connection issues:**
- Verify MongoDB URI secret: `kubectl get secret mongodb-uri-secret -o yaml`
- Check pod environment variables: `kubectl exec -it <pod-name> -- env | grep MONGODB`

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the ISC License. 