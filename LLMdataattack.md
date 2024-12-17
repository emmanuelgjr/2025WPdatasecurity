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
        legend1[**Orange**: Data Sources]:::dataSources
        legend2[**Yellow**: Training Processes]:::training
        legend3[**Green**: Inference Systems]:::inference
        legend4[**Red**: Vulnerabilities & Risks]:::risks
        legend5[**Blue**: Mitigations & Controls]:::mitigations
        legend6[**Grey**: Attack Steps (Kill Chain), Red Team Tools, Governance]:::killchain
    end

    %% Data Sources
    A[Training Data Sources]:::dataSources
    A1[Structured Data (CSV, Databases)]:::dataSources --> A
    A2[Unstructured Data (Text Corpora, Web)]:::dataSources --> A
    A3[Sensitive/Internal Data (Proprietary Docs)]:::dataSources --> A
    A -->|Data Collection| B[Preprocessed Training Data]:::training
    A -->|Raw Inputs| C[External Data Sources<br>(APIs, Web Feeds)]:::dataSources

    %% Training Pipeline
    B -->|Processed for Training| D[LLM Training Pipeline<br>(Tokenization, Embeddings, Fine-tuning)]:::training
    C -->|Knowledge Retrieval| D
    D -->|Weights & Parameters| E[LLM Model Weights]:::training

    %% Evaluation (Red Teaming)
    D -->|Adversarial Testing<br>(MITRE ATLAS Tactics)| D1[Red Team Tools & Evaluation:<br>- Backdoor Insertion<br>- Data Poisoning<br>- Trojans<br>(e.g., Trojaning Language Models, OpenAI Evals)]:::redTeamTools
    D1 -->|Validation & Hardening Guidance| E

    %% Deployment & Inference
    E -->|Deployed to Production| F[Inference API:<br>Chat Interface, Model Serving]:::inference
    E -->|Integrated with| G[RAG Pipelines:<br>Retrieval-Augmented Generation]:::inference
    G -->|Queries| C

    %% Vulnerabilities & Risks
    B -->|Poisoning Attacks<br>(Backdoors, Watermark Removal)| H[Training Data Risks]:::risks
    D -->|Parameter Extraction, Memorization| I[Model Training Vulnerabilities]:::risks
    G -->|Context Manipulation, Fake Docs| J[RAG Pipeline Exploits]:::risks
    F -->|Prompt Injection, Jailbreaks| K[Inference Attacks]:::risks
    F -->|Sensitive Data Leakage| L[Data Exfiltration via Outputs]:::risks
    C -->|Malicious Source Injections| M[Supply Chain & External Data Injection Risks]:::risks

    %% Mitigations & Defensive Controls
    subgraph Defensive_Layer[Defensive & Governance Layers]
        O[Content Filters & Policy Enforcement:<br>- RLHF, Toxicity/Bias Filters<br>- Guardrails on Outputs]:::mitigations --> F
        P[Data Validation & Sanitization:<br>- Schema Checks, Domain Whitelists]:::mitigations --> C
        Q[Adversarial Detection Models:<br>- Anomaly Detection, Perturbation Filters]:::mitigations --> F
        R[Secure Proxy Layers:<br>- Input Sanitization Gateways]:::mitigations --> F
        S[Compliance & Audit Checks:<br>- GDPR/CCPA Audits, Internal Policy Review]:::mitigations --> D
        T[Model Hardening:<br>- Differential Privacy<br>- Watermarking<br>- Parameter Encryption]:::mitigations --> D
        V[Rollback & Disaster Recovery:<br>- Version Control, Snapshots]:::mitigations --> E
    end

    %% Monitoring & Feedback
    subgraph Monitoring_Feedback[Monitoring, SIEM/SOAR & Feedback Loops]
        N[Monitoring Tools:<br>- SIEM/SOAR Integration,<br>- Telemetry Dashboards,<br>- Performance vs Security Metrics]:::mitigations --> F
        N -->|Feedback for Re-Training| B
        U[Human-in-the-Loop Review (Security Analysts):<br>- Incident Response, Alert Triage]:::mitigations --> F
        U -->|Policy & Model Updates| D
    end

    %% Red Team Operations & Collaboration
    subgraph Red_Team_Operations_Center[Red Team Operations Center]
        X[Red Team Playbooks & SOPs:<br>- MITRE ATLAS & Attack Patterns<br>- Threat Intel Feeds<br>- Adversarial Simulation Plans]:::controlCenter
        X -->|Controlled Simulation| D1
        X -->|Reports Findings & Recommendations| U
    end

    %% Sandbox Environment
    subgraph Secure_Sandbox[Safe Testing & Simulation Environment]
        Y[Isolated Sandbox:<br>- Replay Attacks Safely<br>- Test Patches, Filters<br>- Evaluate Changes Before Prod]:::controlCenter
        Y -->|Refine Detection & Controls| D
        Y -->|Validate Red Team Methods| D1
    end

    %% Threat Actor Personas
    subgraph Threat_Actors[Threat Actor Personas (for Red Team Context)]
        TA1[Insider Threats:<br>- Employees with Access<br>- Can Poison Training Data]:::governance
        TA2[External Advanced APT Groups:<br>- Skilled Actors Using RAG Exploits,<br>- Parameter Extraction Attacks]:::governance
        TA3[Script Kiddies & Opportunistic Hackers:<br>- Basic Prompt Injections,<br>- Simple Malicious Inputs]:::governance
    end

    TA1 -.-> B
    TA2 -.-> I
    TA3 -.-> K

    %% Lockheed Martin Cyber Kill Chain Steps
    subgraph Kill_Chain[Lockheed Martin Cyber Kill Chain Steps]
        R1[Reconnaissance:<br>- Identify LLM Architecture,<br>- Data Sources]:::killchain
        R2[Weaponization:<br>- Craft Poisoned Data,<br>- Create Adversarial Prompts]:::killchain
        R3[Delivery:<br>- Inject Malicious Data via External Sources]:::killchain
        R4[Exploitation:<br>- Prompt Injection,<br>- Contextual RAG Manipulation]:::killchain
        R5[Installation:<br>- Embed Backdoors into Model Weights<br>- Persist Malicious Instructions]:::killchain
        R6[Command & Control:<br>- Interact with Deployed Model,<br>- Maintain Query-based C2]:::killchain
        R7[Actions on Objectives:<br>- Extract Sensitive Data,<br>- Cause Misinformation]:::killchain
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

    %% Red Team Tools Extended (Specific Toolkits)
    subgraph Extended_Red_Team_Tools[Red Team Tools & Frameworks]
        RT1[Model Poisoning / Backdoor Kits:<br>- Trojaning LMs (ETH Zurich Tools)]:::redTeamTools --> H
        RT2[Adversarial Prompt Suites:<br>- TextFooler, Polyjuice,<
