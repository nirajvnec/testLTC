```mermaid
graph TD
    A[Desktop and Web Applications] --> B[Register in GAIA]
    B --> C[GAIA provides unique client ID for environments (PROD, UAT, SIT)]
    C --> D[Application makes requests for authentication]
    D --> E{GAIA Endpoint (PROD, UAT, SIT)}
    E --> F[GAIA responds with access token]
    F --> G[Application fetches user identity details from access token]
    G --> H[Complete authorization]
    
    %% Adding styles to the nodes
    style A fill:#8FDA6F,stroke:#333,stroke-width:2px
    style B fill:#66CCFF,stroke:#333,stroke-width:2px
    style C fill:#FFD700,stroke:#333,stroke-width:2px
    style D fill:#FF6347,stroke:#333,stroke-width:2px
    style E fill:#A52A2A,stroke:#333,stroke-width:2px
    style F fill:#9370DB,stroke:#333,stroke-width:2px
    style G fill:#48D1CC,stroke:#333,stroke-width:2px
    style H fill:#8FDA6F,stroke:#333,stroke-width:2px
