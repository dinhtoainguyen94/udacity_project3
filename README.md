Coworking Service

  Microservices AWS Kubernetes Project This project sets up a microservices architecture on AWS EKS (Elastic Kubernetes Service), including a PostgreSQL database and a Python analytics service. It provides configurations and deployment files for both cloud and local environments
  
Local Environment:

  - Python 3.6+ with pip for dependency installation.
  
  - Docker CLI for building and running Docker images.
  
  - kubectl to manage Kubernetes clusters.
  
  - helm for applying Helm Charts.
  
Remote Resources:

  - AWS CodeBuild for remote Docker builds.
  
  - AWS ECR for Docker image hosting.
  
  - AWS EKS for Kubernetes cluster management.
  
  - AWS CloudWatch for monitoring.
  
  - GitHub for version control.
  
Setup Steps:

  1. Configure a PostgreSQL Database
 
      - Install PostgreSQL using Helm:
     
         helm repo add <REPO_NAME> https://charts.bitnami.com/bitnami
     
         helm install <SERVICE_NAME> <REPO_NAME>/postgresql
     
      - Retrieve Database Credentials:
     
         export POSTGRES_PASSWORD=$(kubectl get secret --namespace default <SERVICE_NAME>-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)
     
         echo $POSTGRES_PASSWORD
     
      - Connect to the Database:
     
         Via Port Forwarding:
     
            kubectl port-forward --namespace default svc/<SERVICE_NAME>-postgresql 5432:5432 &
     
            PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432
     
      - Run Seed Files to Populate Data:
     
         kubectl port-forward --namespace default svc/<SERVICE_NAME>-postgresql 5432:5432 &
     
         PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432 < <FILE_NAME.sql>
     
  3. Running the Analytics Application Locally
   
     - Navigate to the analytics/ directory.
     
     - Install Python Dependencies:
     
         pip install -r requirements.txt
     
     - Run the Application with Environment Variables:
     
         DB_USERNAME=username DB_PASSWORD=password DB_HOST=127.0.0.1 DB_PORT=5432 DB_NAME=postgres python app.py
     
      - Alternatively, set environment variables using export commands before running the app.
     
  4. Verifying the Application
     
    - Generate Reports Using API Endpoints:
    
      Check-ins grouped by dates : curl <BASE_URL>/api/reports/daily_usage
      Check-ins grouped by users: curl <BASE_URL>/api/reports/user_visits

Project Instructions

  - Set up a Postgres database with a Helm Chart
  
  - Create a Dockerfile for the Python application. Use a base image that is Python-based.
  
  - Write a simple build pipeline with AWS CodeBuild to build and push a Docker image into AWS ECR
  
  - Create a service and deployment using Kubernetes configuration files to deploy the application
  
  - Check AWS CloudWatch for application logs
  
Deliverables

  - Dockerfile
  
  - Screenshot of AWS CodeBuild pipeline
  
  - Screenshot of AWS ECR repository for the application's repository
  
  - Screenshot of kubectl get svc
  
  - Screenshot of kubectl get pods
  
  - Screenshot of kubectl describe svc <DATABASE_SERVICE_NAME>
  
  - Screenshot of kubectl describe deployment <SERVICE_NAME>
  
  - All Kubernetes config files used for deployment (ie YAML files)
  
  - Screenshot of AWS CloudWatch logs for the application
  
README.md file in your solution that serves as documentation for your user to detail how your deployment process works and how the user can deploy changes. The details should not simply rehash what you have done on a step by step basis. Instead, it should help an experienced software developer understand the technologies and tools in the build and deploy process as well as provide them insight into how they would release new builds.
