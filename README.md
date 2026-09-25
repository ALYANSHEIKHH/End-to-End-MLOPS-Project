# 🚗 End-to-End MLOps Vehicle Classification

> **An end-to-end production-oriented Machine Learning system that demonstrates the complete ML lifecycle — from data ingestion and validation to model training, evaluation, cloud model registry, containerization, and automated CI/CD deployment.**

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)
![FastAPI](https://img.shields.io/badge/FastAPI-API-green?logo=fastapi)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue?logo=docker)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazonaws)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-green?logo=mongodb)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-black?logo=githubactions)
![MLflow](https://img.shields.io/badge/MLOps-Production%20Pipeline-blueviolet)

---

## 📌 Overview

This project implements a complete **Machine Learning Operations (MLOps) pipeline** for vehicle classification.

Instead of building only a machine learning model, the project focuses on how an ML system can be organized, validated, trained, evaluated, versioned, containerized, and deployed in a cloud environment.

### The complete workflow

```text
                    ┌──────────────────────┐
                    │      Raw Dataset     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    MongoDB Atlas     │
                    │   Data Repository    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Data Ingestion     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Data Validation    │
                    │ Schema + Statistics │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Data Transformation  │
                    │ Cleaning + Features  │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    Model Training    │
                    │   Random Forest      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │  Model Evaluation    │
                    │ Accuracy / F1 / etc. │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    Model Registry    │
                    │       AWS S3         │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   FastAPI Backend    │
                    │ Prediction Pipeline  │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Docker Container   │
                    └──────────┬───────────┘
                               │
                               ▼
              ┌────────────────────────────────┐
              │        GitHub Actions          │
              │          CI / CD               │
              └───────────────┬────────────────┘
                              │
                              ▼
                    ┌──────────────────────┐
                    │      AWS ECR         │
                    │   Docker Registry    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      AWS EC2         │
                    │   Self-Hosted Runner  │
                    └──────────┬───────────┘
                               │
                               ▼
                         🚀 Application
```

---

# 🎯 Project Goals

The project was designed to demonstrate how a machine learning model can move beyond experimentation into a repeatable deployment workflow.

### Key objectives

* Build a modular ML pipeline
* Separate data ingestion, validation, transformation, training, and evaluation
* Store datasets in MongoDB Atlas
* Store trained models in AWS S3
* Serve predictions through FastAPI
* Containerize the application using Docker
* Automate CI/CD with GitHub Actions
* Store Docker images in Amazon ECR
* Deploy the application to an AWS EC2 instance
* Use a self-hosted GitHub Actions runner for deployment
* Manage configuration through environment variables and secrets
* Implement centralized logging and exception handling

---

# 🧠 Machine Learning Pipeline

## 1. Data Ingestion

The ingestion component connects to **MongoDB Atlas**, retrieves the dataset, converts the document-based data into a Pandas DataFrame, and creates the required artifacts for the next pipeline stage.

```text
MongoDB Atlas
      ↓
MongoDB Connection
      ↓
Data Access Layer
      ↓
Pandas DataFrame
      ↓
Data Ingestion Artifact
```

---

## 2. Data Validation

The validation stage verifies whether the incoming dataset follows the expected schema.

The project uses a schema configuration file to define expected:

* Columns
* Data types
* Numerical features
* Target information
* Dataset structure

This helps prevent unexpected data from silently entering the training pipeline.

---

## 3. Data Transformation

The transformation stage prepares raw data for machine learning.

Typical operations include:

* Missing-value handling
* Feature preprocessing
* Scaling
* Feature engineering
* Train/test preparation
* Handling class imbalance

The transformation object is saved so that the **same preprocessing logic can be reused during prediction**.

---

## 4. Model Training

The project uses a **Random Forest Classifier** for model training.

The training component:

```text
Transformed Dataset
        ↓
Random Forest
        ↓
Trained Model
        ↓
Model Artifact
```

The trained model is evaluated using multiple classification metrics rather than relying on a single metric.

---

# 📊 Model Evaluation

The model evaluation stage measures the trained model using:

* Accuracy
* Precision
* Recall
* F1 Score

A configurable evaluation threshold is used to determine whether a newly trained model should be accepted for deployment.

```text
New Model
    │
    ▼
Evaluation
    │
    ├── Accuracy
    ├── Precision
    ├── Recall
    └── F1 Score
    │
    ▼
Compare Against Previous Model
    │
    ▼
Accept / Reject
```

---

# ☁️ Cloud Infrastructure

The project integrates several AWS services into the ML lifecycle.

### Amazon S3

Used as cloud storage for trained model artifacts.

```text
Training Pipeline
       ↓
Model Artifact
       ↓
AWS S3
       ↓
Model Registry
```

### Amazon ECR

Docker images are pushed to an Amazon Elastic Container Registry repository.

```text
Application
    ↓
Docker Build
    ↓
Docker Image
    ↓
Amazon ECR
```

### Amazon EC2

The application is deployed to an Ubuntu-based EC2 instance where Docker runs the production container.

```text
Amazon ECR
     ↓
Docker Pull
     ↓
AWS EC2
     ↓
Docker Container
     ↓
FastAPI Application
```

---

# 🔄 CI/CD Pipeline

The project uses **GitHub Actions** to automate the deployment workflow.

Whenever changes are pushed to the `main` branch:

```text
Git Push
   ↓
GitHub Actions
   ↓
Checkout Repository
   ↓
Configure AWS Credentials
   ↓
Login to Amazon ECR
   ↓
Build Docker Image
   ↓
Push Image to ECR
   ↓
Self-Hosted Runner
   ↓
Run Container on EC2
```

### CI responsibilities

* Checkout source code
* Configure AWS credentials
* Authenticate with Amazon ECR
* Build Docker image
* Push image to ECR

### CD responsibilities

* Run deployment job on the EC2 self-hosted runner
* Authenticate with ECR
* Pull/use the latest Docker image
* Start the application container

---

# 🐳 Docker

The application is packaged as a Docker image to provide a consistent runtime environment.

Benefits:

* Reproducible deployments
* Environment consistency
* Easier dependency management
* Cloud-ready deployment
* Simplified application distribution

Example:

```bash
docker build -t vehicleproj .
```

The image is then tagged and pushed to Amazon ECR.

---

# 🏗️ Project Architecture

```text
src/
│
├── components/
│   ├── data_ingestion.py
│   ├── data_validation.py
│   ├── data_transformation.py
│   ├── model_trainer.py
│   ├── model_evaluation.py
│   └── model_pusher.py
│
├── configuration/
│   ├── mongo_db_connection.py
│   └── aws_connection.py
│
├── data_access/
│   └── proj1_data.py
│
├── entity/
│   ├── config_entity.py
│   ├── artifact_entity.py
│   ├── estimator.py
│   └── s3_estimator.py
│
├── cloud_storage/
│   └── aws_storage.py
│
├── utils/
│   └── main_utils.py
│
└── pipeline/
    ├── training_pipeline.py
    └── prediction_pipeline.py

├── app.py
├── Dockerfile
├── .dockerignore
├── requirements.txt
├── setup.py
├── pyproject.toml
├── template.py
└── .github/
    └── workflows/
        └── aws.yaml
```

---

# 🛠️ Technology Stack

| Category           | Technology                   |
| ------------------ | ---------------------------- |
| Language           | Python 3.10                  |
| ML                 | Scikit-learn                 |
| Data Processing    | Pandas, NumPy                |
| Database           | MongoDB Atlas                |
| API                | FastAPI                      |
| Cloud Storage      | AWS S3                       |
| Containerization   | Docker                       |
| Container Registry | Amazon ECR                   |
| Compute            | Amazon EC2                   |
| CI/CD              | GitHub Actions               |
| Version Control    | Git & GitHub                 |
| Environment        | Conda / Virtual Environment  |
| Configuration      | YAML / Environment Variables |
| Logging            | Python Logging               |
| Exception Handling | Custom Exception System      |

---

# 📁 Configuration & Environment Variables

Sensitive credentials are **not hardcoded into the source code**.

The application uses environment variables for configuration.

Example:

```bash
MONGODB_URL="your-mongodb-connection-string"
AWS_ACCESS_KEY_ID="your-access-key"
AWS_SECRET_ACCESS_KEY="your-secret-key"
AWS_DEFAULT_REGION="us-east-1"
```

For GitHub Actions, sensitive deployment credentials are stored using **GitHub Repository Secrets**.

Required secrets include:

```text
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_DEFAULT_REGION
ECR_REPO
MONGODB_URL
```

> ⚠️ Never commit AWS credentials, MongoDB passwords, `.env` files, private keys, or other secrets to Git.

---

# 🚀 Getting Started

## 1. Clone the repository

```bash
git clone <your-repository-url>
cd End-to-End-MLOPS-Project
```

## 2. Create the environment

```bash
conda create -n vehicle python=3.10 -y
conda activate vehicle
```

## 3. Install dependencies

```bash
pip install -r requirements.txt
```

Install the local package:

```bash
pip install -e .
```

Verify the environment:

```bash
pip list
```

---

# 🍃 MongoDB Atlas Setup

Create a MongoDB Atlas deployment and obtain the connection string.

Set the environment variable:

### PowerShell

```powershell
$env:MONGODB_URL="mongodb+srv://<username>:<password>@..."
```

### Linux / macOS

```bash
export MONGODB_URL="mongodb+srv://<username>:<password>@..."
```

Then run the data ingestion/training workflow.

> For production deployments, use restricted network access and least-privilege database credentials rather than exposing MongoDB publicly.

---

# 🧪 Run Locally

Start the FastAPI application:

```bash
python app.py
```

The application runs on:

```text
http://localhost:5000
```

### Training endpoint

The project also exposes a training route that can be used to trigger model training.

```text
/training
```

### Prediction

The prediction pipeline loads the trained model and preprocessing artifacts and uses them to generate predictions.

---

# ☁️ AWS Deployment Setup

## Required AWS resources

Create:

```text
AWS
├── S3 Bucket
│   └── Model artifacts
│
├── ECR Repository
│   └── vehicleproj
│
└── EC2 Instance
    └── Docker + GitHub Self-Hosted Runner
```

Recommended region:

```text
us-east-1
```

---

# 🏃 GitHub Self-Hosted Runner

The deployment workflow uses an EC2 instance as a **GitHub Actions self-hosted runner**.

The runner allows GitHub Actions to execute deployment commands directly on the EC2 environment.

```text
GitHub Actions
      │
      ▼
Self-Hosted Runner
      │
      ▼
AWS EC2
      │
      ▼
Docker
      │
      ▼
Vehicle ML API
```

---

# 🔐 Security Considerations

This project uses environment variables and GitHub Secrets for sensitive configuration.

For a production environment, additional security hardening should be applied:

* Use AWS IAM least-privilege policies
* Avoid `AdministratorAccess` for application users
* Restrict MongoDB network access
* Use HTTPS/TLS
* Rotate credentials regularly
* Avoid exposing database services publicly
* Use AWS IAM roles where possible instead of long-lived access keys
* Store secrets using a dedicated secret-management service
* Restrict EC2 security-group inbound rules
* Keep Docker images and dependencies updated

---

# 📈 Engineering Practices Demonstrated

This project goes beyond model training and demonstrates several software engineering and MLOps concepts:

### Modular architecture

Each ML lifecycle stage is separated into independent components.

### Configuration management

Pipeline settings and schema information are externalized rather than scattered throughout the codebase.

### Artifact management

Data, preprocessing objects, models, and evaluation outputs are treated as pipeline artifacts.

### Reproducibility

The environment and dependencies are explicitly defined.

### Automated deployment

GitHub Actions automates the build and deployment workflow.

### Containerization

Docker packages the application and its runtime dependencies.

### Cloud integration

AWS S3, ECR, and EC2 are integrated into the ML lifecycle.

### Logging & exception handling

Centralized logging and custom exception handling improve debugging and observability.

---

# 🧩 MLOps Lifecycle

The overall architecture follows:

```text
        ┌──────────────┐
        │     Data     │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │   Validate   │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │ Transform    │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │    Train     │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │   Evaluate   │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │ Model Store  │
        │    AWS S3    │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │ Docker Build │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │   AWS ECR    │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │    EC2       │
        └──────┬───────┘
               ↓
        ┌──────────────┐
        │  Prediction  │
        │     API      │
        └──────────────┘
```

---

# 💡 Why This Project Matters

A machine learning model is only one part of a real ML system.

This project demonstrates the transition from:

```text
"Train a model in a notebook"
```

to:

```text
"Build → Validate → Train → Evaluate → Package → Deploy → Serve"
```

The focus is on building a **repeatable and maintainable ML workflow** rather than treating model training as an isolated experiment.

---

# 🔮 Future Improvements

Potential improvements include:

* MLflow experiment tracking
* Model versioning
* Automated model monitoring
* Data drift detection
* Model performance monitoring
* AWS IAM role-based authentication
* HTTPS with a domain and reverse proxy
* Automated testing in CI
* Infrastructure as Code using Terraform
* Kubernetes deployment
* Automated rollback strategy
* CloudWatch monitoring and logging
* Feature store integration

---

# 👨‍💻 Author

**Muhammad Alyan**

Aspiring **Agentic AI & MLOps Developer** focused on building production-oriented AI and machine learning systems.

### Areas of interest

* 🤖 Agentic AI
* 🧠 Machine Learning
* ⚙️ MLOps
* ☁️ Cloud Computing
* 🐳 Docker
* 🚀 CI/CD
* 🔌 API Development
* 🏗️ Scalable Software Systems

---

## ⭐ If you found this project useful

Feel free to explore the implementation, open an issue, or connect with me.

**Built to learn. Built to deploy. Built with production in mind.**
