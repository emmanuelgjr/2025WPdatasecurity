# LLM Red Teaming Data Security - Diagram

```mermaid
flowchart LR

%% Styles
classDef dataSources fill:#FFDDC1,stroke:#FF7F50,stroke-width:2px;
classDef training fill:#FFF3CD,stroke:#FFC107,stroke-width:2px;
classDef inference fill:#D4EDDA,stroke:#28A745,stroke-width:2px;
classDef risks fill:#F8D7DA,stroke:#DC3545,stroke-width:2px;
classDef mitigations fill:#CCE5FF,stroke:#007BFF,stroke-width:2px;
classDef killchain fill:#E2E2E2,stroke:#6C757D,stroke-width:1px,font-weight:bold;
classDef redTeamTools fill:#F1F1F1,stroke:#343A40,stroke-width:1px;
classDef governance fill:#FFFFFF,stroke:#6C757D,stroke-width:1px;

%% Legend at top
subgraph Legend
direction LR
L1[Orange = Data Sources]:::dataSources
L2[Yellow = Training Steps]:::training
L3[Green = Inference Steps]:::inference
L4[Red = Vulnerabilities]:::risks
L5[Blue = Mitigations]:::mitigations
L6[Grey = Kill Chain, Actors, Tools]:::killchain
Note[Top row: Threat Actors, Vulnerabilities\nMiddle: Data→Training→Inference→RAG→External\nBottom: Mitigations, Kill Chain\nLeft to right flow, vertical lines to connect]
end

%% Pipeline (Center row)
A[Training Data]:::dataSources
B[Preprocessed Data]:::training
D[Training Pipeline]:::training
E[Model Weights]:::training
F[Inference API]:::inference
G[RAG Pipeline]:::inference
C[External Data]:::dataSources

A --> B --> D --> E --> F --> G --> C

%% Vulnerabilities (Top row)
TA1[Insider Threat]:::governance
TA2[APT Group]:::governance
TA3[Script Kiddie]:::governance

H[Poisoned Data]:::risks
I[Parameter Extraction]:::risks
K[Prompt Injection]:::risks
L[Data Leakage]:::risks
J[RAG Context Attack]:::risks
M[Supply Chain Attack]:::risks

%% Align vulnerabilities above their nodes
%% Straight down lines
H --- B
I --- D
K --- F
L --- F
J --- G
M --- C

%% Threat actors feed horizontally into the vulnerabilities on the left side
TA1 --> H
TA2 --> I
TA3 --> K

%% Red Team Tools (top right)
X[Red Team Testing]:::redTeamTools
Y[Trojaning Kits]:::redTeamTools
Z[Prompt Attack Tools]:::redTeamTools

X --> D
Y --> D
Z --> F

%% Mitigations (Bottom row)
P[Data Validation]:::mitigations
T[Model Hardening]:::mitigations
O[Content Filters]:::mitigations
Q[Adversarial Detection]:::mitigations
R[Secure Proxy]:::mitigations
V[Rollback Recovery]:::mitigations

%% Align mitigations below their nodes
%% Straight up lines
P --- B
T --- D
V --- E
O --- F
Q --- F
R --- F

%% Monitoring and Human Review (bottom center)
N[Monitoring]:::mitigations
U[Human Review]:::mitigations

N --- B
N --- F
U --- D
U --- F

%% Kill Chain (very bottom)
R1[Recon]:::killchain
R2[Weaponization]:::killchain
R3[Delivery]:::killchain
R4[Exploitation]:::killchain
R5[Installation]:::killchain
R6[Command Control]:::killchain
R7[Actions]:::killchain

%% Straight lines up to pipeline
R1 --- A
R2 --- B
R3 --- C
R4 --- F
R5 --- D
R6 --- F
R7 --- F
