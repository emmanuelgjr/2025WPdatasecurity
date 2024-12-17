# LLM Red Teaming Data Attack Flow Diagram - Kill Chain

```mermaid
flowchart LR
    %% Style Definitions
    classDef dataSources fill:#FFDDC1,stroke:#FF7F50,stroke-width:2px;
    classDef training fill:#FFF3CD,stroke:#FFC107,stroke-width:2px;
    classDef inference fill:#D4EDDA,stroke:#28A745,stroke-width:2px;
    classDef risks fill:#F8D7DA,stroke:#DC3545,stroke-width:2px;
    classDef mitigations fill:#CCE5FF,stroke:#007BFF,stroke-width:2px;
    classDef killchain fill:#E2E2E2,stroke:#6C757D,stroke-width:1px,font-weight:bold;
    classDef redTeamTools fill:#F1F1F1,stroke:#343A40,stroke-width:1px,stroke-dasharray:3 3;
    classDef controlCenter fill:#EFEFEF,stroke:#6C757D,stroke-width:1px;
    classDef governance fill:#FFFFFF,stroke:#6C757D,stroke-width:1px,stroke-dasharray:2 2;

    %% Legend
    subgraph Legend
    direction LR
        L1[Orange: Data Sources]:::dataSources
        L2[Yellow: Training]:::training
        L3[Green: Inference]:::inference
        L4[Red: Vulnerabilities]:::risks
        L5[Blue: Mitigations]:::mitigations
        L6[Grey: Kill Chain & Tools]:::killchain
    end

    %% Data Sources & Training (Left Side)
    subgraph Data_and_Training
    direction LR
        A[Training Data Sources]:::dataSources
        A1[Structured Data]:::dataSources
        A2[Unstructured Data]:::dataSources
        A3[Sensitive/Internal Data]:::dataSources
        A1-->A
        A2-->A
        A3-->A

        B[Preprocessed Training Data]:::training
        A-->|Collect & Clean|B
        C[External Data Sources]:::dataSources
        C-->B

        D[LLM Training Pipeline]:::training
        B-->D
        C-->|Knowledge Retrieval|D
        E[LLM Model Weights]:::training
        D-->E
    end

    %% Adversarial Evaluation (Red Team)
    subgraph Red_Team_Evaluation
    direction LR
        D1[Red Team Testing: Backdoors, Trojaning]:::redTeamTools
        D-->|Adversarial Testing|D1
        D1-->|Feedback & Hardening|E
    end

    %% Deployment & Inference (Right Side)
    subgraph Deployment_and_Inference
    direction LR
        E-->F[Inference API]:::inference
        E-->G[RAG Pipelines]:::inference
        G-->C
    end

    %% Vulnerabilities Underneath
    subgraph Vulnerabilities
    direction LR
        H[Training Data Poisoning]:::risks
        I[Parameter Extraction]:::risks
        J[RAG Context Manipulation]:::risks
        K[Prompt Injection]:::risks
        L[Data Leakage (Outputs)]:::risks
        M[Supply Chain Attacks]:::risks

        B-->|Poisoning|H
        D-->|Extraction|I
        G-->|Context Exploit|J
        F-->|Jailbreak/Injection|K
        F-->|Sensitive Data Exfil|L
        C-->|Malicious Feeds|M
    end

    %% Mitigations & Defensive Controls
    subgraph Mitigations_and_Controls
    direction LR
        O[Content Filters (RLHF)]:::mitigations
        P[Data Validation]:::mitigations
        Q[Adversarial Detection]:::mitigations
        R[Secure Proxy Layers]:::mitigations
        S[Compliance & Audits]:::mitigations
        T[Model Hardening (Privacy, Watermark)]:::mitigations
        V[Rollback & Recovery]:::mitigations

        O-->F
        P-->C
        Q-->F
        R-->F
        S-->D
        T-->D
        V-->E
    end

    %% Monitoring & Feedback
    subgraph Monitoring_and_Feedback
    direction LR
        N[Monitoring (SIEM, Dashboards)]:::mitigations
        U[Human Review (Security Analysts)]:::mitigations
        N-->F
        N-->|Retrain|B
        U-->F
        U-->|Policy Update|D
    end

    %% Red Team Ops Center & Sandbox
    subgraph Red_Team_Operations
    direction LR
        X[Red Team Playbooks & Intel]:::controlCenter
        Y[Isolated Sandbox]:::controlCenter

        X-->|Simulate Attacks|D1
        X-->|Report Findings|U

        Y-->|Test Controls|D
        Y-->|Validate Methods|D1
    end

    %% Threat Actors & Kill Chain
    subgraph Threat_Actors
    direction LR
        TA1[Insider Threat]:::governance
        TA2[APT Group]:::governance
        TA3[Script Kiddie]:::governance
    end

    TA1-.->H
    TA2-.->I
    TA3-.->K

    subgraph Kill_Chain
    direction LR
        R1[Recon]:::killchain
        R2[Weaponization]:::killchain
        R3[Delivery]:::killchain
        R4[Exploitation]:::killchain
        R5[Installation]:::killchain
        R6[Command & Control]:::killchain
        R7[Actions]:::killchain

        R1-->R2-->R3-->R4-->R5-->R6-->R7

        A-.->R1
        B-.->R2
        C-.->R3
        K-.->R4
        H-.->R5
        F-.->R6
        L-.->R7
    end

    %% Extended Tools
    subgraph Extended_Tools
    direction LR
        RT1[Poisoning Kits]:::redTeamTools-->H
        RT2[Prompt Attack Suites]:::redTeamTools-->K
        RT3[Extraction Tools]:::redTeamTools-->I
        RT4[RAG Injection Tools]:::redTeamTools-->J
        RT5[Telemetry Tampering]:::redTeamTools
        RT5-.->N
    end
