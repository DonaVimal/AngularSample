```mermaid
graph TD
    %% Nodes
    DEV[Developer]
    GH[GitHub - Code + IaC + Dockerfile]
    GHA[GitHub Actions - CI/CD + SAST + OPA + Build & Push Docker]
    ECR[Amazon ECR - Container Registry]
    TF[Terraform - Provision Infra: EC2, IAM, S3]
    VSM[Vault / Secrets Manager - Inject Secrets]
    EC2[EC2 Instance - Host]
    DOCK[Docker - Container Runtime]
    APP[.NET 8 Application Container]
    CW[CloudWatch Agent - Logs & Metrics]
    GD[GuardDuty - Threat Detection]
    PR[Prometheus Exporter - Metrics Collection]
    GR[Grafana - Dashboards]
    CT[CloudTrail - API Activity Logs]
    SIEM[SIEM - Audit & Compliance]

    %% Flow
    DEV -->|Push Code & Dockerfile| GH
    GH -->|Trigger Workflow| GHA
    GHA -->|Build & Push Image| ECR
    GHA -->|Apply Infrastructure| TF
    TF -->|Provision Host| EC2
    ECR -->|Pull Image| DOCK
    VSM -->|Inject Secrets| DOCK
    DOCK -->|Run Application| APP

    %% Monitoring
    APP --> CW
    EC2 --> CW
    EC2 --> GD
    EC2 --> PR

    %% Visualization
    CW --> GR
    GD --> GR
    PR --> GR

    %% Audit & Compliance
    TF --> CT
    EC2 --> CT
    CT --> SIEM
