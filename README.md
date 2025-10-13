flowchart TD

%% ========================
%% SECTION 1: SERVER SIDE
%% ========================

A[Developer Uploads New Firmware] --> B[Server Runs jdiff Algorithm]
B --> C[Generate Patch File (Diff Between Old & New Firmware)]
C --> D[Store Patch File & Metadata on Server]

%% ========================
%% SECTION 2: NORMAL OPERATION
%% ========================

D --> E[Gateway Periodically Uploads Data to Server]
E --> F{Server Response 200 OK}

F -->|No Update| G[Continue Normal Operation]
F -->|Update Available| H[Server Includes Update Flag in Response]

%% ========================
%% SECTION 3: GATEWAY UPDATE
%% ========================

H --> I[Gateway Detects Update Flag]
I --> J[Gateway Switches to Bootloader Mode]
J --> K[Bootloader Downloads Patch File from Server]

%% ========================
%% SECTION 4: PATCH APPLY
%% ========================

K --> L[Read Current Firmware Image from Flash → Store to SD Card]
L --> M[Apply Patch Using jdiff Algorithm]
M --> N[Generate New Firmware Image File on SD Card]
N --> O[Validate Patch Integrity (Checksum/CRC)]

%% ========================
%% SECTION 5: FLASH UPDATE
%% ========================

O --> P[Erase Flash Application Area]
P --> Q[Write New Firmware Image from SD Card to Flash]
Q --> R[Verify Firmware (Version/CRC)]

%% ========================
%% SECTION 6: POST-UPDATE
%% ========================

R -->|Valid| S[Jump to Application]
R -->|Invalid| T[Revert to Previous Firmware or Safe Mode]

%% ========================
%% VISUAL STYLE
%% ========================

classDef server fill:#e3f2fd,stroke:#2196f3,stroke-width:1px;
classDef gateway fill:#fff3e0,stroke:#ff9800,stroke-width:1px;
classDef process fill:#f1f8e9,stroke:#689f38,stroke-width:1px;

class A,B,C,D server
class E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T gateway
