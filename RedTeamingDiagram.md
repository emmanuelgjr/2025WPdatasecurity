# LLM Red Teaming Data Flow Diagram

```mermaid
flowchart TD
    %% Data Sources
    A[Training Data Sources]
    A1[Structured Data] --> A
    A2[Unstructured Data] --> A
    A3[Sensitive Data] --> A
    A -->|Data Collection| B[Preprocessed Training Data]
    A -->|Raw Inputs| C[External Data Sources: APIs, Databases]

    %% Training Phase
    B -->|Processed for Training| D[LLM Training Pipeline]
    C -->|External Knowledge Retrieval| D
    D -->|Embedding Generation| E[LLM Model Weights]

    %% Evaluation Phase
    D -->|Red Team Testing| D1[Evaluation Tools: Adversarial Testing]
    D1 -->|Validation| E

    %% Deployment Phase
    E -->|Deployed in| F[Inference API or Chat Interface]
    E -->|Integrated with| G[RAG Pipelines]
    G -->|External Data Sources| C

    %% Vulnerable Points
    B -->|Poisoning Attacks| H[Training Data Vulnerabilities]
    D -->|Data Leakage Testing| I[Model Training Vulnerabilities]
    G -->|RAG Contextual Drift| J[Pipeline Exploitation]
    F -->|Inference Attacks| K[Prompt Injection or Jailbreaks]
    F -->|Output Misuse| L[Data Exfiltration via Outputs]
    C -->|Data Contamination| M[External Data Injection Risks]

    %% Monitoring Feedback Loop
    subgraph Monitoring and Feedback
        N[Monitoring Tools] -->|Anomaly Detection| B
        N -->|Real-Time Alerts| F
        N -->|Feedback to Training| D
        O[Content Filters] -->|Output Validation| F
        P[Data Validation Pipelines] -->|Securing External Sources| C
    end
