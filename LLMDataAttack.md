# LLM Red Teaming Data Attack Flow Diagram - Kill Chain

```mermaid
flowchart TD
    %% Style Definitions
    classDef dataSources fill:#FFDDC1,stroke:#FF7F50,stroke-width:2px;
    classDef training fill:#FFF3CD,stroke:#FFC107,stroke-width:2px;
    classDef inference fill:#D4EDDA,stroke:#28A745,stroke-width:2px;
    classDef risks fill:#F8D7DA,stroke:#DC3545,stroke-width:2px;
    classDef mitigations fill:#CCE5FF,stroke:#007BFF,stroke-width:2px;
    classDef killchain fill:#E2E2E2,stroke:#6C757D,stroke-width:1px,font-weight:bold;
    classDef redTeamTools fill:#F1F1F1,stroke:#343A40,stroke-width:1px,stroke-dasharray: 3 3;
    classDef controlCenter fill:#EFEFEF,stroke:#6C757D,stroke-width:1px;
    classDef governance fill:#FFFFFF,stroke:#6C757D,stroke-width:1px,stroke-dasharray: 2 2;

    %% Legend
    subgraph Legend
        legend1[Orange: Data Sources]:::dataSources
        legend2[Yellow: Training Processes]:::training
        legend3[Green: Inference Systems]:::inference
        legend4[Red: Vulnerabilities and Risks]:::risks
        legend5[Blue: Mitigations and Controls]:::mitigations
        legend6[Grey: Attack Steps, Kill Chain, Red Team Tools, Governance]:::killchain
    end

    %% Data Sources
    A[Training Data Sources]:::dataSources
    A1[Structured Data]:::dataSources --> A
    A2[Unstructured Data]:::dataSources --> A
    A3[Sensitive/Internal Data]:::dataSources --> A
    A -->|Data Collection| B[Preprocessed Training Data]:::training
    A -->|Raw Inputs| C[External Data Sources]:::dataSources

    %% Training Pipeline
    B -->|Processed for Training| D[LLM Training Pipeline]:::training
    C -->|Knowledge Retrieval| D
    D -->|Weights & Parameters| E[LLM Model Weights]:::training

    %% Red Team Adversarial Testing
    D -->|Adversarial Testing| D1[Red Team Tools: Backdoor insertion, data poisoning, trojaning]:::redTeamTools
    D1 -->|Validation & Hardening| E

    %% Deployment & Inference
    E -->|Deployed| F[Inference API: Chat Interface]:::inference
    E -->|Integrated| G[RAG Pipelines]:::inference
    G -->|Queries| C

    %% Vulnerabilities
    B -->|Poisoning Attacks| H[Training Data Risks]:::risks
    D -->|Parameter Extraction| I[Model Training Vulnerabilities]:::risks
    G -->|Context Manipulation| J[RAG Pipeline Exploits]:::risks
    F -->|Prompt Injection| K[Inference Attacks]:::risks
    F -->|Sensitive Data Leakage| L[Data Exfiltration]:::risks
    C -->|Malicious Source Injection| M[Supply Chain Risks]:::risks

    %% Mitigations & Controls
    subgraph Defensive_Layer[Defensive and Governance Layers]
        O[Content Filters, RLHF, Toxicity Checks]:::mitigations --> F
        P[Data Validation and Sanitization]:::mitigations --> C
        Q[Adversarial Detection Models]:::mitigations --> F
        R[Secure Proxy Layers]:::mitigations --> F
        S[Compliance and Audit Checks]:::mitigations --> D
        T[Model Hardening: Differential Privacy, Watermarking]:::mitigations --> D
        V[Rollback and Disaster Recovery]:::mitigations --> E
    end

    %% Monitoring & Feedback
    subgraph Monitoring_Feedback[Monitoring, SIEM/SOAR, Feedback]
        N[Monitoring Tools: SIEM, Dashboards]:::mitigations --> F
        N -->|Retraining Updates| B
        U[Human Review: Security Analysts]:::mitigations --> F
        U -->|Policy & Model Updates| D
    end

    %% Red Team Operations
    subgraph Red_Team_Operations_Center[Red Team Operations Center]
        X[Red Team Playbooks and Threat Intel]:::controlCenter
        X -->|Simulation| D1
        X -->|Reports Findings| U
    end

    %% Sandbox Environment
    subgraph Secure_Sandbox[Safe Testing Environment]
        Y[Isolated Sandbox: Test attacks & patches]:::controlCenter
        Y -->|Refine Controls| D
        Y -->|Validate RT Methods| D1
    end

    %% Threat Actors
    subgraph Threat_Actors[Threat Actor Personas]
        TA1[Insider Threats]:::governance
        TA2[Advanced APT Groups]:::governance
        TA3[Script Kiddies]:::governance
    end
    TA1 -.-> B
    TA2 -.-> I
    TA3 -.-> K

    %% Kill Chain
    subgraph Kill_Chain[Lockheed Martin Cyber Kill Chain]
        R1[Reconnaissance]:::killchain
        R2[Weaponization]:::killchain
        R3[Delivery]:::killchain
        R4[Exploitation]:::killchain
        R5[Installation]:::killchain
        R6[Command & Control]:::killchain
        R7[Actions on Objectives]:::killchain
    end

    R1 --> R2 --> R3 --> R4 --> R5 --> R6 --> R7

    A -.-> R1
    B -.-> R2
    C -.-> R3
    K -.-> R4
    H -.-> R5
    F -.-> R6
    L -.-> R7

    %% Extended Red Team Tools
    subgraph Extended_Red_Team_Tools[Red Team Tools & Frameworks]
        RT1[Model Poisoning Kits]:::redTeamTools --> H
        RT2[Adversarial Prompt Suites]:::redTeamTools --> K
        RT3[Parameter Extraction Tools]:::redTeamTools --> I
        RT4[RAG Context Injection Tools]:::redTeamTools --> J
        RT5[Telemetry Tampering Tools]:::redTeamTools
        RT5 -.-> N
    end
