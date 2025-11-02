graph TD

    %% =========================
    %% DEVSECOPS PIPELINE LAYER
    %% =========================
    DEV[👩‍💻 Developer]
    GH(fa:fa-github GitHub Repo)
    GHA(fa:fa-wrench GitHub Actions CI/CD)
    SAST(fa:fa-shield-alt CodeQL / SonarCloud: SAST Scan)
    OPA(fa:fa-balance-scale OPA / Checkov: IaC Policy Check)
    TF(fa:fa-file-code Terraform: Infra as Code)
    ECR(fa:fa-box AWS ECR: Container Registry)

    DEV --> GH: Push Code
    GH --> GHA: Trigger Pipeline
    GHA --> SAST: Static Code Scan
    GHA --> OPA: Policy as Code Validation
    GHA --> TF: Apply Terraform Plan
    GHA --> ECR: Push .NET Container Artifact

    %% =========================
    %% DEPLOYMENT & INFRA LAYER
    %% =========================
    TF --> LAMBDA(fa:fa-cloud AWS Lambda: .NET 8 App)
    TF --> APIGW(fa:fa-plug API Gateway: Public Endpoint)
    TF --> SECRETS(fa:fa-key AWS Secrets Manager)
    TF --> S3(fa:fa-database S3: Config + Logs)
    TF --> CW(fa:fa-eye CloudWatch: Logs & Metrics)
    TF --> CT(fa:fa-history CloudTrail: Auditing)
    TF --> EC2(fa:fa-server EC2: Monitoring + Vault + SIEM)

    %% Lambda connectivity
    APIGW --> LAMBDA
    LAMBDA --> SECRETS: Fetch Secrets Securely
    LAMBDA --> S3: Store Data / Logs
    LAMBDA --> CW: Push Metrics & Logs
    LAMBDA --> CT: Triggered Events
    LAMBDA --> DDB(fa:fa-table DynamoDB: App Data Store)

    %% =========================
    %% SECURITY & OBSERVABILITY LAYER
    %% =========================
    EC2 --> VAULT(fa:fa-lock Vault: Central Secrets)
    EC2 --> PROM(fa:fa-chart-line Prometheus Agent)
    EC2 --> FALCO(fa:fa-radiation Falco: Runtime Threat Detection)
    EC2 --> GRAF(fa:fa-chart-bar Grafana: Dashboards)
    EC2 --> SIEM(fa:fa-brain SIEM / GuardDuty: Threat Intelligence)

    CW --> GRAF
    PROM --> GRAF
    FALCO --> GRAF
    CT --> SIEM

# AngularSample

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 6.0.5.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
"# AngularSample" 
