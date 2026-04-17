ECS Web Application Deployment

Production-style deployment of a containerized web application on AWS using Amazon ECS, Docker, Application Load Balancer, and networking within a custom VPC.


---

Overview

This project demonstrates how to deploy a web application to AWS using a modern cloud-native architecture.

The application is containerized with Docker and deployed to Amazon ECS. The infrastructure includes a custom VPC, public and private subnets, security groups, an Application Load Balancer, and supporting networking configuration.

The goal of this project is to simulate how a real production deployment would work in AWS.


---

Architecture

Main AWS services used:

Amazon ECS

Docker

Amazon EC2 / ECS Cluster

Application Load Balancer

Amazon VPC

Public and Private Subnets

Internet Gateway

Route Tables

Security Groups

IAM Roles

CloudWatch Logs

GitHub



---

Features

Containerized web application using Docker

ECS service for managing application containers

Application Load Balancer for traffic routing

VPC with multiple subnets

Security groups for controlled access

IAM roles for ECS permissions

Logging with CloudWatch

Scalable architecture

Production-style deployment approach



---

Project Structure

.
├── app/
├── Dockerfile
├── ecs-task-definition.json
├── load-balancer-config/
├── networking/
├── screenshots/
├── README.md
└── .github/


---

Deployment Workflow

1. Create a custom VPC


2. Create public and private subnets


3. Configure route tables and internet gateway


4. Create security groups


5. Build Docker image


6. Push image to container registry


7. Create ECS cluster


8. Create task definition


9. Create ECS service


10. Configure Application Load Balancer


11. Test deployment in browser




---

Docker Build

docker build -t webapp .

Run locally:

docker run -p 3000:3000 webapp

---

Challenges Faced

Some of the main challenges during this project included:

Troubleshooting ECS task failures

Fixing security group and networking issues

Configuring the Application Load Balancer correctly

Understanding ECS task definitions and IAM permissions

Debugging why services were not reachable publicly

Managing Docker image versions


These issues helped improve understanding of AWS networking, container orchestration, and deployment troubleshooting.

---

Lessons Learned

Through this project, I learned:

How ECS services communicate within a VPC

How load balancers route traffic to containers

How security groups affect application accessibility

How to debug ECS deployment failures

How to structure a production-style AWS deployment

The importance of automation and infrastructure planning



---

Future Improvements

Possible future enhancements for this project:

Add Terraform for Infrastructure as Code

Add GitHub Actions for CI/CD

Add Auto Scaling policies

Add monitoring dashboards with CloudWatch

Add ECS Fargate deployment 

Add blue/green deployment strategy



---





ECS Cluster

ECS Service

Running Tasks

Load Balancer

VPC Configuration

CloudWatch Logs

Application running in browser





---

Author

James Okooboh

GitHub: https://github.com/Jamesokooboh Project Repository: https://github.com/Jamesokooboh/ECS-Webapp-Deployment 
