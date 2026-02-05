## Projectoverzicht

### Context en doelstelling

Tijdens mijn stage bij Secamo realiseer ik een proof-of-concept (PoC) voor een modulaire process orchestrator in AWS. Het doel is om een orchestrator (bv. Camunda, Argo Workflows of AWS Step Functions) te selecteren, ontwerpen, implementeren en testen in een PoC AWS account, volledig geïntegreerd met IaC tooling (Terraform, Ansible), CI/CD pipelines en de bestaande Secamo-tooling.

Ik focus op:

- Orchestrator selectie en design
- AWS cloud infrastructuur
- IaC modules schrijven (Terraform modules, Ansible roles)
- CI/CD pipelines
- security/RBAC
- testing (incl. idempotentie, failure recovery, security)
- documentatie en eindpresentatie.

Deze projectplanning beschrijft wat ik ga doen, wanneer, welke deliverables ik oplever en welke acceptatiecriteria gelden per fase om iets als "afgewerkt" te kunnen beschouwen. Dit document gebruik ik als officieel planningsdocument richting mijn stagebegeleider (Xander) en mijn assessoren.

### Projectfasen, periodes en hoofdmijlpalen

Ik hanteer vier opeenvolgende fasen met vaste datum- en weekindeling: 


| Fase                 | Weeknrs | Datumbereik           | Hoofdmijlpaal                                                             |
| :------------------- | :------ | :-------------------- | :------------------------------------------------------------------------ |
| Analyse              | W1–W4   | 02.02.2026–28.02.2026 | Analyse document afgerond en goedgekeurd door Xander                      |
| Design               | W4–W6   | 02.03.2026–13.03.2026 | Design ontwerp document afgerond en goedgekeurd door Xander               |
| Implementatie        | W7–W13  | 16.03.2026–20.04.2026 | Werkende PoC (infra + orchestrator + workflows + CI/CD + IaC)             |
| Testing \& Evaluatie | W13–W16 | 20.04.2026–15.05.2026 | Testresultaten + volledige documentatie + adviesrapport + eindpresentatie |

Er zijn twee expliciete approval gates: na de analyse fase en na de design fase is goedkeuring van Xander vereist voordat ik verder kan. Zonder deze goedkeuring mag ik niet naar de volgende fase doorgaan. 

### Stakeholders

- **Ikzelf (stagiair / uitvoerder)** – verantwoordelijk voor analyse, design, implementatie, testing, documentatie en rapportering. 
- **Xander (stagebegeleider / opdrachtgever)** – inhoudelijke evaluatie, bijsturing waar nodig, zorgt ook dat de "Secamo-relevantie" goed verwerkt wordt in het PoC. Moet de analyse en het design formeel goedkeuren voordat ik respectievelijk kan starten met design en implementatie. 
- **Secamo** – bedrijf dat de PoC AWS account, toolingcontext en use cases levert; uiteindelijk moet de orchestrator inzetbaar en uitbreidbaar zijn in de Secamo-context. 

***

## Faseplanning in detail

### Fase 1: Analyse (W1–W4, 02.02.2026–28.02.2026)

#### Doelen

- **Technische requirements**
    - Identificeer huidige automatiseringsopportuniteiten en uitdagingen bij Secamo en klanten. 
    - Bepaal technische eisen voor een modulaire orchestrator (performance, schaalbaarheid, security). 
    - Breng de bestaande Secamo-tooling in kaart: huidig IaC-gebruik, CI/CD-pipelines, cloud footprint. 
- **Architectuurevaluatie \& toolselectie**
    - Uitvoeren van diepgaande technische vergelijkingen van minstens 3 orchestrators (Camunda, Zeebe, Argo Workflows, AWS Step Functions, open source of custom). 
    - Evalueren op technische criteria: modulariteit, Git-integratie, IaC-native support, security, schaalbaarheid, licentiemodel. 
    - Vergelijken en documenteren van architectuursprincipes (event-driven vs workflow-based, synchrone vs asynchrone executions, etc.). 
- **Functionele scope vastleggen**
    - Bepalen welke use cases de PoC moet demonstreren (bv. VM provisioning, deployment pipeline, automated compliance checks). 
    - Definiëren van minimale MVP functionaliteit en eventueel bijkomende “nice to have” features. 
    - Opstellen van milestones voor minimale doelstellingen. 
- **Cloud Platform \& Tech Stack**
    - Beslissen over cloud provider –-> AWS. 
    - Beslissen welke tools of versiebeheersystemen gebruikt zullen worden (bv. Terraform, Ansible, GitHub, custom scripts, etc.). 
    - Cloud resource budget plannen, te bespreken met Xander eens het zover is. 


#### Key Deliverables

- **Analyse document** met minimaal de volgende onderdelen: 
    - Probleemstelling \& context. 
    - Functionele analyse. 
    - Technische analyse. 
    - Orchestrator platform vergelijking. 
    - Cloud platform keuze – AWS bij Secamo (Datacenter United, huren dus racks in een datacenter). 
    - Technische architectuur (high-level, schematische architectuurdiagram). 
    - (optioneel? of nodig ?) Risicoanalyse


#### Acceptatiecriteria (Analyse)

| Criterium                  | Vereist                                                                                                                 |
| :------------------------- | :---------------------------------------------------------------------------------------------------------------------- |
| Volledigheid               | Alle nodige secties aanwezig in het document.                                                                           |
| Technische diepgang        | Minstens 3 orchestrators vergelijken op basis van +- 7 criteria.                                                        |
| Secamo relevantie          | Analyse is aangepast aan Secamo hun bedrijscontext, use cases en klantcontext; Xander zijn feedback is hierin verwerkt. |
| Architectuur duidelijkheid | Architectuurdiagram bevat alle componenten, is leesbaar en overzichtelijk.                                              |
| Goedkeuring                | Goedkeuring van Xander is een vereiste voordat aan de volgende fase kan gestart worden.                                 |

#### Mijlpaal \& goedkeuringsmoment

- **Mijlpaal:** Analyse document afgerond, inclusief orchestrator vergelijking, functionele scope en high-level architectuur. 
- **Goedkeuring nodig:** Xander moet het analyse document expliciet goedkeuren voordat ik effectief met de design fase start. Dit spreekt voor zich aangezien ik deze tool bouw voor secamo, en de vereisten en use cases dus moeten aangepast zijn aan Secamo.

***

### Fase 2: Design (W4–W6, 02.03.2026–13.03.2026)

#### Doelen

- **Gedetailleerde architectuur uitwerken**
    - Technische architectuur met alle componenten zoals beschreven in de analyse (orchestrator, Git, CI/CD, cloud resources, IaC tooling). 
    - Data flows tussen componenten documenteren, inclusief triggers, events, condities, waar van toepassing. (hangt ook af van keuze processor in fase 1)
    - Deployment topologie definiëren: AWS resource topologie, netwerken, communicatie, segmentatie, etc. 
- **Workflow design finaliseren**
    - Voor elke use case besproken in het analyse document een workflow stap-voor-stap uitwerken. 
    - Beslissingspunten, approval gates, Human in the Loop (HiL), error handling definiëren. 
    - BPMN-diagrammen (Camunda) of YAML-structuur (Argo) schetsen. 
- **Modulariteit ontwerpen**
    - Terraform module samenstelling bepalen zodat elke module een eigen verantwoordelijkheid, functie of doel heeft. 
    - Scope binnen deze modulariteitsvereisten: folderstructuur, naming conventions, input/output variabelen. 
    - Ansible role architectuur uitwerken (roles, playbooks, inventory strategy). 
    - Herbruikbaarheid duidelijk documenteren: welke onderdelen zijn generiek (=algemeen toepasbaar) en welke zijn specifiek. 
- **Integratiestrategie vastleggen**
    - Git workflow (branching strategie, PR process, tagging voor release). 
    - CI/CD pipeline design (triggers, validation steps, deployment stages). 
    - Orchestrator IaC integratie: hoe roept de orchestrator Terraform/Ansible aan en wanneer? 
- **Security \& compliance ontwerp**
    - Secrets management strategie (bv. Keeper). 
    - Reliability/availability eisen voor de PoC. 
    - Monitoring \& Observability design


#### Key Deliverables

- **Design ontwerp document** met volgende onderdelen: 
    - Technische vereisten \& constraints (modulariteit, schaalbaarheid, security, performance)
    - Risicoanalyse
    - Architectuuroverzicht
    - Workflow designs: voor elke use case gedefinieerd in de analyse fase een uitgeschreven workflow
    - Module code structuur
    - Git \& CI/CD workflow
    - Security \& RBAC design
    - Infrastructure design
    - Non-functional requirements (metrics m.b.t. performantie, betrouwbaarheid, scalability en maintainability)
    - Technische diagrammen: architectuurdiagram, workflow diagram, netwerk-topologie, CI/CD pipeline diagram
    - Module design:
        - Terraform modules – per module variabelen, dependencies, functie, gebruik. 
        - Ansible roles – per rol functie, task-overzicht, variabelen, handlers. 
    - Samenvatting met: 
        - Gekozen orchestrator (Camunda, Argo, AWS Step Functions, custom). 
        - Uitleg en onderbouwing van de gemaakte keuze
        - Gekozen cloud platform – AWS
        - Licentiekosten


#### Acceptatiecriteria (Design)

| Criterium               | Vereist                                                                                                                          |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------- |
| Volledigheid            | Alle bovenstaand vermelde onderdelen aanwezig in het document.                                                                   |
| Actiegericht            | Het ontwerpdocument is voldoende gedetailleerd zodat implementatie kan starten **zonder** extra designonderzoek of beslissingen. |
| Technische consistentie | Alle componenten in de architectuur zijn compatibel en op dezelfde manier geconfigureerd (bv. naming conventions).               |
| Modulariteit            | Herbruikbare modules/roles, DRY principe toegepast.                                                                              |
| Security by Design      | Secrets management, RBAC, audit logging zijn uitgewerkt.                                                                         |
| Goedkeuring             | Goedkeuring door Xander is vereist voordat ik aan de effectieve implementatie fase kan beginnen.                                 |

#### Mijlpaal \& goedkeuringsmoment

- **Mijlpaal:** Design ontwerp document afgerond, inclusief alle technische en security-ontwerpen. 
- **Goedkeuring vereist:** Xander moet het Design document expliciet goedkeuren voordat ik met Implementatie mag starten. 

***

### Fase 3: Implementatie (W7–W13, 16.03.2026–20.04.2026)

> Opmerking: in deze fase start ik al met iteratieve testing en debugging; formele testplanning en uitgebreide evaluatie vallen in de volgende fase. 

#### Doelen

- **Cloud infra opzetten in AWS**
    - AWS omgeving provisioneren, al dan niet met Terraform (afhankelijk van de Analysefase): VPC, subnets, security groups, IAM roles, Secrets Manager. 
- **Orchestrator deployen**
    - Gekozen orchestrator (Camunda/Argo in EKS/ECS of AWS Step Functions) deployen in AWS. 
    - Monitoring \& logging configureren (CloudWatch, CloudWatch Logs). 
- **Git repository \& CI/CD**
    - Repository initialiseren met folderstructuur volgens het ontwerp uit de vorige fase. 
    - Branching strategie implementeren (main/develop/feature branches). 
    - CI/CD pipeline bouwen (GitHub Actions, AWS CodePipeline, eventueel Aikido). 
- **IaC modules schrijven \& standalone testen**
    - Terraform modules schrijven. 
    - Ansible roles bouwen. 
    - Modules standalone testen zonder orchestrator (zie acceptatiecriteria). 
- **Orchestrator workflows implementeren**
    - X aantal workflows bouwen in de orchestrator; X wordt in de analysefase bepaald door de use cases. (Nader te bepalen in analyse) 
    - Integratie met Terraform/Ansible via API-calls of workers. 
    - Custom scripting integraties (Python/PowerShell/AWS Step Functions scripts voor orchestration workers, API clients voor orchestrator). 
    - Notificatie integratie (e-mail, Slack, SNS) voor approvals, eventueel ook als HiL-oplossing. 
- **Iteratief testen \& debuggen**
    - Workflows end-to-end draaien. 
    - Performance issues oplossen (timeouts, resource constraints). 
    - Edge cases implementeren (error handling, retries, escalatie). 


#### Key Deliverables

- **Werkende PoC-infrastructuur en orchestrator platform**
    - Gekozen orchestrator gedeployed in AWS. 
    - Database operationeel
    - RBAC geconfigureerd
    - Healthcheck endpoint werkt (bv. `/api/health`)
- **Git repository \& CI/CD**
    - Correcte en consistente structuur. 
    - CI/CD pipeline met geautomatiseerde validatie op elke commit (bv. `terraform fmt`, `terraform validate`, `ansible-lint`, securityscans met Checkov/KICS/Aikido). 
- **IaC \& workflows**
    - Terraform modules en Ansible roles die standalone werken en in orchestrator workflows gebruikt kunnen worden
    - X aantal werkende workflows (waar X in de Analysefase bepaald werd). (Nader te bepalen in Analyse) 

#### Acceptatiecriteria (Implementatie)

| Criterium               | Vereist                                                                                             |
| :---------------------- | :-------------------------------------------------------------------------------------------------- |
| End-to-end functioneel  | Workflows kunnen getriggerd worden en werken naar behoren                                           |
| Approval gate werkt     | Workflow pauzeert bij approval; kan approved/rejected worden                                        |
| Herbruikbare modules    | Alle modules werken ook als standalone en kan in andere workflows worden gebruikt.                  |
| Idempotentie            | Workflows of playbooks moeten altijd hetzelfde resultaat geven na een aantal keren gerund te worden |
| Error handling          | Failure of approval rejection --> clean exit met notificatie (geen crash)                           |
| Min. 5 succesvolle runs | Workflow wordt 5 keer gedraaid waarvan minsten 4 runs (80%) successvol moet zijn gelukt.            |


***

### Fase 4: Testing \& Evaluatie (W13–W16, 20.04.2026–15.05.2026)

> Opmerking: deze fase bevat zowel testing, optimalisatie als evaluatie. 

#### Doelen

- **Uitgebreide testing \& validatie**
    - Alle testscenarios uit het testplan uitvoeren (zie Implementatiefase). 
    - Testresultaten documenteren (success rate, execution times, error logs). 
    - Edge cases en failure scenarios testen. 
    - Performance metrics verzamelen. 
- **Performance optimalisatie**
    - Bottlenecks identificeren. 
    - Optimalisaties doorvoeren (parallelisme tuning, caching, resource allocation). 
- **Modulariteit \& herbruikbaarheid valideren**
    - Tonen dat modules herbruikbaar zijn, bv. door een nieuwe workflow op te bouwen uit bestaande modules als “bouwstenen” (optioneel). 
    - Code re-usage percentage berekenen (waar relevant). 
    - Refactoring uitvoeren waar nodig (DRY principe). 
- **Security \& compliance audit**
    - Security scans uitvoeren (geautomatiseerd). 
    - RBAC valideren door met een unauthorized account toegang te proberen krijgen. 
- **Volledige documentatie opstellen**
    - Technische documentatie (architectuur, installatie, configuratie, API-referentie). 
    - Gebruikersdocumentatie (workflows, triggers, stappen, approvals; modules gebruik, doel, functie, variabelen). 
    - Best practices gids. 
- **Adviesrapport \& eindpresentatie voorbereiden**
    - Adviesrapport met conclusie. 
    - Demo scenarios voorbereiden (live + backup video). 
    - Presentatieslides maken. 
    - Technische problemen/fixes documenteren. 
    - Persoonlijke reflectie voorbereiden. 


#### Key Deliverables

- **Testresultaten document**
    - Testuitvoering overzicht (tabel per scenario met metrics). 
    - Testplanning document met: 
        - Teststrategie. 
        - Testtypen (unit, integration, performance, security, idempotency, failure recovery). 
        - Testomgevingen (PoC AWS account, isolated test EC2 instance). 
        - Testing tools. 
        - Testscenarios – deze kunnen pas na de Analysefase concreet uitgeschreven worden; minimaal 5 testscenarios voorzien. (Nader te bepalen in Analyse) 
        - Successcriteria – TODO. (Nader te bepalen in Testing \& Evaluatie) 
        - Testscenario details – TODO. (Nader te bepalen in Testing \& Evaluatie) 
- **AWS Resources** (illustratief opgesomd in de planning): 
    - VPC. 
    - Subnets (orchestrator subnet, workload subnet). 
    - Orchestrator hosting. 
    - AWS Secrets Manager. 
    - S3 bucket. 
    - CloudWatch Log Group. 
    - IAM Roles. 
- **Technische documentatie**
    - Architectuur documentatie: 
        - Diagrammen (componenten, data flows). 
        - Component beschrijvingen (orchestrator, modules, playbooks, workflows, etc.). 
        - Technology stack. 
        - Ontwerpbeslissingen en argumentatie. 
    - Installatie \& deployment guide (stap-voor-stap instructies). 
    - Configuratie referentie: 
        - Terraform variabelen. 
        - Ansible variabelen. 
        - Orchestrator configuratie. 
        - AWS resources. 
    - Workflow handleidingen en API-documentatie (indien custom scripts/workers). 
    - Best practices guide. 
    - Troubleshooting \& FAQ. 
- **Conclusie \& adviesrapport**
    - Beantwoording onderzoeksvraag/technologievraag. 
    - Hypothese validatie met data. 
    - Bevindingen \& inzichten: 
        - Wat werkt goed? Wat werkt minder goed? 
        - Waar had ik problemen mee? Hoe heb ik deze opgelost? 
        - Beperkingen van de PoC (wat werkt niet dat wel zou moeten werken?). 
    - Aanbevelingen aan Secamo: 
        - Hoe kan Secamo deze process orchestrator gebruiken om bedrijfsprocessen efficiënter te laten verlopen? 
        - Hoe kan Secamo deze orchestrator uitbreiden met nieuwe workflows? 
        - Hoe kan de output/productiviteit van werknemers stijgen door juist gebruik te maken van deze orchestrator? 


#### Acceptatiecriteria (Testing \& Evaluatie – functioneel \& technisch)

| Criterium | Vereist |
| :-- | :-- |
| End-to-end functioneel | Workflows kunnen getriggerd worden en werken naar behoren.  |
| Approval gate werkt | Workflow pauzeert bij approval en kan approved/rejected worden.  |
| Herbruikbare modules | Alle modules werken ook standalone en kunnen in andere workflows gebruikt worden.  |
| Idempotentie | Workflows of playbooks moeten altijd hetzelfde resultaat geven na X aantal keren na elkaar gerund te zijn.  |
| Error handling | Failure of approval rejection resulteert in een “clean exit” met notificatie; geen crashes.  |
| Minimaal 5 succesvolle runs | Workflow wordt 5 keer gedraaid en moet minstens 4 keer succesvol zijn voordat ik een workflow als afgewerkt beschouw.  |

#### Acceptatiecriteria (Testing \& Evaluatie – documentatie \& evaluatie)

| Criterium | Vereist |
| :-- | :-- |
| Testuitvoering compleet | Alle testscenarios uit het testplan minimaal 3× uitgevoerd en resultaten gedocumenteerd.  |
| Failure analyse compleet | Alle failures die ik ben tegengekomen zijn gefixt en gedocumenteerd.  |
| Modulariteit bewezen | Bewezen door het bouwen van een nieuwe workflow met bestaande modules.  |
| Reproduceerbaarheid | Iemand anders kan de PoC volledig reproduceren op basis van mijn einddocumentatie.  |
| Best practices | Best practices concreet en helder gedocumenteerd met codevoorbeelden waar relevant.  |
| Concrete aanbevelingen | Heldere uitleg over hoe deze orchestrator gebruikt kan worden in een bedrijfscontext.  |


***

## Week-voor-week tijdlijn (Gantt-achtige tabel)

Onderstaande tabel toont mijn weekplanning met fasen, activiteiten, artefacten, afhankelijkheden en acceptatiechecks. W4 en W13 zijn overgangsweken zoals eerder beschreven.


| Week | Datumbereik      | Fase(n)                                         | Concrete activiteiten                                                                                                                                                                                | Outputs / Artefacten                                                                                                           | Afhankelijkheden                                      | Acceptatiechecks                                                        |
| :--- | :--------------- | :---------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------- | :---------------------------------------------------------------------- |
| W1   | 02.02–08.02.2026 | Analyse                                         | Interviews/overleg met Xander en Secamo; huidige automatiseringsopportuniteiten en uitdagingen in kaart brengen.  Bestaande Secamo-tooling (IaC, CI/CD, cloud footprint) analyseren.                 | Eerste ruwe notities voor Analyse Document; lijst met bestaande tooling \& processen.                                          | Beschikbaarheid Xander en relevante Secamo-contacten. | N.v.t. (verzamelfase).                                                  |
| W2   | 09.02–15.02.2026 | Analyse                                         | Orchestrator shortlist opstellen (Camunda, Zeebe, Argo Workflows, AWS Step Functions, evt. custom).  Vergelijking uitwerken op 7+ criteria.                                                          | Tabel orchestratorvergelijking; concept-architectuurschetsen (event-driven vs workflow-based, synchroon/asynchroon).           | Informatie uit W1; toegang tot documentatie/tools.    | Technische diepgang-criterium opzetten (≥3 orchestrators, ≥7 criteria). |
| W3   | 16.02–22.02.2026 | Analyse                                         | Functionele scope vastleggen (use cases, MVP vs nice-to-have).  Milestones voor minimale doelstellingen definiëren.  Cloud platform \& tech stack beslissen (AWS, Terraform, Ansible, GitHub, etc.). | Conceptversie Analyse Document met probleemstelling, functionele \& technische analyse, scope, orchestratorvergelijking.       | Resultaten orchestratorvergelijking (W2).             | Volledigheid van alle secties bijna gehaald.                            |
| W4   | 23.02–28.02.2026 | Analyse → Design (overgang)                     | Analyse Document finaliseren, architectuurdiagram uitwerken.  Feedback van Xander verwerken.  Voorbereidende designschetsen (workflow-ideeën, module-structuur) opzetten.                            | Definitieve versie Analyse Document incl. architectuurdiagram.  Eerste ruwe schetsen voor Design.                              | Analyse-inhoud uit W1–W3.                             | **Goedkeuringsgate 1:** Analyse Document goedgekeurd door Xander.       |
| W5   | 02.03–08.03.2026 | Design                                          | Gedetailleerde technische architectuur uitwerken.  Data flows, deployment topologie en netwerksegmentatie beschrijven.  Workflow designs per use case uitwerken (BPMN/YAML).                         | Draft Design document met architectuuroverzicht, workflow designs en eerste module/codestucturen.                              | Goedgekeurde Analyse.                                 | Controleren op volledigheid en technische consistentie.                 |
| W6   | 09.03–13.03.2026 | Design                                          | Module design (Terraform modules, Ansible roles) detailleren.  Git workflow, CI/CD pipeline design, orchestrator–IaC integratie en Security by Design afronden.                                      | Definitief Design Ontwerp document incl. alle diagrammen, non-functional requirements, security \& RBAC design.                | Resultaten W5.                                        | **Goedkeuringsgate 2:** Design door Xander goedgekeurd.                 |
| W7   | 16.03–22.03.2026 | Implementatie                                   | AWS basisinfra provisioneren (VPC, subnets, security groups, IAM roles, Secrets Manager).  Orchestrator basisdeployment opzetten.                                                                    | Werkende AWS basisomgeving; eerste orchestratordeployment (zonder volledige workflows).                                        | Goedgekeurd Design; toegang tot PoC AWS account.      | Basis-healthcheck endpoint online.                                      |
| W8   | 23.03–29.03.2026 | Implementatie                                   | Monitoring \& logging (CloudWatch, CloudWatch Logs) configureren.  Git repository initialiseren met folderstructuur.  Branching strategie implementeren.                                             | Git repository met juiste structuur; basis logging/monitoring ingericht.                                                       | Infra en orchestrator uit W7.                         | CI/CD skeleton klaar (minimale pipeline).                               |
| W9   | 30.03–05.04.2026 | Implementatie                                   | CI/CD pipelines uitbouwen (GitHub Actions, AWS CodePipeline, evt. Aikido).  IaC modules schrijven (Terraform modules).                                                                               | Werkende CI/CD pipeline met automatische `fmt/validate` en securityscans (Checkov/KICS/Aikido).  Eerste set Terraform modules. | Git repo \& infra uit W7–8.                           | Modules kunnen standalone draaien (basischeck).                         |
| W10  | 06.04–12.04.2026 | Implementatie                                   | Ansible roles bouwen.  Verdere IaC modules uitwerken.  Standalone tests van modules zonder orchestrator.                                                                                             | Reeks Ansible roles en bijkomende Terraform modules; testresultaten van standalone runs.                                       | Ontwerpdocument module-architectuur.                  | Idempotentie en DRY op moduleniveau beginnen te bewaken.                |
| W11  | 13.04–19.04.2026 | Implementatie                                   | Orchestrator workflows implementeren (X aantal workflows, bepaald in Analyse).  Integratie met Terraform/Ansible via API-calls of workers.  Notificatie integraties (email, Slack, SNS).             | Eerste set end-to-end workflows die infra aansturen via IaC en notificaties versturen.                                         | Beschikbare modules \& orchestratorplatform.          | Bulk workflows werken end-to-end in basisvorm.                          |
| W12  | 20.04–20.04.2026 | Implementatie                                   | Iteratief testen \& debuggen; performance issues (timeouts, resource constraints) oplossen.  Edge cases en error handling (retries, escalaties) toevoegen.                                           | Verbeterde workflows met robuuste error handling en betere performance.                                                        | Volledige workflowimplementatie.                      | Minstens 4/5 runs per workflow succesvol.                               |
| W13  | 20.04–26.04.2026 | Implementatie → Testing \& Evaluatie (overgang) | Laatste implementatie-fixes; voorbereiden testplan-detail (min. 5 scenarios).  Opstarten formele testuitvoering.                                                                                     | Stabiele PoC met X werkende workflows; uitgewerkt testplan.                                                                    | Volledige infra \& workflows.                         | Einde Implementatie: alle key deliverables aanwezig.                    |
| W14  | 27.04–03.05.2026 | Testing \& Evaluatie                            | Uitvoeren van testscenarios (min. 3× per scenario).  Resultaten loggen (success rate, execution times, error logs).                                                                                  | Testresultaten document (tabel per scenario).                                                                                  | Testplan uit W13.                                     | Testuitvoering-compleet-criterium nastreven.                            |
| W15  | 04.05–10.05.2026 | Testing \& Evaluatie                            | Performanceoptimalisatie en bottleneckanalyse.  Herbruikbaarheid aantonen door nieuwe workflow op te bouwen uit bestaande modules.                                                                   | Geoptimaliseerde workflows; bewijs van modulariteit (nieuwe workflow uit modules).                                             | Testresultaten \& PoC-implementatie.                  | Modulariteit bewezen; performance-metrics verzameld.                    |
| W16  | 11.05–15.05.2026 | Testing \& Evaluatie                            | Security \& compliance audit (geautomatiseerde scans, RBAC testen).  Volledige documentatie afronden.  Adviesrapport \& eindpresentatie (slides, demo, video, reflectie) finaliseren.                | Definitieve technische documentatie, best practices guide, troubleshooting \& FAQ, adviesrapport, slides, demo(materiaal).     | Alle vorige fases succesvol afgerond.                 | Alle documentatie- en evaluatie-acceptatiecriteria gehaald.             |


***

## Deliverables checklist

Onderstaande checklist bundelt alle deliverables uit de planning met hun fase, geplande opleverdatum, status (nu: Gepland) en Definition of Done (afgeleid uit de acceptatiecriteria).


| Deliverable                                | Fase                 | Geplande opleverdatum (einde week) | Status  | Definition of Done                                                                                                                                                                            |
| :----------------------------------------- | :------------------- | :--------------------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Analyse Document                           | Analyse              | W4 (28.02.2026)                    | Gepland | Alle secties aanwezig; min. 3 orchestrators vergeleken op 7 criteria; Secamo-relevantie verwerkt; architectuurdiagram duidelijk; goedkeuring door Xander.                                     |
| Orchestratorvergelijkingstabel             | Analyse              | W3–W4                              | Gepland | Minimaal 3 orchestrators, minimaal 7 evaluatiecriteria, duidelijk gedocumenteerde voor- en nadelen.                                                                                           |
| High-level architectuurdiagram             | Analyse              | W4                                 | Gepland | Diagram bevat alle relevante componenten, leesbaar en overzichtelijk.                                                                                                                         |
| Design Ontwerp document                    | Design               | W6 (13.03.2026)                    | Gepland | Alle secties aanwezig; detailgraad voldoende om implementatie zonder extra designbeslissingen te starten; consistent; modulariteit \& security by design uitgewerkt; goedgekeurd door Xander. |
| Workflow designs (BPMN/YAML)               | Design               | W5–W6                              | Gepland | Voor elke use case gedefinieerd in Analyse een uitgewerkte workflow met beslissingspunten, approval gates, HiL en error handling.                                                             |
| Module design (Terraform, Ansible)         | Design               | W6                                 | Gepland | Voor elke module/rol variabelen, dependencies, functie, gebruik en handlers beschreven; DRY principe ingedacht.                                                                               |
| Git \& CI/CD ontwerp                       | Design               | W6                                 | Gepland | Branching strategie, PR-process, tagging, pipeline-stappen beschreven en afgestemd op IaC/orchestrator-integratie.                                                                            |
| AWS infra (VPC, subnets, SG, IAM, Secrets) | Implementatie        | W7–W8                              | Gepland | Basisinfra draait in PoC AWS account; security groups en IAM roles correct geconfigureerd; Secrets Manager beschikbaar.                                                                       |
| Gedeployde orchestrator in AWS             | Implementatie        | W7–W8                              | Gepland | Orchestrator draait; healthcheck endpoint werkt; basisconnectiviteit getest.                                                                                                                  |
| Monitoring \& logging setup                | Implementatie        | W8                                 | Gepland | CloudWatch metrics en logs beschikbaar; kerncomponenten worden gemonitord.                                                                                                                    |
| Git repository met structuur               | Implementatie        | W8                                 | Gepland | Repository bevat afgesproken folderstructuur volgens design.                                                                                                                                  |
| CI/CD pipeline                             | Implementatie        | W9                                 | Gepland | Pipelines voeren automatisch `terraform fmt/validate`, `ansible-lint` en securityscans (Checkov/KICS/Aikido) uit bij commits.                                                                 |
| Terraform modules                          | Implementatie        | W9–W10                             | Gepland | Modules zijn standalone testbaar, volgen naming conventions en voldoen aan DRY.                                                                                                               |
| Ansible roles                              | Implementatie        | W10                                | Gepland | Rollen zijn standalone testbaar en sluiten aan op orchestrator-workflows.                                                                                                                     |
| X werkende workflows                       | Implementatie        | W11–W13                            | Gepland | X workflows (X bepaald in Analyse) draaien end-to-end; minimaal 4/5 runs per workflow succesvol.  (X nader te bepalen in Analyse)                                                             |
| Testplanning document                      | Testing \& Evaluatie | W13–W14                            | Gepland | Bevat teststrategie, testtypen, omgevingen, tools en minstens 5 testscenarios.  (scenarios nader te bepalen)                                                                                  |
| Testresultaten document                    | Testing \& Evaluatie | W14–W15                            | Gepland | Voor elk scenario minstens 3 testuitvoeringen met metrics; failures en success rates gedocumenteerd.                                                                                          |
| AWS resource-overzicht                     | Testing \& Evaluatie | W15–W16                            | Gepland | Overzicht van alle PoC-resources (VPC, subnets, Secrets Manager, S3, CloudWatch, IAM, etc.).                                                                                                  |
| Technische documentatie                    | Testing \& Evaluatie | W16                                | Gepland | Architectuur, componentbeschrijvingen, installatie- en configuratiehandleidingen, workflows, API’s en designbeslissingen volledig gedocumenteerd.                                             |
| Best practices guide                       | Testing \& Evaluatie | W16                                | Gepland | Concrete best practices met codevoorbeelden waar relevant.                                                                                                                                    |
| Troubleshooting \& FAQ                     | Testing \& Evaluatie | W16                                | Gepland | Overzicht van veelvoorkomende problemen en oplossingen.                                                                                                                                       |
| Adviesrapport                              | Testing \& Evaluatie | W16                                | Gepland | Onderzoeksvraag beantwoord; hypothese gevalideerd; bevindingen en aanbevelingen voor Secamo uitgewerkt.                                                                                       |
| Eindpresentatie (slides + demo + video)    | Testing \& Evaluatie | W16                                | Gepland | Slides, live-demo-scripts en backup-video aanwezig; persoonlijke reflectie voorbereid.                                                                                                        |


***

## Mijlpalen \& goedkeuringsgates

Hier vat ik alle formele mijlpalen en approval gates samen.

1. **Mijlpaal 1 – Analyse afgerond**
    - Deliverable: Analyse Document (incl. orchestratorvergelijking, functionele scope, high-level architectuur). 
    - Gate:
        - Analyse Document voldoet aan alle acceptatiecriteria (volledigheid, technische diepgang, Secamo-relevantie, architectuur duidelijkheid). 
        - **Goedkeuringsgate:** Xander keurt het Analyse Document goed. 
2. **Mijlpaal 2 – Design afgerond**
    - Deliverable: Design Ontwerp document (incl. workflow designs, module design, Git/CI/CD, security, non-functional requirements). 
    - Gate:
        - Document voldoet aan alle acceptatiecriteria (volledigheid, actiegerichtheid, technische consistentie, modulariteit, Security by Design). 
        - **Goedkeuringsgate:** Xander keurt het Design document goed. 
    - Zonder deze goedkeuring start ik de Implementatiefase niet.
3. **Mijlpaal 3 – Implementatie gereed (PoC werkend)**
    - Deliverables:
        - Werkende PoC-infrastructuur en orchestrator platform. 
        - Git repository, CI/CD pipeline, IaC modules en X werkende workflows. 
    - Gate (informeler, maar noodzakelijk voor volgende fase):
        - Alle kerncomponenten draaien end-to-end zodat formele testing zinvol is. 
4. **Mijlpaal 4 – Testing \& Evaluatie afgerond (project klaar)**
    - Deliverables:
        - Testplanning document, testresultaten document, volledige technische documentatie, best practices guide, troubleshooting \& FAQ, adviesrapport en eindpresentatie. 
    - Gate:
        - Alle acceptatiecriteria rond testuitvoering, failure analyse, modulariteit, reproduceerbaarheid, best practices en concrete aanbevelingen zijn gehaald. 

***

## Risico’s \& aandachtspunten

### TBD-onderdelen en hoe ik ze concretiseer

- **“X aantal workflows”** – het exacte aantal wordt in de Analysefase bepaald op basis van de gekozen use cases. 
    - Plan: in W2–W3 definieer ik de concrete use cases en bepaal ik X als minimaal aantal representatieve workflows (bv. mix van infra, CI/CD en compliance). Ik leg dit vast in het Analyse Document en hergebruik het in Design en Implementatie. 
- **Testscenarios \& successcriteria (TODO)** – in de planning staat expliciet dat testscenarios en successcriteria later worden ingevuld, met minimaal 5 testscenarios. 
    - Plan: in W13–W14 stel ik, gebaseerd op de geïmplementeerde workflows, minimaal 5 testscenarios op en vul ik de TODO-velden voor successcriteria en scenario-details in het testplan. 


### Overlappingen in fasen (W4, W13)

- **Week 4 (Analyse ↔ Design)** – risico op context-switching en onvolledige Analyse bij te vroege start van Design. 
    - Mitigatie: ik zorg ervoor dat de Analyse-inhoud (inclusief orchestratorkeuze en scope) inhoudelijk “af” is voordat ik substantiële Design-artefacten maak. W4 gebruik ik als afrondings- en overgangsweek, met de formele goedkeuring van Xander als harde gate. 
- **Week 13 (Implementatie ↔ Testing)** – risico dat implementatiewerk doorschuift en testtijd in het gedrang komt. 
    - Mitigatie: ik plan conservatief in W11–W12 zodat W13 vooral stabilisatie en testvoorbereiding bevat. Als er toch uitloop is, hou ik strikte prioriteit op het “werkend krijgen” van de kern-workflows vóór extra nice-to-have features. 


### Afhankelijkheid van Xander’s goedkeuring

- De twee formele approval gates (na Analyse en na Design) zijn kritieke afhankelijkheden. 
- Risico: vertraging in feedback of goedkeuring kan de start van de volgende fase uitstellen.
- Mitigatie: ik plan regelmatig overleg met Xander, lever tussentijdse versies aan en zorg dat het Analyse- en Design document al vóór de laatste week van de fase in een reviewbare staat zijn.


### Scope \& complexiteit

- De scope bevat meerdere complexe onderdelen tegelijk (orchestrator, IaC, CI/CD, security, testing). 
- Risico: overschatting van wat in de gegeven tijd haalbaar is.
- Mitigatie: ik volg de MVP/nice-to-have scheiding uit de Analysefase strikt en focus in eerste instantie op een robuuste, maar beperkte set workflows en modules. Extra workflows en optimalisaties pak ik alleen op als er ruimte is na het behalen van de kernacceptatiecriteria. 

***

Met dit projectplan leg ik vast wat ik ga doen, wanneer ik het doe, welke deliverables ik oplever en hoe succes gemeten wordt. Alle doelen, key deliverables en acceptatiecriteria zijn rechtstreeks gebaseerd op de inhoud van het document *planning_v1.0.pdf* en de timing is afgestemd op de opgegeven weken en datums. 


