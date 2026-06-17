```mermaid graph TD
    %% Styling Definitions
    classDef standard fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    classDef exception fill:#fff3e0,stroke:#ff9800,stroke-width:2px,stroke-dasharray: 5 5;
    classDef waste fill:#ffebee,stroke:#ef5350,stroke-width:2px;
    classDef critical fill:#b71c1c,stroke:#b71c1c,stroke-width:2px,color:#fff;

    %% Section 1: Standard Flow
    subgraph S1 [1. Standard Work Order Flow]
        A[Item Disposition: Work Order] --> B[Item Picked]
        B -->|FUD Risk| FUD((FUD Risk))
        B --> C[Measurement Process]
    end
    
    %% Section 2: Sweep & Exception Flow
    subgraph S2 [2. Exception Identification & Staging]
        D[Item Identified as Needing Cubiscan via Sweep] --> E{Sweeper Criteria}
        E -->|No Record| F[Set Aside Upstairs]
        E -->|No Knowledge| F
        E -->|Fud Risk| F
        
        F --> G[Delayed Staging: Multiple Floors / Multiple Totes]
        G --> H[Sorted by Exceptions & Delay]
    end

    %% Section 3: Cubiscan Processing & Loops
    subgraph S3 [3. Cubiscan & Waste Loops]
        H --> I[Cubiscan: Weight & Dimensions]
        
        %% Loop A
        I -->|Exceptions Again| H
        
        %% Loop B
        I -->|May become Non-Sort but sent back| J[Transport Upstairs to Downstairs]
        J -->|Waste Loop| G
        
        %% Duplicate Work Loop
        I -->|Repeated ASIN Not Recognised| K[Duplicate Work: High Volume / Low Gain]
        K -->|System Drop / Re-handling| I
        K -->|Returned to Start| C
    end

    %% Global Risk Factor
    subgraph Risk [Operational Risk]
        L[/"Items 'Lost' <br>(Unaccounted for / Non-replenished Inventory)"/]
    end
    
    F -.-> Risk
    G -.-> Risk

    %% Apply Classes
    class A,B,C standard;
    class D,E,F,H exception;
    class G,I,J,K waste;
    class L critical;

```
