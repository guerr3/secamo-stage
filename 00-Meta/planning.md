## Projectoverzicht

Het project bestaat uit 4 duidelijke fases die lopen van **2 februari 2026** tot **15 mei 2026**.

- **Fase 1: Analyse** (4 weken) 
  **Doel:** Focus op requirements, toolselectie (min. 3 orchestrators vergelijken) en high-level architectuur. 
  **Milestone** Goedgekeurd analysedocument.​
    
- **Fase 2: Design** (2 weken) 
  **Doel:** Uitwerken van technische ontwerpen, modulaire IaC-structuur en security-architectuur. 
  **Milestone** Goedgekeurd Design / Ontwerp Document.​
    
- **Fase 3: Implementatie** (5 weken)  
  **Doel:** Opzetten van AWS-infra, CI/CD, IaC-modules en orchestrator-workflows. 
  **Milestone** Werkende PoC & Testplan.​
    
- **Fase 4: Testing & Evaluatie** (4 weken) 
  **Doel:** Validatie van de oplossing (min. 5 succesvolle runs), performance optimalisatie en documentatie. 
  **Milestone** Eindpresentatie & Adviesrapport.​
    



---

## Weekplanning

| Week    | Datumbereik | Fase            | Taken (Work items)                                                                                                                                                                                                                                | Outputs / Artefacten                                                |
| ------- | ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **W1**  | 02.02–08.02 | **Analyse**     | - Identificeer huidige automatiseringskansen & uitdagingen bij Secamo.  <br>- Breng bestaande tooling in kaart (IaC-gebruik, CI/CD, cloud footprint).  <br>- Bepaal technische eisen (Performance, Security).                                     | - Huidige situatie analyse  <br>- Lijst met requirements            |
| **W2**  | 09.02–15.02 | **Analyse**     | - Vergelijk min. 3 orchestrators (Camunda, Zeebe, Argo, etc.) op >7 criteria.  <br>- Evalueer architectuurprincipes (event-driven vs workflow, sync vs async).                                                                                    | - Orchestrator vergelijkingsmatrix                                  |
| **W3**  | 16.02–22.02 | **Analyse**     | - Leg functionele scope vast: Welke use cases (bv. VM Provisioning).  <br>- Definieer MVP vs "Nice to have" features.  <br>- Beslis Cloud Platform (AWS) & Tech Stack (Terraform, Ansible).                                                       | -Scope definitie  <br>-Keuze tech stack                             |
| **W4**  | 23.02–01.03 | **Analyse**     | - Werk High-Level Architectuur uit (schematisch diagram).  <br>- Plan budget voor cloud resources.  <br>- **Review Gate:** Dien Analysedocument in bij Xander.                                                                                    | - **Analysedocument**  <br>- High-Level Arch Diagram                |
| **W5**  | 02.03–08.03 | **Design**      | - Werk gedetailleerde technische architectuur & data flows uit.  <br>- Definieer deployment topologie (AWS netwerken, segmentatie).  <br>- Ontwerp modulaire structuur (Terraform folders, Ansible roles).                                        | - Dataflow Diagrammen  <br>- Module Specificaties                   |
| **W6**  | 09.03–15.03 | **Design**      | - Finaliseer workflow designs (BPMN/YAML) per use case.  <br>- Bepaal integratiestrategie (Git branching, CI/CD triggers).  <br>- Ontwerp Security/RBAC & Monitoring (Observability).  <br>- **Review Gate:** Dien Design Document in bij Xander. | - **Design Document**  <br>- Workflow Schema's  <br>- CI/CD Ontwerp |
| **W7**  | 16.03–22.03 | **Implement**   | - Initialiseer Git repository (structuur volgens design).  <br>- Bouw CI/CD pipeline (Linting, Validatie).  <br>- Provisioneer basis Cloud Infra (VPC, IAM, Security Groups).                                                                     | - Git Repo & CI/CD Pipeline  <br>- Basis AWS Infra                  |
| **W8**  | 23.03–29.03 | **Implement**   | - Deploy Orchestrator (EKS/ECS) & Database.  <br>- Configureer RBAC en Secrets Management.  <br>- Verifieer Orchestrator Healthcheck.                                                                                                             | - Draaiende Orchestrator  <br>- RBAC Config                         |
| **W9**  | 30.03–05.04 | **Implement**   | - Schrijf Terraform modules & Ansible roles.  <br>- Test modules standalone (dry-run/validate).  <br>- Schrijf custom scripts (Python/PowerShell workers).                                                                                        | - IaC Modules (v1)  <br>- Worker Scripts                            |
| **W10** | 06.04–12.04 | **Implement**   | - Implementeer specifieke workflows (bepaald in Analyse).  <br>- Integreer Orchestrator met IaC via API/Workers.  <br>- Implementeer notificaties ("Human in the Loop", Slack/Email).                                                             | - Functionele Workflows  <br>- Notificatie Integratie               |
| **W11** | 13.04–19.04 | **Implement**   | - End-to-end testen & debuggen.  <br>- Implementeer edge cases (Retries, Error handling).  <br>- Stel concept Testplanning Document op.                                                                                                           | - **Werkende PoC**  <br>- Concept Testplan                          |
| **W12** | 20.04–26.04 | **Test & Eval** | - Voer alle testscenario's uit (min. 3x per scenario).  <br>- Documenteer testresultaten (success rate, logs).  <br>- Identificeer bottlenecks in performance.                                                                                    | - Testdata & Logs  <br>- Performance Metrieken                      |
| **W13** | 27.04–03.05 | **Test & Eval** | - Optimaliseer performance (caching, parallellisme).  <br>- Valideer Modulariteit (bouw nieuwe workflow met bestaande blokken).  <br>- Voer Security Audit uit (Automated scans, RBAC tests).                                                     | - Optimalisatierapport  <br>- Security Scan Resultaten              |
| **W14** | 04.05–10.05 | **Test & Eval** | - Stel volledige documentatie op (Technisch & Gebruikers).  <br>- Schrijf Adviesrapport & Conclusie.  <br>- Bereid Eindpresentatie voor (Demo + Slides).                                                                                          | - **Technische Docs**  <br>- **Adviesrapport**                      |
| **W15** | 11.05–15.05 | **Test & Eval** | - Buffer voor laatste fixes.  <br>- Laatste afwerking deliverables.  <br>- **Eindpresentatie** (Live Demo + back-up video).                                                                                                                       | - **Eindpresentatie**                                               |
​

---

## Mijlpalen & Gates (Beslismomenten)

Deze gates zijn de momenten waarop goedkeuring van stakeholders (Xander in dit geval) vereist is voordat de volgende fase start.​

**Gate 1: Analyse goedkeuring (Eind W4)**

- **Beslisser:** Xander
    
- **Criteria om te slagen (Acceptatiecriteria):**
    
    -  **Volledigheid:** Document bevat alle secties (Context, Functioneel, Technisch).​
        
    -  **Technische diepgang:** Min. 3 orchestrators vergeleken op basis van >7 criteria.​
        
    -  **Secamo relevantie:** Analyse is specifiek aangepast aan Secamo use cases en klantcontext.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        
    -  **Architectuur:** Diagram is leesbaar, overzichtelijk en bevat alle componenten.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        

**Gate 2: Design Goedkeuring (Eind W6)**

- **Beslisser:** Xander
    
- **Criteria om te slagen (Acceptatiecriteria):**
    
    -  **Actiegericht:** Ontwerp is gedetailleerd genoeg om implementatie te starten zonder extra onderzoek.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        
    -  **Technische consistentie:** Naamgeving en configuratiestijlen zijn uniform compatibel.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        
    -  **Modulariteit:** Ontwerp past DRY-principes toe; modules/rollen zijn herbruikbaar.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        
    -  **Security by Design:** Bevat secrets management, RBAC en audit logging.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        

**Gate 3: PoC Gereedheid (Eind W11/W12)**

- **Criteria om te slagen:**
    
    -  **End-to-end functioneel:** Workflows kunnen getriggerd worden en werken naar behoren.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        
    -  **Minimaal 5 succesvolle runs:** Minstens 4 van de 5 runs moeten succesvol zijn (80% success rate) voordat een workflow als 'afgewerkt' geldt.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        
    -  **Idempotentie:** Workflows geven altijd hetzelfde resultaat na X aantal runs.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        
    -  **Error handling:** Bij falen of afwijzing volgt een "clean exit" met notificatie (geen crash).[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        

---

## Checklist Opleveringen (Deliverables)

**Fase 1: Analyse**

-  **Analyse Document**
    
    - _Definition of Done:_ Bevat Probleemstelling, Functionele Analyse en Technische Analyse (Vergelijking, Cloud keuze, High-level architectuur).​
        

**Fase 2: Design**

-  **Design / Ontwerp Document**
    
    - _Definition of Done:_ Bevat Architectuuroverzicht, gedetailleerde Workflow designs (BPMN/YAML), Module structuur, CI/CD design, Security/RBAC design en Infra topologie.​
        

**Fase 3: Implementatie**

-  **Werkende PoC-Infrastructuur**
    
    - _Definition of Done:_ Orchestrator gedeployed in AWS, Database operationeel, RBAC geconfigureerd, Healthcheck endpoint (`/api/health`) werkt.​
        
-  **Git Repository**
    
    - _Definition of Done:_ Correcte en consistente structuur volgens design.​
        
-  **CI/CD Pipeline**
    
    - _Definition of Done:_ Geautomatiseerde validatie (fmt, validate) bij elke commit.​
        
-  **Testplanning Document**
    
    - _Definition of Done:_ Teststrategie, tools en minstens 5 uitgeschreven testscenario's.[[ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/59944576/c513b525-30a9-4c56-bee7-5f3aa30872ff/planning_v1.0.pdf)]​
        

**Fase 4: Testing & Evaluatie**

-  **Testresultaten Document**
    
    - _Definition of Done:_ Overzichtstabel per scenario met metrics; uitvoering minimaal 3x gedocumenteerd.​
        
-  **Technische Documentatie**
    
    - _Definition of Done:_ Bevat installatiegids, configuratiereferentie, API docs en architectuurbeschrijving.​
        
-  **Gebruikersdocumentatie (Workflow handleidingen)**
    
    - _Definition of Done:_ Handleidingen voor workflows (triggers, approvals) en modules.​
        
-  **Adviesrapport & Conclusie**
    
    - _Definition of Done:_ Beantwoordt de onderzoeksvraag, valideert hypotheses en geeft concrete aanbevelingen aan Secamo.​
        
-  **Eindpresentatie**
    
    - _Definition of Done:_ Demo scenario's voorbereid (live + backup video), slides afgewerkt.​
        

**Aannames & TBDs:**

- **TBD:** "X workflows" en "X use cases" zijn placeholders in de PDF. _Aanname:_ Deze worden in W3 (Scope vastleggen) concreet gemaakt (bv. "3 workflows").
    
- **TBD:** "Cloud resource budget". _Actie:_ Bespreken met Xander in W4 zodra de architectuur vastligt.