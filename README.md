Strapi Deployment on AWS ECS Fargate using Terraform and GitHub Actions

Project Overview

This project demonstrates how to deploy a production-ready Strapi application on AWS ECS Fargate. The entire infrastructure is managed using Terraform and the deployment process is fully automated using GitHub Actions.

The pipeline builds a Docker image, pushes it to Amazon ECR, and updates the ECS Task Definition automatically. No manual AWS Console steps are required after initial setup.

Architecture

The application uses the following AWS services:

Amazon ECS for container orchestration
Amazon ECS Fargate as the serverless compute engine
Amazon Elastic Container Registry for storing Docker images
Application Load Balancer for public access
Amazon CloudWatch for logs
IAM Roles for permissions
VPC and Subnets for networking

When code is pushed to the main branch, GitHub Actions automatically builds and deploys a new version of the application.

Repository Structure

app
Contains the Strapi application and Dockerfile

terraform
Contains all Terraform configuration files
provider.tf
variables.tf
main.tf
outputs.tf

.github/workflows
Contains GitHub Actions workflow file

How Deployment Works

Step 1
Developer pushes code to main branch

Step 2
GitHub Actions workflow starts automatically

Step 3
Docker image is built from the app folder

Step 4
Image is tagged with the Git commit SHA

Step 5
Image is pushed to Amazon Elastic Container Registry

Step 6
Terraform apply runs with the new image tag

Step 7
A new ECS Task Definition revision is created

Step 8
ECS Service updates automatically and deploys the new version

Prerequisites

AWS Account
IAM user with programmatic access
GitHub repository
Docker installed locally for development

Required GitHub Secrets

Go to repository settings and add the following secrets:

AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY

Terraform Configuration

Terraform manages:

ECR repository
ECS cluster
IAM roles
Task definition
Fargate service
Networking configuration

The image tag is dynamically passed from GitHub Actions to Terraform so each deployment creates a new task revision.

How to Access Strapi

After successful deployment:

Go to AWS Console
Navigate to EC2
Open Load Balancers
Copy the DNS name

Access in browser:

http://load-balancer-dns-name

If using public IP configuration instead of ALB:

Go to ECS
Select Cluster
Select Service
Open Running Task
Copy Public IP

Access in browser:

http://public-ip:1337

Security Group Requirements

Inbound rule must allow:

Port 80 or 1337
Source 0.0.0.0/0

Strapi must be configured to run on host 0.0.0.0 to allow external access.

Production Improvements

Store Terraform state in S3
Enable state locking using DynamoDB
Use private subnets
Use RDS for database
Enable auto scaling
Add HTTPS using ACM

What This Project Demonstrates

Infrastructure as Code using Terraform
Containerization using Docker
CI CD using GitHub Actions
Serverless container deployment using ECS Fargate
Automated image versioning and task revision updates

This project represents a production-style DevOps workflow with zero manual deployment steps.
