# Movie Search and Summary System

Welcome to the **Movie Search and Summary System**, a comprehensive solution for searching and summarizing movie data using modern technologies like **FastAPI**, **Apache Airflow**, **Kubernetes**, and **Google Cloud Platform (GCP)** services. This system leverages **OpenAI**, **Pinecone**, and **BigQuery** to provide semantic search capabilities and generate concise summaries of relevant movies.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Components](#components)
  - [1. FastAPI Application](#1-fastapi-application)
  - [2. Apache Airflow DAG](#2-apache-airflow-dag)
  - [3. Deployment Scripts](#3-deployment-scripts)
  - [4. Docker Configuration](#4-docker-configuration)
  - [5. Kubernetes Deployment](#5-kubernetes-deployment)
  - [6. Google Kubernetes Engine (GKE) Setup](#6-google-kubernetes-engine-gke-setup)
  - [7. Monitoring Configuration](#7-monitoring-configuration)
- [Setup and Installation](#setup-and-installation)
  - [Prerequisites](#prerequisites)
  - [Environment Variables](#environment-variables)
- [Running the Application](#running-the-application)
  - [FastAPI Application](#fastapi-application)
  - [Apache Airflow DAG](#apache-airflow-dag)
- [Deployment](#deployment)
  - [Using Docker](#using-docker)
  - [Deploying to Kubernetes](#deploying-to-kubernetes)
  - [Setting Up GKE](#setting-up-gke)
- [Monitoring](#monitoring)
- [Usage](#usage)
  - [API Endpoints](#api-endpoints)
  - [Example Requests](#example-requests)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

---

## Overview

The **Movie Search and Summary System** provides an API that allows users to perform semantic searches on a movie dataset and receive summarized information about relevant movies. The system is designed to handle large datasets efficiently, ensuring scalability and reliability through the use of containerization, orchestration, and cloud services.

## Architecture

![Architecture Diagram](https://example.com/architecture-diagram.png)

1. **FastAPI Application**: Serves as the backend API handling search and summarization requests.
2. **Apache Airflow DAG**: Manages data processing pipelines, including data ingestion, embedding generation, and storage.
3. **Google Cloud Platform Services**:
   - **BigQuery**: Stores movie metadata and descriptions.
   - **Pinecone**: Acts as a vector database for semantic search.
4. **Kubernetes**: Orchestrates containerized services ensuring scalability and high availability.
5. **Monitoring Tools**:
   - **Prometheus**: Collects metrics for monitoring.
   - **Grafana**: Visualizes metrics and sets up dashboards.

## Components

### 1. FastAPI Application

The FastAPI application provides endpoints for searching movies and generating summaries based on user queries. It integrates with OpenAI for embeddings, Pinecone for vector search, and BigQuery for data storage.

- **Key Features**:
  - Semantic search using vector embeddings.
  - Summarization of relevant movie data.
  - Health checks and metrics collection.

- **Main Files**:
  - `main.py`: Core FastAPI application.
  - `requirements.txt`: Python dependencies.

### 2. Apache Airflow DAG

An Airflow DAG automates the data processing workflow, including data ingestion from Google Cloud Storage (GCS), preprocessing, embedding generation, and storage in Pinecone and BigQuery.

- **Key Features**:
  - Parallel and distributed processing with Dask.
  - Robust error handling and retries.
  - Integration with GCP services.

- **Main Files**:
  - `movie_vector_processing_dag.py`: Airflow DAG definition.

### 3. Deployment Scripts

Scripts to automate the deployment of various components, including Airflow DAGs, Docker images, and Kubernetes resources.

- **Main Files**:
  - `deploy_airflow.sh`: Deploys Airflow DAG and dependencies.
  - `deploy_kubernetes.sh`: Deploys Docker images to Kubernetes.

### 4. Docker Configuration

Containerizes the FastAPI application for consistent and scalable deployments.

- **Main Files**:
  - `Dockerfile`: Instructions to build the FastAPI Docker image.
  - `requirements.txt`: Python dependencies for the Docker container.

### 5. Kubernetes Deployment

Kubernetes manifests to deploy the FastAPI application, manage configurations, secrets, and set up monitoring.

- **Main Files**:
  - `configmap.yaml`: Configuration settings.
  - `secret.yaml`: Sensitive data like API keys.
  - `deployment.yaml`: Deployment specifications.
  - `service.yaml`: Service definitions.
  - `fluent-bit-config.yaml`: Log processing configurations.
  - `hpa.yaml`: Horizontal Pod Autoscaler configurations.

### 6. Google Kubernetes Engine (GKE) Setup

A script to set up a GKE cluster tailored for deploying the Movie Search application, ensuring scalability and proper permissions.

- **Main Files**:
  - `setup_gke.sh`: Script to create and configure the GKE cluster.

### 7. Monitoring Configuration

Sets up Prometheus and Grafana for monitoring the application's performance and health within Kubernetes.

- **Main Files**:
  - `prometheus-values.yaml`: Prometheus and Grafana configurations.
  - `service-monitor.yaml`: ServiceMonitor for Prometheus.
  - `prometheus-rules.yaml`: Alerting rules for Prometheus.

## Setup and Installation

### Prerequisites

- **Google Cloud Account**: Ensure you have access to GCP services.
- **Docker**: Installed on your local machine for containerization.
- **Kubernetes Cluster**: Set up via GKE or another provider.
- **Apache Airflow**: For managing DAGs.
- **Git**: For version control.
- **Python 3.9+**: For running FastAPI and Airflow DAGs.

### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# FastAPI Application
PINECONE_API_KEY=your-pinecone-api-key
PINECONE_ENV=your-pinecone-environment
PINECONE_INDEX_NAME=your-pinecone-index-name
OPENAI_API_KEY=your-openai-api-key
GCP_PROJECT_ID=your-gcp-project-id
BQ_DATASET=your-bigquery-dataset

# Airflow Variables
GCS_BUCKET=your-gcs-bucket
INPUT_PATH=your/input/path
```

**Note**: Replace placeholder values with your actual credentials and configurations.

## Running the Application

### FastAPI Application

To run the FastAPI application locally:

1. **Install Dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

2. **Run the Application**:

   ```bash
   uvicorn main:app --host 0.0.0.0 --port 8000
   ```

   The API will be accessible at `http://localhost:8000` until deployed.

### Apache Airflow DAG

To execute the Airflow DAG:

1. **Start Airflow**:

   Ensure your Airflow environment is running. If using Docker Compose:

   ```bash
   docker-compose up -d
   ```

2. **Deploy the DAG**:

   Copy the DAG file to Airflow's DAGs folder:

   ```bash
   ./deploy_airflow.sh
   ```

3. **Trigger the DAG**:

   Access the Airflow UI in cloud composer, locate the `movie_vector_processing` DAG, and trigger it manually or wait for the scheduled run.

## Deployment

### Using Docker

1. **Build the Docker Image**:

   ```bash
   docker build -t gcr.io/your-project-id/movie-search-api:latest .
   ```

2. **Push the Image to Google Container Registry (GCR)**:

   ```bash
   docker push gcr.io/your-project-id/movie-search-api:latest
   ```

3. **Update Kubernetes Deployment**:

   ```bash
   ./deploy_kubernetes.sh
   ```

### Deploying to Kubernetes

1. **Apply Configuration Files**:

   ```bash
   kubectl apply -f config/configmap.yaml
   kubectl apply -f config/secret.yaml
   kubectl apply -f config/deployment.yaml
   kubectl apply -f config/service.yaml
   kubectl apply -f config/fluent-bit-config.yaml
   kubectl apply -f config/hpa.yaml
   ```

2. **Verify Deployment**:

   ```bash
   kubectl get pods -n movie-search
   kubectl get services -n movie-search
   ```

### Setting Up GKE

1. **Run the GKE Setup Script**:

   ```bash
   ./setup_gke.sh
   ```

   This script creates a GKE cluster with autoscaling, sets up namespaces, service accounts, and grants necessary permissions.

2. **Verify Cluster Setup**:

   ```bash
   kubectl get nodes
   kubectl get namespaces
   ```

## Monitoring

The system uses **Prometheus** and **Grafana** for monitoring, with alerts configured for high error rates, latency, and CPU usage.

1. **Deploy Monitoring Stack**:

   Apply the monitoring configuration:

   ```bash
   kubectl apply -f monitoring/prometheus-values.yaml
   kubectl apply -f monitoring/service-monitor.yaml
   kubectl apply -f monitoring/prometheus-rules.yaml
   ```

2. **Access Grafana**:

   Forward the Grafana service port to access the dashboard:

   ```bash
   kubectl port-forward service/grafana 3000:80 -n monitoring
   ```

   Access Grafana at `http://localhost:3000` with the admin credentials specified in `prometheus-values.yaml`.

## Usage

### API Endpoints

- **Search and Summarize**: `POST /search`
- **Health Check**: `GET /health`
- **Metrics**: `GET /metrics`

### Example Requests

1. **Search Request**

   ```bash
   curl -X POST "http://localhost:8000/search" \
   -H "Content-Type: application/json" \
   -d '{
         "query": "Sci-fi adventures",
         "top_k": 5,
         "min_score": 0.7
       }'
   ```

   **Response**:

   ```json
   {
     "query": "Sci-fi adventures",
     "movies": [
       {
         "id": "movie123",
         "full_text": "A thrilling sci-fi adventure...",
         "similarity_score": 0.85
       },
       // More movies
     ],
     "summary": "Based on your query, here are some sci-fi adventure movies that you might enjoy..."
   }
   ```

2. **Health Check**

   ```bash
   curl "http://localhost:8000/health"
   ```

   **Response**:

   ```json
   {
     "status": "healthy",
     "services": {
       "pinecone": "connected",
       "bigquery": "connected",
       "openai": "connected"
     }
   }
   ```

## Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the Repository**
2. **Create a Feature Branch**:

   ```bash
   git checkout -b feature/YourFeature
   ```

3. **Commit Your Changes**:

   ```bash
   git commit -m "Add your feature"
   ```

4. **Push to the Branch**:

   ```bash
   git push origin feature/YourFeature
   ```

5. **Create a Pull Request**

## License

This project is licensed under the [MIT License](LICENSE).

## Contact

For any inquiries or support, please contact [maureesewilliams@gmail.com](mailto:maureesewilliams@gmail.com).

