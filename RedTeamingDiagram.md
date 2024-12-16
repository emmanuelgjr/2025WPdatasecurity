# LLM Red Teaming Data Flow Diagram

```mermaid
flowchart TD
    %% Define Colors
    classDef dataSources fill:#FFDDC1,stroke:#FF7F50,stroke-width:2px;
    classDef training fill:#FFF3CD,stroke:#FFC107,stroke-width:2px;
    classDef inference fill:#D4EDDA,stroke:#28A745,stroke-width:2px;
    classDef risks fill:#F8D7DA,stroke:#DC3545,stroke-width:2px;
    classDef mitigations fill:#CCE5FF,stroke:#007BFF,stroke-width:2px;

    %% Data Sources
    A[Training Data Sources]:::dataSources
    A1[Structured Data]:::dataSources --> A
    A2[Unstructured Data]:::dataSources --> A
    A3[Sensitive Data]:::dataSources --> A
    A -->|Data Collection| B[Preprocessed Training Data]:::training
    A -->|Raw Inputs| C[External Data Sources<br>(APIs, Databases)]:::dataSources

    %% Training Phase
    B -->|Processed for Training| D[LLM Training Pipeline]:::training
    C -->|External Knowledge Retrieval| D
    D -->|Embedding Generation| E[LLM Model Weights]:::training

    %% Evaluation Phase
    D -->|Red Team Testing| D1[Evaluation Tools<br>(Adversarial Testing)]:::training
    D1 -->|Validation| E

    %% Deployment Phase
    E -->|Deployed in| F[Inference API<br>(Chat Interface)]:::inference
    E -->|Integrated with| G[RAG Pipelines<br>(Retrieval-Augmented Generation)]:::inference
    G -->|External Data Sources| C

    %% Vulnerable Points
    B -->|Poisoning Attacks| H[Training Data Vulnerabilities]:::risks
    D -->|Data Leakage Testing| I[Model Training Vulnerabilities]:::risks
    G -->|RAG Contextual Drift| J[Pipeline Exploitation]:::risks
    F -->|Inference Attacks| K[Prompt Injection<br>or Jailbreaks]:::risks
    F -->|Output Misuse| L[Data Exfiltration<br>via Outputs]:::risks
    C -->|Data Contamination| M[External Data<br>Injection Risks]:::risks

    %% Monitoring Feedback Loop
    subgraph Monitoring and Feedback
        N[Monitoring Tools<br>(Anomaly Detection)]:::mitigations -->|Real-Time Alerts| F
        N -->|Feedback to Training| D
        O[Content Filters<br>(Output Validation)]:::mitigations --> F
        P[Data Validation Pipelines<br>(Securing External Sources)]:::mitigations --> C
    end

    %% Class Assignments
    class A,A1,A2,A3,C dataSources;
    class B,D,E,D1 training;
    class F,G inference;
    class H,I,J,K,L,M risks;
    class N,O,P mitigations;
