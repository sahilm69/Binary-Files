```mermaid
flowchart TD

%% ========================
%% SECTION 1: SERVER SIDE
%% ========================

A[Developer uploads new firmware] --> B[Server runs jdiff algorithm]
B --> C[Generate patch file - difference between old and new firmware]
C --> D[Store patch file and metadata on server]

%% ========================
%% SECTION 2: NORMAL OPERATION
%% ========================

D --> E[Gateway periodically uploads data to server]
E --> F{Server response 200 OK?}

F -->|No update| G[Continue normal operation]
F -->|Update available| H[Server includes update flag in response]

%% ========================
%% SECTION 3: GATEWAY UPDATE
%% ========================

H --> I[Gateway detects update flag]
I --> J[Gateway switches to bootloader mode]
J --> K[Bootloader downloads patch file from server]

%% ========================
%% SECTION 4: PATCH APPLY
%% ========================

K --> L[Read current firmware image from flash and store to SD card]
L --> M[Apply patch using jdiff algorithm]
M --> N[Generate new firmware image file on SD card]
N --> O[Validate patch integrity using checksum or CRC]

%% ========================
%% SECTION 5: FLASH UPDATE
%% ========================

O --> P[Erase flash application area]
P --> Q[Write new firmware image from SD card to flash]
Q --> R[Verify firmware version or CRC]

%% ========================
%% SECTION 6: POST-UPDATE
%% ========================

R -->|Valid| S[Jump to application]
R -->|Invalid| T[Revert to previous firmware or safe mode]
