# Extra uitbreiding: Multi departement orchestrator

Omdat we de scope van deze process orchestrator niet beperken tot enkel CI/CD taken maar ook andere departementen (HR, Invoicing, Strategy, etc) erbij nemen, is het dus ook belangrijk dat we deze uitgebreidere scope verwerken in onze analyse. 

Het risico hierbij is dat het platform te generiek wordt (alles willen kunnen voor iedereen), dus moeten we governance afdwingen en duidelijke isolatie implementeren zoals: workflow-catalogus, standards, per departement duidelijke grenzen in data en rechten.​

## Architectuur-aanpassing (multi-dept)

We hanteren het principe van “één kern (orchestrator) + meerdere domeinen (subagents, workflows, suborchestrators)”:

- Centrale orchestrator capabilities (shared)
  In dit verhaal is er dus 1 centrale orchestrator die toegang heeft tot workflows, subagents of zelfs suborchestrators om specifieke taken uit te voeren. 
  
- Workflow-engine (executions, retries, timers, approvals/waits) met audit logging en correlatie-id per run.​
  
- Adapter/connector layer naar tools (ticketing, HR-systemen, finance/invoicing, cloud, e-mail/Teams/Slack), omdat ik al weet dat systemen variëren per klant en we dus abstracties nodig hebben. 
    
- Template/catalog: herbruikbare workflow building blocks (approve, create/update record, notify, export report, provisioning step).
    
- Domein-scheiding (per departement)
    
- RBAC per domein (HR, Invoicing, Support, Implementation) + “least privilege” per workflow-step.​
    
- Data boundaries: HR/finance zijn vaak gevoeliger dan CI/CD; dus definieer per domein logging-masking, minimale data in payloads, en wie approvals mag uitvoeren.
    
- “Namespace/tenant” concept: workflows + secrets + logs logisch gescheiden per departement (en eventueel per klant).