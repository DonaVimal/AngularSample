```mermaid
flowchart TD
    %% === STYLE DEFINITIONS ===
    classDef start fill:#fdf5e6,stroke:#333,stroke-width:1px,color:#000,font-weight:bold;
    classDef action fill:#d6eaf8,stroke:#2980b9,stroke-width:1px,color:#000;
    classDef decision fill:#f9e79f,stroke:#b7950b,stroke-width:1px,color:#000;
    classDef done fill:#d5f5e3,stroke:#27ae60,stroke-width:1px,color:#000,font-weight:bold;

    %% === MAIN FLOW ===
    A([Review and Deploy Test environment]):::start -->|PR review| B[Produce InfraCost estimate and post to PR comment]:::action
    B --> C[Run Checkov and KICS static code analysis and post to PR comment]:::action
    C --> D[Run Terraform format, init, validate, plan and post to PR comment]:::action
    A -->|PR merge| E[Run Terraform format, init, validate, plan and apply]:::action

    %% === ECS Check and Build Flow ===
    D --> F{Check Amazon ECS Image exists?}:::decision
    F -->|Image does not exist| G[Update program.cs]:::action
    F -->|Image exists| H{Check for changes in ./appsrc directory}:::decision

    H -->|Changes detected| G
    H -->|No changes detected| Z[Print Web App URL in summary]:::action

    G --> I[Setup .NET environment]:::action
    I --> J[Build .NET environment]:::action
    J --> K[Login to Amazon ECR]:::action
    K --> L[Build, tag and push Docker Image to Amazon ECR]:::action
    L --> Z
    Z --> M([END]):::done

