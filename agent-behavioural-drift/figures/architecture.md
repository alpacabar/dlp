# Architecture — per-agent drift detector (Study 2)

Rendered view of [`architecture.mmd`](architecture.mmd). Edit the `.mmd` source, then copy it into the block below.

```mermaid
flowchart TB
    subgraph RT["Agent runtime"]
        direction LR
        AG["Agent"]
        MEM[("Memory store")]
        TLS["Tools"]
        AG <--> MEM
        AG <--> TLS
    end

    subgraph TEL["Telemetry"]
        direction LR
        COL["Trace collector<br/>OpenTelemetry spans"]
        CF["Content filter<br/>drop messages, arguments, results"]
        COL --> CF
    end

    subgraph DET["Detection — content-free"]
        direction LR
        FE["Feature extractor<br/>tool order · timing gaps"]
        PF["Precondition filter<br/>recall-before-send"]
        DS["Drift scorer<br/>distance from baseline"]
        FE --> PF --> DS
    end

    BS[("Per-agent baseline<br/>clean sessions")]
    OUT["Ranked sessions<br/>for review"]

    RT --> COL
    CF --> FE
    PF -.->|"enrollment window, assumed clean"| BS
    BS --> DS
    DS --> OUT

    style RT fill:#FCE8E6,stroke:#E5B5B0,stroke-width:1px
    style TEL fill:#E6F4EA,stroke:#A8CDB5,stroke-width:1px
    style DET fill:#E8F0FE,stroke:#AAC0E4,stroke-width:1px

    classDef box fill:#FFFFFF,stroke:#9FAEBF,stroke-width:1px,color:#1A2333
    classDef store fill:#FEF7E0,stroke:#DEC489,stroke-width:1px,color:#1A2333
    classDef out fill:#F4F6F8,stroke:#8B95A5,stroke-width:1px,color:#1A2333

    class AG,MEM,TLS,COL,CF,FE,PF,DS box
    class BS store
    class OUT out
```

## Caption for `architecture.mmd`

> **Figure N.** Content-free drift detection. Agent activity is collected as
> OpenTelemetry spans; payload fields are dropped before feature extraction, so detection
> operates only on fields logged by default. The recall-before-send precondition narrows
> the candidate set, and a drift score against that agent's own baseline ranks what
> remains. Baseline sessions pass the same precondition, so candidates are compared
> against the matched population rather than against all prior activity. Cleanliness is
> established by the enrollment window rather than by the pipeline itself (dashed).
