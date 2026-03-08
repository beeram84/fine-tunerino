# GPT-2 MLOps Pipeline on Azure Kubernetes Service (AKS)

![MLOps](https://img.shields.io/badge/MLOps-End--to--End-blue)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestrated-blue)
![Azure](https://img.shields.io/badge/Azure-AKS-0078D4)
![Python](https://img.shields.io/badge/Python-3.10-green)

## Project Overview

This project demonstrates a **production-style MLOps pipeline** that
automatically:

1.  Processes uploaded PDF documents
2.  Extracts and cleans text
3.  Fine-tunes a GPT-2 language model
4.  Stores model artifacts
5.  Deploys the trained model as a scalable API
6.  Monitors the system using observability tools

All compute workloads run on **Azure Kubernetes Service (AKS)** using
containerized workloads.

------------------------------------------------------------------------

## Architecture

Pipeline workflow:

User Upload PDF\
↓\
Azure Blob Storage\
↓\
Event Trigger (Event Grid / Webhook)\
↓\
Preprocessing Job (Kubernetes Pod)\
↓\
Model Training Job (GPU Pod)\
↓\
Model Artifact Storage\
↓\
Inference API Deployment\
↓\
Monitoring (Prometheus + Grafana + Loki)

------------------------------------------------------------------------

## Key Components

### Document Ingestion

PDF documents are uploaded and stored in **Azure Blob Storage**.

### Data Preprocessing

Kubernetes jobs extract and clean text from PDF files using:

-   pdfplumber
-   PyPDF
-   pandas
-   HuggingFace datasets

### Model Training

Fine-tuning GPT‑2 using:

-   HuggingFace Transformers
-   PyTorch
-   GPU-enabled Kubernetes nodes

Training produces:

-   model weights
-   tokenizer
-   configuration files

### Model Deployment

The trained model is deployed as a **FastAPI inference service** inside
Kubernetes.

Example request:

POST /generate

{ "text": "Artificial intelligence will" }

### Monitoring

Observability stack:

-   Prometheus (metrics)
-   Grafana (dashboards)
-   Loki (logs)

Monitored metrics:

-   CPU usage
-   memory usage
-   request latency
-   error rate

------------------------------------------------------------------------

## Repository Structure

mlops-gpt2-pipeline

data/\
sample_pdfs/

preprocessing/\
preprocess.py

training/\
train_gpt2.py

inference/\
api.py

docker/\
Dockerfile.training\
Dockerfile.inference

k8s/\
training-job.yaml\
inference-deployment.yaml\
service.yaml

monitoring/\
prometheus.yml\
grafana-dashboard.json

SECURITY.md\
README.md\
requirements.txt

------------------------------------------------------------------------

## Deployment

### Clone repository

git clone https://github.com/your-repo/mlops-gpt2-pipeline.git

cd mlops-gpt2-pipeline

### Build Docker images

docker build -t gpt2-training -f docker/Dockerfile.training .

docker build -t gpt2-inference -f docker/Dockerfile.inference .

### Push images

docker tag gpt2-training `<registry>`{=html}/gpt2-training

docker push `<registry>`{=html}/gpt2-training

### Deploy to AKS

kubectl apply -f k8s/training-job.yaml

kubectl apply -f k8s/inference-deployment.yaml

kubectl apply -f k8s/service.yaml

------------------------------------------------------------------------

## Testing the API

curl -X POST http://`<service-ip>`{=html}/generate\
-H "Content-Type: application/json"\
-d '{"text":"Machine learning will"}'

------------------------------------------------------------------------

## CI/CD (Recommended)

Typical pipeline:

Code Commit\
↓\
Build Docker Image\
↓\
Security Scan\
↓\
Push Image to Registry\
↓\
Deploy to AKS

Possible tools:

-   GitHub Actions
-   GitLab CI
-   Azure DevOps

------------------------------------------------------------------------

## Security

Security best practices:

-   container image scanning
-   Kubernetes RBAC
-   secrets management
-   dependency vulnerability scanning
-   TLS communication

See **SECURITY.md** for full details.

------------------------------------------------------------------------

## Technologies Used

Machine Learning - HuggingFace Transformers - PyTorch - Python

Infrastructure - Docker - Kubernetes - Azure Kubernetes Service (AKS)

Monitoring - Prometheus - Grafana - Loki

------------------------------------------------------------------------

## License

MIT License

This project is intended for **educational and demonstration purposes**.
