# Data Security in LLM Agents and Agentic Applications

```mermaid
flowchart LR
    %% Define Colors
    classDef dataSources fill:#FFDDC1,stroke:#FF7F50,stroke-width:2px;
    classDef agent fill:#FFF3CD,stroke:#FFC107,stroke-width:2px;
    classDef inference fill:#D4EDDA,stroke:#28A745,stroke-width:2px;
    classDef risks fill:#F8D7DA,stroke:#DC3545,stroke-width:2px;
    classDef mitigations fill:#CCE5FF,stroke:#007BFF,stroke-width:2px;

    %% Legend
    subgraph Legend [Color Coding Labels]
        legend1[**Orange**: Data Sources]:::dataSources
        legend2[**Yellow**: Agent Processes]:::agent
        legend3[**Green**: Inference Systems and Pipelines]:::inference
        legend4[**Red**: Vulnerabilities and Risks]:::risks
        legend5[**Blue**: Monitoring and Mitigation Strategies]:::mitigations
    end

    %% Data Sources
    A1[Structured Data: JSON]:::dataSources
    A2[Unstructured Data: Text]:::dataSources
    A3[Sensitive Data: PII]:::dataSources

    subgraph Training Stage
        A[Training Data Sources]:::dataSources
        B[Model Weights]:::inference
        A1 --> A
        A2 --> A
        A3 --> A
        A -->|Processed| B
    end

    %% Agentic Flow
    subgraph Agentic Application Stage
        C[LLM Agents\nReasoning & Decision-Making]:::agent
        D[External Data Sources:\nAPIs & Databases]:::dataSources
        E[Actions via Tools/APIs]:::agent
        F[Model Updates or Fine-Tuning]:::inference

        B -->|Deployed In| C
        C -->|Data Retrieval| D
        C -->|Decision Outputs| E
        E -->|Feedback| F
        D -->|Retrieved Context| C
    end

    %% Vulnerabilities
    subgraph Security Risks
        V1[Data Extraction:\nPrompt Injection]:::risks
        V2[Over-Permissioned Agents:\nTool Misuse]:::risks
        V3[Data Poisoning:\nExternal Data Manipulation]:::risks
        V4[Action Injection:\nUnauthorized Actions]:::risks
        V5[Corrupted Updates:\nData Contamination]:::risks
    end

    B -->|Inference Attacks| V1
    C -->|Agent Permissions| V2
    D -->|Poisoned Input| V3
    E -->|Malicious Outputs| V4
    F -->|Model Update Risks| V5

    %% Mitigations
    subgraph Mitigation Strategies
        M1[Encryption & Access Control]:::mitigations
        M2[Content Filters & Guardrails]:::mitigations
        M3[Tool/API Permissioning]:::mitigations
        M4[Data Validation Pipelines]:::mitigations
        M5[Monitoring Tools:\nAnomaly Detection]:::mitigations
        M6[Logging & Traceability]:::mitigations
    end

    %% Link Risks to Mitigations
    V1 -->|Encrypt & Validate| M1
    V1 -->|Filter Inputs/Outputs| M2
    V2 -->|Enforce Tool Permissions| M3
    V3 -->|Validate External Data| M4
    V4 -->|Trace Actions| M6
    V5 -->|Monitor Updates| M5

    %% Class Assignments
    class A,A1,A2,A3,D dataSources;
    class B,F inference;
    class C,E agent;
    class V1,V2,V3,V4,V5 risks;
    class M1,M2,M3,M4,M5,M6 mitigations;
