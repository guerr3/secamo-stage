# Uitgebreide Analyse en Strategisch Ontwerp van een Modulaire Proces-Orchestrator

**Auteur:** Warre Gehre (Stagiair Cybersecurity Engineer)
**Periode:** Februari 2026
**Betreft:** Analysefase (Week 1-4) - Stage Secamo

## 1. Projectcontext en Strategische Doelstellingen

Als stagiair Cybersecurity Engineer bij Secamo (e-Solutions Services NV) heb ik de opdracht gekregen om een Proof of Concept (PoC) te realiseren voor een **modulaire** proces-orkestrator binnen de AWS-omgeving. De scope van deze opdracht reikt verder dan enkel technische implementatie, het gaat om het fundamenteel herzien van hoe Secamo operationele processen schaalt, beveiligt en beheert. Secamo positioneert zich in de markt als een "Trusted Advisor", waarbij de koppeling tussen strategie en uitvoering centraal staat. Neem daar het feit bij dat Secamo een scale up is die snel aan het groeien is, is het noodzakelijk dat interne (repetitieve) processen niet afhankelijk zijn van ad-hoc (=specifieke, niet algemene) handelingen, maar gestroomlijnd worden via een schaalbaar en auditbaar platform dat de process orchestrator voorstelt.

Het primaire doel van dit project is het selecteren, ontwerpen, implementeren en testen van een orchestrator (zoals Camunda, Argo Workflows, N8N of AWS Step Functions). Deze orchestrator moet fungeren als de centrale "backbone" die verschillende systemen (van ticketing in Jira tot infrastructuurbeheer in AWS) met elkaar verbindt. De focus ligt hierbij op het modulaire aspect, verminderen van menselijke fouten, het verhogen van de efficiëntie en het waarborgen van compliance door middel van "idempotentie-by-design" en strikte audit trails. Deze strikte audit trails zijn heel belangrijk om achteraf te kunnen checken waarom een bepaalde workflow is uitgevoerd door de orchestrator en wat het resultaat hiervan was.

In deze eerste fase, de analysefase (Week 1–4), ligt mijn focus op het diepgaand in kaart brengen van de huidige situatie (tools, processen), het definiëren van de technische vereisten en het valideren van de scope met mijn stagebegeleider Xander. De documentatie die ik tot nu toe heb verzameld, waaronder interview-inventarissen, feedback-sessies en requirement-lijsten, vormt het fundament voor het latere design.

### Volgende Stappen

- **Validatie Bedrijfsdoelen:** Ik moet expliciet verifiëren of de doelstelling om Secamo's rol als "Trusted Advisor" te versterken direct meetbaar gemaakt kan worden via de PoC (bijvoorbeeld door verkorte doorlooptijden van audits).
    
- **Stakeholder Mapping:** Een gedetailleerder overzicht van welke specifieke teams (buiten SOC en Cloud Engineering) direct beïnvloed zullen worden door de PoC.
    

---

## 2. Analyse van de Functionele Scope: Multi-Departementale Integratie

Uit mijn initiële onderzoek in [[01_ANALYSE_RESEARCH.md]] en de daaropvolgende verdieping in [[01_ANALYSE_RESEARCH_EXTRA.md]] is gebleken dat de scope van de orkestrator breder moet zijn dan aanvankelijk gedacht. Waar traditionele orkestratieprojecten zich vaak beperken tot CI/CD-pipelines en technische IT-taken, is het voor Secamo van cruciaal belang om een multi-departementale benadering te hanteren. Wat hiermee bedoelt wordt is niet dat ik een process orchestrator zal ontwikkelen die "ineens alles kan", maar eerder een fundamentele basis leg voor een orkestrator die zij later zelfstandig kunnen uitbreiden adhv mijn documentatie. 

Ik heb begrepen dat Secamo in het verleden een externe persoon ook al workflows heeft ontwikkeld mbv AWS step functions maar het grote pijnpunt hierbij was dat ze dit zelf niet konden onderhouden of uitbreiden en dus altijd beroep moesten doen op deze externe persoon om problemen op te lossen of nieuwe flows te schrijven wat natuurlijk niet ideaal

De focus zal eerst liggen bij het automatiseren van IT taken op basis van tickets, zoals een nieuwe gebruiker aanmaken op basis van een service request. 

### Uitbreiding naar Niet-Technische Domeinen

De orkestrator zal op termijn niet alleen technische handelingen automatiseren, maar ook processen ondersteunen voor departementen zoals HR, Invoicing en Strategy. Dit brengt een  verschuiving in de architecturale vereisten met zich mee. Het risico bestaat dat het platform "te generiek" wordt, waarbij het alles voor iedereen probeert te zijn en daardoor aan effectiviteit verliest. 

Om dit te mitigeren, heb ik een architectuurprincipe geformuleerd: "Eén kern (orchestrator) + meerdere domeinen".

In verder onderzoek zal dus onderzocht worden welke oplossing de kern zal voorstellen (hoofdorchestrator) en welke de meerdere domeinen zullen voorstellen. Na een korte brainstorm sessie zijn er een aantal concrete opties:

- Custom Python Orchestrator in AWS Step Functions (Kern) + n8n workflows (meerdere domeinen)
- Custom Python Orchestrator in AWS Step Functions (Kern) + Custom Sub Orchestrators met onderliggend specifieke workflows (meerdere domeinen)

Verder zijn er ook next generation opties waarbij we kunnen kiezen om te werken met agents en sub agents die door de orchestrator zullen worden aangeroepen, uit mijn analyse onderzoek zal dan blijken waar agents van pas kunnen komen (waar simpele workflows onvoldoende zijn) en hoe we dit op een modulair manier kunnen aanpakken. Een voordeel van agents is dat ze goed zijn in het verwerken van ongestructureerde data zoals tickets met losse beschrijving (Xander zei me dat deze tickets vaak de moeilijkste zijn)

### Governance en Isolatie

De integratie van gevoelige departementen zoals HR vereist strikte governance. In tegenstelling tot CI/CD-logs, die voornamelijk technisch en gestructureerd van aard zijn, bevatten HR- en Invoicing-workflows mogelijks persoonsgegevens en financiële data. Dit betekent dat ik in mijn ontwerp rekening moet houden met:

- **Logische Scheiding:** Workflows, secrets en logs moeten per departement gescheiden blijven. Een fout in een support workflow mag nooit leiden tot het lekken van HR-data. Bij workflows is dit makkelijker dan bij agents.
    
- **Workflow-catalogus:** Er moet een gestandaardiseerde catalogus komen van herbruikbare bouwstenen (bijv. "Approve", "Notify", "Provision", "Create") die door verschillende departementen kunnen worden gebruikt zonder de onderliggende code te hoeven herschrijven.
    

### Volgende Stappen

- **Data Classificatie:** Ik moet per departement (Implementation, HR, Strategy, Ops) vastleggen welke data-classificatie van toepassing is om de juiste security-controls in de architectuur te verwerken.
    
- **Stakeholder Interviews Niet-IT:** De huidige vragen focussen sterk op technische profielen. Ik moet verifiëren of er input nodig is van HR of Finance om hun specifieke "pijnpunten" te begrijpen, zoals beschreven in de "Pijn Matrix" (nog op te stellen).
    

---

## 3. Identificatie en Kwalificatie van Processen

Een cruciaal onderdeel van mijn analyse is het bepalen _welke_ processen geautomatiseerd moeten worden. Niet alles is gepast zich voor orkestratie. Op basis van [[01_ANALYSE_RESEARCH.md]] en [[01_Stappenplan_processen.md ]]heb ik criteria opgesteld om potentiële processen te kwalificeren.

### De Criteria

Om te voorkomen dat ik tijd besteed aan het automatiseren van onnodige taken, hanteer ik vier strikte criteria :

| **Criterium**         | **Drempelwaarde / Definitie**                                                  | **Relevantie voor Secamo**                                                                             |
| --------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| **Frequentie**        | Minstens wekelijks of 50+ keer per jaar.                                       | Incidentele taken is niet de moeite om te automatiseren met complexe workflows die niet generiek zijn. |
| **Complexiteit**      | Raakt minstens 2 verschillende systemen (bv. Ticket systeem + Cloud omgeving). | Als het proces slechts één systeem raakt, volstaat vaak een simpel script.                             |
| **Menselijke Factor** | Bevat wachttijden ("Wachten op goedkeuring") of copy-paste werk.               | Dit zijn de grootste vertragers in de huidige processen (bottlenecks).                                 |
| **Foutgevoeligheid**  | Is het proces gevoelig voor menselijke fouten (bijv. vermoeidheid)?            | Fouten in firewall-regels of user-provisioning vormen een direct veiligheidsrisico.                    |

### Ontdekkingsstrategie van processen

Ik gebruik specifieke tactieken om verborgen processen te vinden :

1. **Ticket-onderzoek** Ik analyseer het ticketsysteem op zoek naar termen als "reset", "provision", "access" en "create". Tickets die vaak met de status "Done manual" worden gesloten, zijn primaire kandidaten.
    
2. **De interviews:** Door gerichte vragen te stellen aan collega's zoals "Wat is een taak waarvan je weet dat die geautomatiseerd kan worden maar niet de kennis hebt om dit te implementeren?", probeer ik frustraties bloot te leggen die niet direct in tickets zichtbaar zijn.
   
3. **Voorgestelde workflows/processen:** Door mijn gesprekken met Xander krijg ik al waardevolle voorstellen van mogelijke processen die kunnen geautomatiseerd worden.

### Geïdentificeerde Use Cases

Op basis van mijn voorlopige analyse en de feedback van Xander heb ik drie concrete use cases geïdentificeerd die als leidraad dienen voor de PoC:

#### Use Case 1: Vulnerability Management (Focus: Maxime)

Uit het feedbackgesprek met Xander bleek dit een goed startpunt.

- **Probleem:** Microsoft Defender genereert alerts voor kwetsbaarheden (bv. verouderde Notepad++). Analisten moeten handmatig uitzoeken wie dit heeft en tickets aanmaken.
    
- **Oplossing:** De orkestrator pollt alerts, clustert deze (bv. alle devices met dezelfde kwetsbaarheid in één cluster) en maakt automatisch tickets aan in Jira op basis van templates.
    
- **Criteria Check:** Hoge frequentie (alerts zijn continu), raakt meerdere systemen (Defender, Jira), en vermindert menselijke fouten.

#### Use Case 2: IAM Automation (Focus: Implementation)
Onboarding/offboarding van nieuwe gebruikers

- **Probleem:** Service Request tickets in Jira met de vraag om een nieuwe gebruiker aan te maken voor een nieuwe werknemer gebeurt manueel:
    
- **Oplossing:** Een workflow die op basis van het ingevulde ticket template (naam, company, department) een gebruiker aanmaakt binnen de omgeving van de klant zonder manuele handelingen. 
    
- **Criteria Check:** 2 systemen: Jira + Cloud omgeving
    

#### Use Case 3: Geautomatiseerde Compliance Rapportage

- **Probleem:** Implementation engineers maken maandelijks screenshots om te bewijzen dat backups werken en firewalls dicht staan en dus de klant nog steeds compliant is. 
    
- **Oplossing:** Een workflow die op de 1e van de maand logs ophaalt uit AWS, een PDF genereert en deze uploadt naar SharePoint. Hierbij wordt altijd vergeleken met de baseline audit om eventuele non-compliant onderdelen te verifieren.
    
- **Criteria Check:** Hoge menselijke factor (saai werk, veel copy paste werk).
    

### Volgende Stappen

- **Mapping afronden:** Voor elke use case moet ik een gedetailleerde "Input-Process-Output" kaart maken, zoals beschreven in [[01_Stappenplan_processen]].
    
- **Validatie API-limieten:** Voor de Vulnerability Management case moet ik specifiek onderzoeken wat de rate limits zijn van de Microsoft Defender en Jira API's om de polling-frequentie te bepalen.
    

---

## 4. Technische Vereisten en Architectuur

De technische eisen voor de orkestrator zijn vastgelegd in [[01_TECH_REQUIREMENTS]] en vormen de basis van het ontwerp.

### Performance en Schaalbaarheid

Ik maak onderscheid tussen "fast path" workflows (reactietijd seconden/minuten) en zware infrastructuur-wijzigingen (minuten/uren).

- **Concurrency:** Het systeem moet pieken kunnen opvangen. Als support massaal tickets aanmaakt, mag dit de kritieke HR-processen niet vertragen. Dit vereist "backpressure" en queuing systemen.
    
- **Idempotentie:** Een kernprincipe is dat elke workflow herhaalbaar moet zijn zonder "drift" te veroorzaken of te moeten aanpassen aan specifieke klant. Als een workflow voor de tweede keer draait, mag dit geen dubbele resources creëren of een ander resultaat geven.
    

### Security by Design

Gezien de aard van Secamo als MSP is security geen extratje maar een basis:

- **IAM & Least Privilege:** Elke stap in een workflow krijgt een specifieke IAM-rol. Een workflow die tickets aanmaakt, mag geen infrastructuur wijzigen.
    
- **Secrets Management:** Geen secrets in logs. Integratie met AWS Secrets Manager of Parameter Store is vereist. 
  Eventueel Keeper als secrets manager aangezien dit gebruikt wordt binnen Secamo. 
    
- **Auditeerbaarheid:** "Tamper-evident" logging is noodzakelijk. We moeten kunnen bewijzen _wie_ een actie heeft getriggerd en _wie_ een goedkeuring heeft gegeven.
    

### Architectuur Keuzes: De Rol van Python en Middleware

In het feedbackgesprek met Xander de belangrijke kantekening gemaakt om me niet blind te staren op "bekende" tools zoals n8n, maar te kijken naar een robuustere oplossing. Een interessante architecturale keuze is het gebruik van een **Python FastAPI service** als tussenlaag.

- **Functie:** Deze laag fungeert als de primaire interface tussen de orkestrator (bijv. Step Functions) en de externe systemen.
    
- **Voordeel:** Het isoleert credentials en logica. Low-code tools kunnen communiceren met deze API zonder direct toegang te hebben tot gevoelige cloud-credentials. Dit verhoogt de veiligheid en modulariteit.

### Hosting: Self-Hosted vs. Serverless

Xander heeft een voorkeur uitgesproken voor "self-hosted" oplossingen, bij voorkeur op de Secamo racks in het datacenter. Hierop wordt AWS gebruikt. Dit heeft invloed op de keuze van de orkestrator:

- **AWS Step Functions:** Is volledig serverless. Hoewel technisch superieur voor AWS-integratie, kan de wens voor "eigen beheer" en hosting op specifieke hardware (racks) pleiten voor een container-based oplossing zoals **Argo Workflows** of **Camunda** draaiend op EKS/ECS. Verder onderzoek zal dit wat concreter maken.
- N8N biedt een selfhosted oplossing en is open source. 
    

### Volgende Stappen

- **Keuze Orchestrator Finaliseren:** Ik moet de vergelijking tussen Custom Python als adapter laag, N8N, Zappier, Microsoft Copilot, Camunda, Argo en Step Functions afronden op basis van de "self-hosted" voorkeur en de kostenimpact.
    
- **Rate Limiting Design:** Ik moet in het design-document (fase 2) specifiek uitwerken hoe de Python tussenlaag omgaat met API-rate limits (backoff/retry policies).
    

---

## 5. Infrastructuur als Code (IaC) en CI/CD Integratie

De orkestrator staat niet op zichzelf; hij moet integreren met de bestaande tooling van Secamo. Uit het bestand `01_INTERVIEW_INVENTORY` blijkt dat ik de huidige status van IaC en CI/CD gedetailleerd in kaart moet brengen.

### Terraform en Ansible

- **Terraform:** Zal gebruikt worden voor het provisioneren van de cloud-infrastructuur (VPC's, Security Groups) die nodig is voor de orchestrator. Ik moet onderzoeken hoe de huidige modules zijn gestructureerd en hoe "remote state" wordt beheerd in een multi-tenant omgeving.
    
- **Ansible:** Wordt ingezet voor configuratiemanagement op de servers (bv. agents installeren of software installeren). De orkestrator moet Ansible playbooks kunnen triggeren na de Terraform-run.
    

### CI/CD Pipeline

De orkestrator zelf, evenals de workflows, moeten via een CI/CD-pipeline worden uitgerold.

- **Validatie:** Elke commit triggert `terraform fmt`, `validate` en security scans (Checkov, KICS).
    
- **Repository Structuur:** Ik moet een mappenstructuur ontwerpen die modulair is en hergebruik bevordert, met duidelijke scheiding tussen generieke modules en klantspecifieke configuraties.
    

### Volgende Stappen

- **Inventory Invullen:** Het bestand [[01_INTERVIEW_INVENTORY]] is nu nog leeg en zal volgende week worden ingevuld door interviews met de relevante personen.
    
- **Tooling Bevestigen:** Definitieve bevestiging van de CI/CD-tool (GitHub Actions, GitLab CI, of AWS CodePipeline) is nodig voor het Design-document.
    

---

## 6. Projectplanning en Voortgang

### Huidige Status

Ik bevind me momenteel in **Week 1** van de Analysefase.

- **Gedaan:** Initiële research naar processen, feedbackgesprek met Xander, opzet requirements.
    
- **Bezig:** Interviews afnemen (Inventory), Orchestrator vergelijking voeren.
    
- **Planning:** Er is een gedetailleerde weekplanning beschikbaar in [[Projectplanning.md]] die ik nauwgezet volg.

### Feedback Verwerking

De feedback van Xander uit [[05_02_Feedback.md]] is belangrijk geweest voor de aanvulling en bijsturing van mijn planning. 

### Volgende Stappen

- **Gate 1 Voorbereiding:** Ik moet toewerken naar de "Gate 1" review aan het eind van week 4. Dit betekent dat het analysedocument compleet, diepgaand en gevalideerd moet zijn.
    
- **Toegang Regelen:** Ik moet zorgen dat ik tijdig toegang heb tot de beloofde M365 en AWS tenants om in de design-fase direct aan de slag te kunnen.

---

## Conclusie en Persoonlijke Reflectie

Deze samenvatting toont aan dat er al een goede basis ligt voor het verdere onderzoek. De verschuiving van een puur technische CI/CD-tool naar een bedrijfsbrede proces-orkestrator maakt het project complexer, maar ook waardevoller voor Secamo. Ik moet zoals eerder vermeld niet voor alles workflows gaan schrijven, maar wel ervoor zorgen dat dit mogelijk is. De sleutel tot succes in de komende weken ligt in het concretiseren van de use cases (vooral Vulnerability Management) en het maken van een onderbouwde keuze voor de orchestrator-technologie die past bij de wens voor self-hosting, modulariteit en traceability.

Ik heb nu een duidelijk overzicht van wat ik heb (requirements, use cases, architectuur-ideeën) en wat ik nog mis (ingevulde inventory, definitieve tool-keuze, API-limiet details). Dit stelt mij in staat om gericht actie te ondernemen in de rest van de analysefase.