# LLM Red Teaming Data Attack Flow Diagram - Kill Chain

```mermaid
flowchart LR

    %% Style Definitions for Nodes
    classDef dataSources fill:#FFDDC1,stroke:#FF7F50,stroke-width:2px;
    classDef training fill:#FFF3CD,stroke:#FFC107,stroke-width:2px;
    classDef inference fill:#D4EDDA,stroke:#28A745,stroke-width:2px;
    classDef risks fill:#F8D7DA,stroke:#DC3545,stroke-width:2px;
    classDef mitigations fill:#CCE5FF,stroke:#007BFF,stroke-width:2px;
    classDef killchain fill:#E2E2E2,stroke:#6C757D,stroke-width:1px,font-weight:bold;
    classDef redTeamTools fill:#F1F1F1,stroke:#343A40,stroke-width:1px,stroke-dasharray:3 3;
    classDef controlCenter fill:#EFEFEF,stroke:#6C757D,stroke-width:1px;
    classDef governance fill:#FFFFFF,stroke:#6C757D,stroke-width:1px,stroke-dasharray:2 2;

    %% Style Definitions for Edges
    %% Different classes for edges to make them more readable
    classDef dataflow stroke:#007BFF,stroke-width:1.5px;
    classDef attack stroke:#DC3545,stroke-width:1.5px;
    classDef control stroke:#28A745,stroke-width:1.5px;
    classDef monitoring stroke:#6f42c1,stroke-width:1.5px;
    classDef killchainEdge stroke:#6C757D,stroke-width:1.5px,stroke-dasharray:2 2;

    %% Legend
    subgraph Legend
    direction LR
        L1[Orange: Data Sources]:::dataSources
        L2[Yellow: Training]:::training
        L3[Green: Inference]:::inference
        L4[Red: Vulnerabilities]:::risks
        L5[Blue: Mitigations]:::mitigations
        L6[Grey: Kill Chain & Tools]:::killchain
        Note[Edge Colors:\nBlue=Dataflow\nRed=Attacks\nGreen=Controls\nPurple=Monitoring\nGrey=Kill Chain]
    end

    %% Data and Training
    subgraph Data_and_Training
    direction LR
        A[Training Data Sources]:::dataSources
        A1[Structured Data]:::dataSources
        A2[Unstructured Data]:::dataSources
        A3[Sensitive Internal Data]:::dataSources
        
        A1-->A:::dataflow
        A2-->A:::dataflow
        A3-->A:::dataflow

        B[Preprocessed Training Data]:::training
        A-->|Collect|B:::dataflow

        C[External Data Sources]:::dataSources
        C-->|Input Feeds|B:::dataflow

        D[LLM Training Pipeline]:::training
        B-->|Train|D:::dataflow
        C-->|Retrieval|D:::dataflow

        E[LLM Model Weights]:::training
        D-->|Generate Weights|E:::dataflow
    end

    %% Red Team Evaluation
    subgraph Red_Team_Evaluation
    direction LR
        D1[Red Team Testing]:::redTeamTools
        D-->|Adversarial Test|D1:::attack
        D1-->|Harden|E:::control
    end

    %% Deployment and Inference
    subgraph Deployment_and_Inference
    direction LR
        F[Inference API]:::inference
        E-->|Deploy|F:::dataflow

        G[RAG Pipelines]:::inference
        E-->|Integrate|G:::dataflow
        G-->|Query|C:::dataflow
    end

    %% Vulnerabilities
    subgraph Vulnerabilities
    direction LR
        H[Training Data Poisoning]:::risks
        I[Parameter Extraction]:::risks
        J[RAG Context Manipulation]:::risks
        K[Prompt Injection]:::risks
        L[Data Leakage Outputs]:::risks
        M[Supply Chain Attacks]:::risks

        B-->|Poisoned Data|H:::attack
        D-->|Extract Params|I:::attack
        G-->|Context Attack|J:::attack
        F-->|Jailbreak|K:::attack
        F-->|Leak Info|L:::attack
        C-->|Malicious Input|M:::attack
    end

    %% Mitigations and Controls
    subgraph Mitigations_and_Controls
    direction LR
        O[Content Filters]:::mitigations
        P[Data Validation]:::mitigations
        Q[Adversarial Detection]:::mitigations
        R[Secure Proxy Layers]:::mitigations
        S[Compliance Audits]:::mitigations
        T[Model Hardening]:::mitigations
        V[Rollback Recovery]:::mitigations

        O-->|Filter Outputs|F:::control
        P-->|Check Inputs|C:::control
        Q-->|Detect Threat|F:::control
        R-->|Sanitize Input|F:::control
        S-->|Audit|D:::control
        T-->|Harden Model|D:::control
        V-->|Revert|E:::control
    end

    %% Monitoring and Feedback
    subgraph Monitoring_and_Feedback
    direction LR
        N[Monitoring SIEM]:::mitigations
        U[Human Analysts]:::mitigations

        N-->|Logs|F:::monitoring
        N-->|Retrain Data|B:::monitoring
        U-->|Review|F:::monitoring
        U-->|Policy Updates|D:::monitoring
    end

    %% Red Team Operations
    subgraph Red_Team_Operations
    direction LR
        X[Red Team Playbooks]:::controlCenter
        Y[Isolated Sandbox]:::controlCenter

        X-->|Simulate|D1:::attack
        X-->|Report|U:::monitoring
        Y-->|Test Controls|D:::monitoring
        Y-->|Validate Attacks|D1:::attack
    end

    %% Threat Actors
    subgraph Threat_Actors
    direction LR
        TA1[Insider Threat]:::governance
        TA2[APT Group]:::governance
        TA3[Script Kiddie]:::governance
    end

    TA1-.->H:::attack
    TA2-.->I:::attack
    TA3-.->K:::attack

    %% Kill Chain
    subgraph Kill_Chain
    direction LR
        R1[Recon]:::killchain
        R2[Weaponization]:::killchain
        R3[Delivery]:::killchain
        R4[Exploitation]:::killchain
        R5[Installation]:::killchain
        R6[Command Control]:::killchain
        R7[Actions]:::killchain

        R1-->R2:::killchainEdge
        R2-->R3:::killchainEdge
        R3-->R4:::killchainEdge
        R4-->R5:::killchainEdge
        R5-->R6:::killchainEdge
        R6-->R7:::killchainEdge

        A-.->R1:::killchainEdge
        B-.->R2:::killchainEdge
        C-.->R3:::killchainEdge
        K-.->R4:::killchainEdge
        H-.->R5:::killchainEdge
        F-.->R6:::killchainEdge
        L-.->R7:::killchainEdge
    end

    %% Extended Tools
    subgraph Extended_Tools
    direction LR
        RT1[Poisoning Kits]:::redTeamTools-->H:::attack
        RT2[Prompt Attack Suites]:::redTeamTools-->K:::attack
        RT3[Extraction Tools]:::redTeamTools-->I:::attack
        RT4[RAG Injection Tools]:::redTeamTools-->J:::attack
        RT5[Telemetry Tampering]:::redTeamTools
        RT5-.->N:::attack
    end
