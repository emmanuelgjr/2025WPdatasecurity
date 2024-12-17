# LLM Red Teaming Data Flow Diagram

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
        legend4[Red: Vulnerabilities & Risks]:::risks
        legend5[Blue: Mitigations & Controls]:::mitigations
        legend6[Grey: Attack Steps (Kill Chain), Red Team Tools, Governance]:::killchain
    end

    %% Data Sources
    A[Training Data Sources]:::dataSources
    A1[Structured Data]:::dataSources --> A
    A2[Unstructured Data]:::dataSources --> A
    A3[Sensitive/Internal Data]:::dataSources --> A
    A -->|Data Collection| B[Preprocessed Training Data]:::training
    A -->|Raw Inputs| C[External Data Sources]:::dataSources

    %% Training Pipeline
    B -->|Processed for Training| D[LLM Training Pipeline\n(Tokenization, Embeddings, Fine-tuning)]:::training
    C -->|Knowledge Retrieval| D
    D -->|Weights & Parameters| E[LLM Model Weights]:::training

    %% Evaluation (Red Teaming)
    D -->|Adversarial Testing| D1[Red Team Tools & Evaluation:\nBackdoor Insertion, Data Poisoning, Trojaning]:::redTeamTools
    D1 -->|Validation & Hardening| E

    %% Deployment & Inference
    E -->|Deployed| F[Inference API:\nChat Interface, Model Serving]:::inference
    E -->|Integrated| G[RAG Pipelines\n(Retrieval-Augmented Generation)]:::inference
    G -->|Queries| C

    %% Vulnerabilities & Risks
    B -->|Poisoning Attacks| H[Training Data Risks]:::risks
    D -->|Parameter Extraction, Memorization| I[Model Training Vulnerabilities]:::risks
    G -->|Context Manipulation| J[RAG Pipeline Exploits]:::risks
    F -->|Prompt Injection, Jailbreaks| K[Inference Attacks]:::risks
    F -->|Sensitive Data Leakage| L[Data Exfiltration]:::risks
    C -->|Malicious Inputs| M[Supply Chain & External Data Injection]:::risks

    %% Mitigations & Controls
    subgraph Defensive_Layer[Defensive & Governance Layers]
        O[Content Filters & Policy Enforcement:\nRLHF, Toxicity/Bias Filters]:::mitigations --> F
        P[Data Validation & Sanitization:\nSchema Checks, Source Vetting]:::mitigations --> C
        Q[Adversarial Detection Models:\nAnomaly Detection]:::mitigations --> F
        R[Secure Proxy Layers:\nInput Sanitization]:::mitigations --> F
        S[Compliance & Audit Checks:\nGDPR, CCPA]:::mitigations --> D
        T[Model Hardening:\nDifferential Privacy, Watermarking]:::mitigations --> D
        V[Rollback & Recovery:\nVersion Control]:::mitigations --> E
    end

    %% Monitoring & Feedback
    subgraph Monitoring_Feedback[Monitoring, SIEM/SOAR & Feedback]
        N[Monitoring Tools:\nSIEM/SOAR, Metrics Dashboards]:::mitigations --> F
        N -->|Feedback for Retraining| B
        U[Human-in-the-Loop Review:\nSecurity Analysts]:::mitigations --> F
        U -->|Policy & Model Updates| D
    end

    %% Red Team Operations
    subgraph Red_Team_Operations_Center[Red Team Operations Center]
        X[Red Team Playbooks & SOPs:\nMITRE ATLAS, Threat Intel]:::controlCenter
        X -->|Controlled Simulation| D1
        X -->|Findings & Reports| U
    end

    %% Sandbox Environment
    subgraph Secure_Sandbox[Safe Testing & Simulation Environment]
        Y[Isolated Sandbox:\nReplay Attacks, Test Patches]:::controlCenter
        Y -->|Refine Detection & Controls| D
        Y -->|Validate Red Team Methods| D1
    end

    %% Threat Actor Personas
    subgraph Threat_Actors[Threat Actor Personas]
        TA1[Insider Threat:\nCan Poison Training Data]:::governance
        TA2[Advanced APT:\nRAG Exploits, Extraction Attacks]:::governance
        TA3[Script Kiddies:\nSimple Prompt Injection]:::governance
    end

    TA1 -.-> B
    TA2 -.-> I
    TA3 -.-> K

    %% Kill Chain
    subgraph Kill_Chain[Lockheed Martin Cyber Kill Chain]
        R1[Reconnaissance:\nIdentify Data, Pipeline]:::killchain
        R2[Weaponization:\nCraft Poisoned Data, Prompts]:::killchain
        R3[Delivery:\nInject Malicious Data]:::killchain
        R4[Exploitation:\nPrompt Injection, RAG Manipulation]:::killchain
        R5[Installation:\nBackdoor in Model Weights]:::killchain
        R6[Command & Control:\nInteract via Queries]:::killchain
        R7[Actions on Objectives:\nData Exfiltration, Misinformation]:::killchain
    end

    R1 --> R2 --> R3 --> R4 --> R5 --> R6 --> R7

    %% Map Vulnerabilities to Kill Chain
    A -.Used in Recon-.-> R1
    B -.Data Poisoning-> R2
    C -.Malicious Delivery-> R3
    K -.Prompt Injection-> R4
    H -.Model Backdoors-> R5
    F -.C2 via Queries-> R6
    L -.Data Exfiltration-> R7

    %% Red Team Tools Extended
    subgraph Extended_Red_Team_Tools[Red Team Tools & Frameworks]
        RT1[Model Poisoning Kits:\nTrojaning LMs]:::redTeamTools --> H
        RT2[Adversarial Prompt Suites:\nTextFooler, Polyjuice]:::redTeamTools --> K
        RT3[Parameter Extraction Tools:\nEmbedding Extractors]:::redTeamTools --> I
        RT4[RAG Injection Tools:\nFake Doc Injection]:::redTeamTools --> J
        RT5[Telemetry Tampering:\nHide Attack Traces]:::redTeamTools
        RT5 -.-> N
    end
