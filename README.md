# SIT737 – Prac 10P: Deployment and Monitoring of Calculator Microservice

## Deployment Steps

### I. GCP Configuration & Artifact Registry
```bash
gcloud auth login
gcloud config set project sit737-25t1-tariq-4b52e3f
gcloud services enable container.googleapis.com
gcloud services enable artifactregistry.googleapis.com
gcloud artifacts repositories create calculator-repo \
  --repository-format=docker \
  --location=australia-southeast1 \
  --description="Docker repo for calculator app"
gcloud auth config
```
### II. Docker Tagging and Pushing
```bash
docker tag calculator-microservice \
  australia-southeast1-docker.pkg.dev/sit737-25t1-tariq-4b52e3f/calculator-repo/calculator-microservice

docker push \
  australia-southeast1-docker.pkg.dev/sit737-25t1-tariq-4b52e3f/calculator-repo/calculator-microservice
```

### III. Cluster Creation and Kubernetes Deployment
```bash
gcloud container clusters create-auto calculator-cluster \
  --region=australia-southeast1

gcloud container clusters get-credentials calculator-cluster \
  --region=australia-southeast1

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

## Tools and Configuration Used
- Google Cloud CLI: For authentication, service enabling, registry setup, and managing clusters.
- Docker: To containerize and tag the microservice image.
- Kubernetes (kubectl): To deploy the app using YAML manifests.
- Logs Explorer (Cloud Logging): For log inspection (e.g., 403 Forbidden, ErrImagePull) during deployment.
- Cloud Monitoring (Stackdriver): For tracking retry metrics and system performance indicators.
