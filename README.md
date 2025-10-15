```mermaid
flowchart TD

%% ========================
%% SECTION 1: SERVER SIDE
%% ========================

A[New firmware's uploaded] --> B[Server runs jdiff algorithm]
B --> C[Generated patch file's stored on server]

%% ========================
%% SECTION 2: NORMAL OPERATION
%% ========================

C --> D[Gateway periodically uploads data to server]
D --> E{Server response 200 OK?}

E -->|No update| F[Continue normal operation]
E -->|Update available| G[Server includes update flag in response]

%% ========================
%% SECTION 3: GATEWAY UPDATE
%% ========================

G --> H[Gateway detects update flag]
H --> I[Gateway switches to bootloader mode]
I --> J[Bootloader downloads patch file from server]

%% ========================
%% SECTION 4: PATCH APPLY
%% ========================

J --> K[Read current firmware image from flash and store to SD card]
K --> L[Apply patch using jdiff algorithm]
L --> M[Generate new firmware image file on SD card]
M --> N[Validate patch integrity using checksum or CRC]

%% ========================
%% SECTION 5: FLASH UPDATE
%% ========================

N --> O[Erase flash application area]
O --> P[Write new firmware image from SD card to flash]
P --> Q[Verify firmware version or CRC]

%% ========================
%% SECTION 6: POST-UPDATE
%% ========================

Q -->|Valid| R[Jump to application]
Q -->|Invalid| S[Revert to previous firmware or safe mode]
