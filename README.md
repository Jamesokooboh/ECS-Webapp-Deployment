# ECS Web Application Deployment

Production-style deployment of a Java web application to AWS using **Amazon ECS on Fargate**, Docker, an Application Load Balancer, and a custom VPC — built to simulate how a real production deployment works end to end.

## Overview

The application is a Java 17 / Spring (Core, Web, WebMVC) web app, built as a WAR with Maven and served via an embedded Jetty container. It's containerized with Docker and deployed to Amazon ECS **Fargate** — no EC2 instances to manage. The surrounding infrastructure includes a custom VPC with public and private subnets, security groups, and an Application Load Balancer.

## Architecture

```
   Client
     |
     v
[Application Load Balancer]  (public subnet)
     |
     v
[ECS Fargate Task: webapp-container]   (port 8080)
     |                                  1 vCPU / 3072 MB
     v
[CloudWatch Logs]  /ecs/webapp-TD
```

**AWS services used:** ECS (Fargate), Application Load Balancer, VPC (public/private subnets, Internet Gateway, route tables), Security Groups, IAM (ECS task execution role), CloudWatch Logs

## Tech stack

- **Java 17**, Spring 5.3.x (spring-core, spring-web, spring-webmvc, spring-context)
- Packaged as a **WAR** via Maven (`maven-compiler-plugin`, `jetty-maven-plugin` for local dev)
- **Docker** for containerization
- **Amazon ECS (Fargate)** for orchestration
- Maven artifacts published to **GitHub Packages** via CI

## Project structure

```
.
├── src/main/                # Java application source
├── .github/workflows/       # CI/CD pipeline
├── Dockerfile
├── pom.xml                  # Maven build config (Java 17, Spring 5.3.x)
├── settings.xml             # Maven server auth for GitHub Packages (CI only)
├── task-definition.json     # ECS Fargate task definition
└── README.md
```

## Local development

```bash
# Run with the Jetty Maven plugin (context path: /maven-web-application)
mvn jetty:run
```

## Docker

```bash
docker build -t webapp .
docker run -p 8080:8080 webapp
```

## ECS task definition

The committed `task-definition.json` defines:

| Setting | Value |
|---|---|
| Family | `webapp-TD` |
| Container | `webapp-container` |
| Port | `8080` |
| Launch type | `FARGATE` |
| CPU / Memory | `1024` (1 vCPU) / `3072` MB (3 GB) |
| Network mode | `awsvpc` |
| Logging | `awslogs` → `/ecs/webapp-TD` (us-east-1) |

## Deployment workflow

1. Create a custom VPC with public and private subnets
2. Configure route tables and an Internet Gateway
3. Create security groups
4. Build the Docker image and push it to a container registry
5. Create the ECS cluster
6. Register the task definition
7. Create the ECS service (Fargate launch type)
8. Configure the Application Load Balancer and target group
9. Verify the deployment in the browser

## Security notes

A few things worth tightening before treating this as a template for real workloads:

- **`task-definition.json` has a real AWS account ID hardcoded** into the IAM role ARNs. For a public repo, parameterize this (e.g. via CI variable substitution) rather than committing the literal account ID.
- The container image reference (`jamesokooboh/test:latest`) looks like a placeholder from testing — worth pointing at a proper ECR repository and a real, versioned tag before calling this production-style end to end.
- `settings.xml` pulls Maven Packages credentials from `GITHUB_TOKEN` and `GPG_PASSPHRASE` environment variables — correct pattern, just make sure those are only ever injected via GitHub Actions secrets, never hardcoded.

## Challenges faced

- Troubleshooting ECS task failures
- Fixing security group and networking issues
- Configuring the Application Load Balancer correctly
- Understanding ECS task definitions and IAM permissions
- Debugging why services weren't reachable publicly
- Managing Docker image versions

## Lessons learned

- How ECS services communicate within a VPC
- How load balancers route traffic to containers
- How security groups affect application accessibility
- How to debug ECS deployment failures
- How to structure a production-style AWS deployment
- The importance of automation and infrastructure planning

## Future improvements

- [ ] Add Terraform for Infrastructure as Code
- [ ] Add GitHub Actions for full CI/CD (build → push → deploy)
- [ ] Add Auto Scaling policies
- [ ] Add CloudWatch monitoring dashboards
- [ ] Add a blue/green deployment strategy
- [ ] Move the container image to ECR with proper tagging

## Author

**James Okooboh**
GitHub: [Jamesokooboh](https://github.com/Jamesokooboh)
Project repository: [ECS-Webapp-Deployment](https://github.com/Jamesokooboh/ECS-Webapp-Deployment)
