---
date: 2026-02-04
stage_dag: "3"
tags:
  - analyse
  - technical
  - requirements
---
## Technische eisen orchestrator

Gebruik deze eisen als “requirements checklist” voor je analysedocument (non-functionals), met **PoC-doelwaarden** + vragen om ze te valideren.

**Performance**
    
- End-to-end “workflow latency” targets per workflow type: “fast path” (sec-min) vs “infra changes” (min-uur), plus maximale wachttijd voor approvals.
    
- Throughput/concurrency: hoeveel workflows tegelijk (peak) en hoeveel tasks per workflow; expliciet rekening houden met API rate limits van ticketing/cloud en backoff/retry policy.
    
- Observability-performance: elke run meetbaar (duration per step, queue/wait time, retry counts), omdat je later in Testing & Evaluatie execution times en success rate moet rapporteren.
  
- SLA per domein: Support (sneller, meer runs), HR/Invoicing (minder runs maar streng auditbaar), Implementation (mix van approvals + technische stappen).
    
- Timeouts & timers: “wacht op approval” is first-class (met escalatiepad indien er niet wordt gereageerd op deze approval gate).
    
**Schaalbaarheid**


- Horizontale schaalbaarheid van “workers/executors” (meer taken zonder redesign), plus backpressure (queueing) zodat failures of pieken geen cascade veroorzaken.
    
- Multi-tenant/segregatie: per klant (of per “tenant”) isolatie van credentials, state en logs (minstens logisch; idealiter ook via AWS accounts/roles).
    
- Idempotentie-by-design: workflows/modules moeten herhaalbaar zijn zonder drift (sluit aan bij je latere acceptatiecriteria rond idempotentie en 5 runs).

- Concurrency per domein + quotas: voorkom dat een bulk Support workflow de alle resources van de orchestrator verbruikt en HR approvals vertraagt.
    
- Connector scalability: adapters moeten rate limits respecteren (ticketing/ERP/HRIS) en een queue/backpressure mechanisme hebben.

**Security**
    
- Identity & access: least privilege per workflow step (aparte IAM role per “capability” zoals ticket-create, terraform-apply, read-only compliance export).
    
- Secrets: geen secrets in pipeline logs; centrale secrets strategy die past bij Secamo (Keeper staat al genoemd) en technische integratie met AWS Secrets Manager/Parameter Store waar nodig.[](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_a6b39ab4-ec1e-4486-a75f-a3ccb106d42c/7c8ff403-a567-4675-85ce-fb5a50fd4c24/Projectplanning.md)
    
- Auditability: tamper-evident logs (wie triggerde wat, welke approvals, welke changes), met correlatie-id over ticket ↔ workflow ↔ cloud changes (belangrijk in SOC/compliance context).​

- Sterke RBAC + audit trail per domein, want een PoC voor een Cyber Security bedrijf spreekt voor zich dat dit het principe van Security By Design hanteert (secrets, RBAC, audit logging).​
    
- Secrets strategy: Keeper wordt gebruikt maar werking is nog onduidelijk --> leg vast hoe departement-secrets gescheiden worden en hoe workflows ze consumeren zonder exposure.

## Vragen

**Voor Xander (scope & risico)**
    
- “Is deze PoC bedoeld als ‘demo in een apart PoC-account’ of moet het al aansluiten op bestaande Secamo AWS landing zone/guardrails (SCPs, org policies)? Ook ivm toegang tot interne sharepoint documenten, is het de bedoeling dat ik dit test met echte documenten of dummy documenten met dezelfde structuur maar dummy data ? 
    
- Welke security baseline is verplicht vanaf dag 1: MFA/SSO, netwerksegmentatie, loggingretentie, approval flows?
    
**Voor Cloud Engineers (performance/schaal)**
    
- Wat zijn de top-3 handelingen die vandaag via Terraform of andere automatisatietools lopen, en welke gebeuren nog manueel ‘in console’ (bv. azure cli, powershell graph) ?
    
- Welke API’s/limieten botsen jullie vaak tegenaan (AWS, Azure, ticketing), en hoe vangen jullie retries/backoff nu op?
    

**Voor SOC/Security (audit & HiL)**
    
- Welke handelingen moeten altijd human-approved zijn (bv. isoleren, account disable, firewall changes), en wat is de maximale ‘approval timeout’ voor incident-response? Of gebeurt dit al automatisch binnen defender/sentinel ?
    
- Welke logs/audit trails verwachten jullie achteraf te kunnen tonen (intern, klant, compliance) in de context van de taken die de process orchestrator heeft uitgevoerd? 

**Gerichte vragen aan andere departementen:**

**HR**
    
- Welke HR-systemen (HRIS) en identity lifecycle triggers bestaan er vandaag (onboarding/offboarding), en via welke kanalen komt de input binnen (mail, ticket, formulier)?
    
- Welke stappen moeten altijd human-approved zijn en door wie (HR lead, security officer)?​
    
**Invoicing / Finance**
    
- Welke tool is “source of truth” (ERP/boekhouding/PSA), en welke events moeten een workflow starten (contract getekend, project milestone, PO ontvangen)?
    
- Welke audit/compliance-eisen gelden (retentie, traceerbaarheid, minimale data in logs)?
    
**Support**
    
- Welke ticketingtools komen het meest voor (intern en bij klanten), en welke acties moeten altijd kunnen (create/update ticket, add comment, attach evidence)?​
    
- Wat is “volume & piek” (tickets/week) en welke handmatige stappen zijn het meest foutgevoelig?​
    
**Implementation / Project delivery**
    
- Welke projectmanagement/PSA tooling wordt gebruikt (planning, timesheets, change management), en waar zitten overdrachtsmomenten die nu mislopen?​
    
- Welke technische stappen zitten in implementation (bv. tenant provisioning), en welke zijn al IaC vs nog handmatig?