# CircleCI Project

## Overview
This project is a CI/CD pipeline using CircleCI, Terraform, Ansible, and ArgoCD to deploy a web application and infrastructure on AWS EKS. It integrates GitLab as a container registry and uses automated testing, security scanning, and deployment strategies.

## Project Structure
```
├── ansible                 
│   ├── gitlab.yaml
│   └── inventory.yaml
├── README.md              
├── requirements.txt       
├── src                    
│   ├── app.py             
│   ├── Dockerfile         
│   ├── exceptions.py      
│   ├── static
│   │   └── style.css      
│   ├── templates        
│   │   ├── 404.html
│   │   └── weather.html
│   ├── tests              
│   │   ├── check-connectivity.py
│   │   └── test-selenium.py
│   ├── weather_utility.py  
│   └── wsgi.py             
└── terraform               
    ├── eks-cluster.tf      
    ├── main.tf             
    ├── outputs.tf        
    ├── terraform.tf      
    ├── variables.tf       
    └── vpc.tf              
```

## CI/CD Pipeline
The `.circleci/config.yml` defines a pipeline that includes:
- **Build & Test:** Builds a Docker image, runs a container, and executes a smoke test.
- **Security Scanning:** Runs Checkov for static security checks.
- **Infrastructure Testing:** Uses Terratest to validate the Terraform infrastructure.
- **Deployment:**
  - Creates an EKS cluster with Terraform.
  - Deploys ArgoCD to manage Kubernetes applications.
  - Configures ArgoCD with GitLab repository authentication.

## CircleCI Jobs
- **`build`**: Builds a Docker image, runs a container, performs a smoke test, and pushes the image to GitLab.
- **`checkov`**: Runs Checkov for security scanning on Terraform files.
- **`terratest_and_cluster`**: Initializes Terraform, runs Terratest for infrastructure validation, and destroys resources.
- **`setup-argocd`**: Installs and configures ArgoCD on the EKS cluster and deploys applications.

## Infrastructure
- **Terraform:** Provisions an AWS EKS cluster and VPC.
- **Ansible:** Configures a self-hosted GitLab instance.
- **ArgoCD:** Manages application deployments on Kubernetes.

## Deployment Steps
1. Push code to GitLab.
2. CircleCI runs the pipeline:
   - Builds and tests the application.
   - Scans Terraform code.
   - Deploys the infrastructure.
   - Sets up ArgoCD for Kubernetes deployments.
3. Application is deployed to AWS EKS via ArgoCD.

## Prerequisites
- CircleCI account with required permissions.
- AWS account with IAM roles for EKS.
- GitLab account for Docker registry.
- Terraform and Ansible installed locally for manual execution.

## Usage
To trigger the pipeline, push changes to the GitLab repository. The pipeline will execute automatically based on `.circleci/config.yml`.

## Future Enhancements
- Implement Helm charts for Kubernetes deployments.
- Improve test coverage with additional integration tests.
- Add monitoring with Prometheus and Grafana.

