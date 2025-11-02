```mermaid
graph TD

  %% =========================
  %% DEVSECOPS PIPELINE LAYER
  %% =========================
  DEV[Developer]
  GH[GitHub Repository]
  GHA[GitHub Actions - CI/CD]
  SAST[CodeQL / SonarCloud - Static Code Analysis]
  OPA[OPA / Checkov - Policy as Code]
  TF[Terraform - Infrastructure as Code]
  ECR[AWS ECR - Container Registry]

  DEV --> GH
  GH --> GHA
  GHA --> SAST
  GHA --> OPA
  GHA --> TF
  GHA --> ECR

  %% =========================
  %% DEPLOYMENT & INFRA LAYER
  %% =========================
  TF --> LAMBDA[AWS Lambda (.NET 8 App)]
  TF --> APIGW[API Gateway - Entry Point]
  TF --> SECRETS[AWS Secrets Manager]
  TF --> S3[S3 Bucket - Config & Logs]
  TF --> CW[CloudWatch - Logs & Metrics]
  TF --> CT[CloudTrail - API Audit Logs]
  TF --> EC2[EC2 Instance - Monitoring, Vault, SIEM]
  TF --> DDB[DynamoDB - Data Store]

  APIGW --> LAMBDA
  LAMBDA --> SECRETS
  LAMBDA --> S3
  LAMBDA --> CW
  LAMBDA --> CT
  LAMBDA --> DDB

  %% =========================
  %% SECURITY & OBSERVABILITY LAYER
  %% =========================
  EC2 --> VAULT[Vault - Secret Management]
  EC2 --> PROM[Prometheus Agent - Metrics]
  EC2 --> FALCO[Falco - Threat Detection]
  EC2 --> GRAF[Grafana - Dashboards]
  EC2 --> SIEM[SIEM / GuardDuty - Security Intelligence]

  CW --> GRAF
  PROM --> GRAF
  FALCO --> GRAF
  CT --> SIEM
