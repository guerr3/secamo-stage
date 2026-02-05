---
date: 2026-02-03
version: "1"
tags:
  - diagram
  - analyse
---
Eerste "brainstorm" diagram waarbij Python FastAPI als een tussenlaag fungeert in de vorm van de orchestrator waardoor in n8n nieuwe workflows (=tools) kunnen gebouwd worden op een low-code platform die communiceren met de orchestrator. Deze custom python layer fungeert vooral als interactieplek tussen intern en externe handelingen, zodat n8n niet te maken krijgt met credentials of andere sensitieve data maar enkel taken uitvoert doordat n8n heel veel "connectors" heeft. 

```mermaid
graph TB
    A["<b>OPTIONAL: Camunda</b><br/><i>Week 10+</i><br/><br/>Long-running processes<br/>approval workflows, SLAs"]
    
    B["<b>PRIMARY: Python FastAPI Service</b><br/><i>Week 7-9</i><br/><br/>• CloudOrchestrator<br/>• TicketingOrchestrator<br/>• PasswordOrchestrator<br/>• Config management<br/>• Error handling & logging"]
    
    C["<b>TACTICAL: n8n</b><br/><i>Week 8-10</i><br/><br/>• Notification workflows<br/>• Scheduled tasks<br/>• Webhook receivers<br/>• Visual workflows"]
    
    D1[Azure AD]
    D2[AWS IAM]
    D3[ServiceNow]
    D4[Jira]
    D5[SDP]
    D6[Keeper]
    
    A -->|REST API| B
    B -->|Simple ops| C
    C --> D1
    C --> D2
    C --> D3
    C --> D4
    C --> D5
    C --> D6
    
    classDef optional fill:#e1f5ff,stroke:#0288d1,stroke-width:2px
    classDef primary fill:#fff9c4,stroke:#f57c00,stroke-width:3px
    classDef tactical fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef api fill:#e8f5e9,stroke:#388e3c,stroke-width:1px
    
    class A optional
    class B primary
    class C tactical
    class D1,D2,D3,D4,D5,D6 api

```
