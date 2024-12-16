# LLM Red Teaming Data Flow Diagram

```mermaid
flowchart TD
    %% Define Colors
    classDef dataSources fill:#FFDDC1,stroke:#FF7F50,stroke-width:2px;
    classDef training fill:#FFF3CD,stroke:#FFC107,stroke-width:2px;
    classDef inference fill:#D4EDDA,stroke:#28A745,stroke-width:2px;
    classDef risks fill:#F8D7DA,stroke:#DC3545,stroke-width:2px;
    classDef mitigations fill:#CCE5FF,stroke:#007BFF,stroke-width:2px;

    %% Subgraph for Labels
    subgraph Color Coding Labels
        legend1[**Orange**: Data Sources]:::dataSources
        legend2[**Yellow**: Training Processes]:::training
        legend3[**Green**: Inference Systems and Pipelines]:::inference
        legend4[**Red**: Vulnerable Points and Risks]:::risks
        legend5[**Blue**: Monitoring and Mitigation Strategies]:::mitigations
    end

    %% Data Sources
    A[Training Data Sources]:::dataSources
    A1[Structured Data]:::dataSources --> A
    A2[Unstructured Data]:::dataSources --> A
    A3[Sensitive Data]:::dataSources --> A
    A -->|Data Collection| B[Preprocessed\nTraining Data]:::training
    A -->|Raw Inputs| C[External Data Sources:\nAPIs, Databases]:::dataSources

    %% Training Phase
    B -->|Processed for Training| D[LLM Training\nPipeline]:::training
    C -->|External Knowledge\nRetrieval| D
    D -->|Embedding\nGeneration| E[LLM Model Weights]:::training

    %% Evaluation Phase
    D -->|Red Team Testing| D1[Evaluation Tools:\nAdversarial Testing]:::training
    D1 -->|Validation| E

    %% Deployment Phase
    E -->|Deployed in| F[Inference API:\nChat Interface]:::inference
    E -->|Integrated with| G[RAG Pipelines:\nRetrieval-Augmented Generation]:::inference
    G -->|External Data Sources| C

    %% Vulnerable Points
    B -->|Poisoning Attacks| H[Training Data\nVulnerabilities]:::risks
    D -->|Data Leakage Testing| I[Model Training\nVulnerabilities]:::risks
    G -->|RAG Contextual Drift| J[Pipeline\nExploitation]:::risks
    F -->|Inference Attacks| K[Prompt Injection\nor Jailbreaks]:::risks
    F -->|Output Misuse| L[Data Exfiltration\nvia Outputs]:::risks
    C -->|Data Contamination| M[External Data\nInjection Risks]:::risks

    %% Monitoring Feedback Loop
    subgraph Monitoring and Feedback
        N[Monitoring Tools:\nAnomaly Detection]:::mitigations -->|Real-Time Alerts| F
        N -->|Feedback to Training| D
        O[Content Filters:\nOutput Validation]:::mitigations --> F
        P[Data Validation Pipelines:\nSecuring External Sources]:::mitigations --> C
    end

    %% Class Assignments
    class A,A1,A2,A3,C dataSources;
    class B,D,E,D1 training;
    class F,G inference;
    class H,I,J,K,L,M risks;
    class N,O,P mitigations;
