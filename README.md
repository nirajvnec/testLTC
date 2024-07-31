# Project Title

A brief description of your project.

## Flowchart

```mermaid
flowchart TD
    %% Nodes
    A("Desktop and Web Applications")
    B["Register in GAIA"]
    C("GAIA provides unique client ID for environments (PROD, UAT, SIT)"]
    D("Application makes requests for authentication")
    E{"GAIA Endpoint (PROD, UAT, SIT)"}
    F("GAIA responds with access token")
    G("Application fetches user identity details from access token")
    H("Complete authorization")

    %% Edge connections between nodes
    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H

    %% Styling for clarity
    style A color:#FFFFFF, fill:#4CAF50, stroke:#4CAF50
    style B color:#FFFFFF, fill:#2196F3, stroke:#2196F3
    style C color:#FFFFFF, fill:#FFC107, stroke:#FFC107
    style D color:#000000, fill:#FF5722, stroke:#FF5722
    style E color:#FFFFFF, fill:#795548, stroke:#795548
    style F color:#FFFFFF, fill:#9C27B0, stroke:#9C27B0
    style G color:#000000, fill:#00BCD4, stroke:#00BCD4
    style H color:#FFFFFF, fill:#8BC34A, stroke:#8BC34A
