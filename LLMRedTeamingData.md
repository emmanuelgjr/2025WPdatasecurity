# LLM Red Teaming Data Security - Diagram

```mermaid
flowchart TB

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

%% Main Pipeline (Center)
A1[Structured Data]:::dataSources
A2[Unstructured Data]:::dataSources
A3[Sensitive Data]:::dataSources
A[Training Data Sources]:::dataSources
B[Preprocessed Data]:::training
D[Training Pipeline]:::training
E[Model Weights]:::training
F[Inference API]:::inference
G[RAG Pipeline]:::inference
C[External Data]:::dataSources

A1 --> A
A2 --> A
A3 --> A
A --> B
B --> D
D --> E
E --> F
F --> G
G --> C

%% Red Team Evaluation (Right side near Training)
D1[Red Team Testing]:::redTeamTools
D --> D1
D1 --> E

%% Vulnerabilities (Left side)
H[Poisoned Training Data]:::risks
I[Parameter Extraction]:::risks
K[Prompt Injection]:::risks
L[Data Leakage]:::risks
J[RAG Context Manipulation]:::risks
M[Supply Chain Attack]:::risks

%% Align vulnerabilities with their respective step
%% Horizontal lines from pipeline node to vulnerability on the left
A -.-> H
D -.-> I
F -.-> K
F -.-> L
G -.-> J
C -.-> M

%% Mitigations (Right side)
O[Content Filters]:::mitigations
T[Model Hardening]:::mitigations
Q[Adversarial Detection]:::mitigations
R[Secure Proxy]:::mitigations
P[Data Validation]:::mitigations
V[Rollback/Recovery]:::mitigations

%% Align mitigations with steps
B -.-> P
D -.-> T
F -.-> O
F -.-> Q
F -.-> R
E -.-> V

%% Monitoring & Human Review (Bottom)
N[Monitoring Tools]:::mitigations
U[Human Review]:::mitigations

%% Connect upwards to pipeline
N --> B
N --> F
U --> D
U --> F

%% Threat Actors (Bottom Left)
TA1[Insider Threat]:::governance
TA2[APT Group]:::governance
TA3[Script Kiddie]:::governance

%% Threat actors pointing to vulnerabilities to the left (no crossing lines, just vertical alignment)
TA1 --> H
TA2 --> I
TA3 --> K

%% Kill Chain (Bottom)
R1[Recon]:::killchain
R2[Weaponization]:::killchain
R3[Delivery]:::killchain
R4[Exploitation]:::killchain
R5[Installation]:::killchain
R6[Command & Control]:::killchain
R7[Actions]:::killchain

%% Straight lines up from kill chain to pipeline
R1 --> A
R2 --> B
R3 --> C
R4 --> F
R5 --> D
R6 --> F
R7 --> F

%% Red Team Ops & Sandbox (Bottom Right)
X[Red Team Playbooks]:::controlCenter
Y[Sandbox]:::controlCenter

X --> D1
X --> U
Y --> D1
Y --> D
