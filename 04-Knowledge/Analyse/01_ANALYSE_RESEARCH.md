---
tags:
  - research
  - stage
  - analyse
week: "1"
---
#research #analyse #interviews 
# Analyse Week 1

**DOEL:**

1. Huidige automatiseringsopportuniteiten en uitdagingen in kaart brengen

2. Bestaande Secamo-tooling (IaC, CI/CD, cloud footprint) analyseren

## Wat gaan we onderzoeken ?

Om een zo "nuttig" mogelijke process orchestrator op te zetten heb ik een zo compleet mogelijk beeld nodig over Secamo als bedrijf, welke bedrijfsprocessen zij hebben, welke tools ze op dit moment gebruiken, waar de opportuniteiten liggen binnen automatisatie, en zo verder. 

Het is hierbij belangrijk dat ik in de eerste week al actief data en informatie verzamel voor mijn analyse aangezien dit de basis is voor alle andere stappen.

Een belangrijk en niet te onderschatten aspect is hoe we zullen bepalen wanneer een process in aanmerking komt om geautomatiseerd te worden via de process orchestrator, niet alles moet geautomatiseerd worden. 

We gebruiken deze criteria om een proces te kwalificeren voor de orchestrator:

- **Frequentie:** Minstens wekelijks of 50+ keer per jaar (anders is het de moeite niet).
- **Complexiteit:** Raakt minstens 2 verschillende systemen (bv. Ticket Systeem + Cloud Console). _Als het maar 1 systeem is, is een simpel script vaak al genoeg._
  Met uitzondering veelvoorkomende "monosystemische" processen zoals bv. entra id gebruikers aanmaken en verwijderen
- **Menselijke Factor:** Bevat wachttijden ("Wachten op goedkeuring van Xander") of copy-paste werk.
- **Foutgevoeligheid:** Gaat het wel eens mis als iemand moe is? (bv. verkeerde firewall regel).

### Soorten processen:

De kracht van een process orchestrator ligt in het overbruggen van twee werelden binnen Secamo, namelijk de HR en IT. Bij IT processen wordt voornamelijk gewerkt met gestructureerde data terwijl bij HR processen voornamelijk gewerkt wordt met ongestructureerde data (mails, tickets)

- **IT-processen** zijn doorgaans **deterministisch**: Input A leidt _altijd_ tot Output B (tenzij er een technisch fout is). De uitdaging is technische complexiteit.
    
- **HR-processen** zijn **stochastisch** (onvoorspelbaar): Mensen vergeten dingen, sturen onvolledige tickets, of veranderen van mening. De uitdaging is proces-rigiditeit en foutafhandeling.

We zullen dus voornamelijk focussen op het automatiseren van de IT processen.


## 🔍 Onderzoeksopdrachten

### Opdracht 1: Automatiseringsopportuniteiten in kaart brengen

Ik bouw een Process Orchestrator, wat betekent dat ik processen moet vinden die complex genoeg zijn om georchestreerd te worden. Dit willen we doen op een manier dat een complex process is opgebouwd uit kleinere processen die als bouwstenen kunnen worden herbruikt om nieuwe workflows te maken. 

**Hoe brengen we dit in kaart ?**

#### 1. "Pijn Matrix" opstellen

Gesprekken voeren met Secamo collega's (vooral technische profielen) over wat ze doen, welke taken ze uitvoeren en welke tools ze gebruiken. Hierbij is het belangrijk dat we geen oppervlakkige vragen stellen zoals enkel "wat doe je" maar hier dieper op ingaan met vragen zoals:

- "Welke taak heb je deze week al meer dan 3 keer manueel uitgevoerd ?"
- "Bij welk process moet je wachten op goedkeuring van een collega (of klant) voordat je verder kan?"
- "Waar loopt de overdracht tussen verschillende teams soms mis?"

Ook bestaan hier verschillende gradaties in, om een diepe pijn matrix op te stellen moet ik verder kijken dan enkel de IT afdeling. Secamo positioneert zich als **trusted advisor** waarbij strategie wordt gekoppeld aan uitvoering. 

| Doelgroep                   | Focus van de vraag     | Vraag                                                                                                                                                        |
| --------------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Strategy Consultants**    | Compliance vs. Reality | _"Hoeveel tijd kost het om te bewijzen dat een klant nu nog steeds voldoet aan de ISO27001/NIS2-controls die jullie bv. vorig kwartaal hebben ingesteld?"_ ​ |
| **Cloud Engineers**         | Drift & Manuele acties | _"Wat is de meest voorkomende 'emergency fix' die je manueel in de AWS/Azure console doet omdat de (Terraform) pipeline te traag is of faalt?"_              |
| **Security Analysts (SOC)** | Alert Fatigue          | _"Welke alerts sluit je vaak 'als false positive' zonder diep onderzoek omdat de context (wie, wat, waar) ontbreekt in het ticket?"_​                        |
| **Project Managers**        | Lead times             | _"Wat is de grootste bottleneck bij het opleveren van een nieuwe security-omgeving voor een klant? Is dat menselijke goedkeuring of technische uitrol?"_     |
**Extra**

| Doelgroep            | De "Pijn" Vraag (Direct te stellen)                                                                                                                        | Wat je eigenlijk zoekt (Context voor jou)                                                                                                                              |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SOC Analist**      | _"Bij welke alerts klik je standaard 5 keer op 'refresh' of kopieer je IP-adressen manueel naar VirusTotal/AbuseIPDB?"_                                    | **Alert Enrichment:** Dit is het laaghangend fruit voor orkestratie. Een orchestrator kan data ophalen vòòrdat de analist het ticket opent.                            |
| **Cloud Engineer**   | _"Als er een nieuwe klant start, hoeveel tijd zit er tussen 'contract getekend' en 'eerste server live'? En hoeveel daarvan is wachten op iemand anders?"_ | **Tenant Onboarding:** Vaak een mix van IaC (Terraform) en manuele acties (Azure AD groepen, facturatie-tags). Orkestratie verbindt deze werelden.                     |
| **Service Manager**  | _"Voor welk maandrapport (bv. ISO compliance of patch status) ben je elke keer een halve dag screenshots aan het knippen en plakken?"_                     | **Compliance Reporting:** Ideaal voor een orchestrator die data uit verschillende bronnen (AWS, Azure, Tenable) trekt en in een PDF giet.                              |
| **Security Officer** | _"Is er een noodprocedure (bv. 'Besmette server isoleren') die je 's nachts niet durft te automatiseren zonder menselijke check?"_                         | **Human-in-the-loop:** Jouw orchestrator (Camunda/Step Functions) kan wachten op een "Approve" knop in Teams/Slack. Dit is de _killer feature_ t.o.v. simpele scripts. |
#### 2. Kijken naar Secamo's core business (Security & MSP)

Na het verkrijgen van een rijkere context door de interviews en gesprekken met collega's, zal ik (hopelijk) in staat zijn deze informatie te gebruiken om me meer te focussen processen die specifiek horen bij Secamo's core business. Hieronder zijn alvast een aantal voorbeelden van processen die mogelijks relevant kunnen zijn binnen de context van Secamo als bedrijf:

**Incident Response:** 
- Wordt een (malware) alert manueel opgepakt ?
- Bv. VM wordt malware gedetecteerd, wordt deze automatisch 

**Closed-loop vulnerability remediation:** 
- Als een critical vulnerability of bug wordt gevonden, hoe wordt deze dan opgelost ? 
- Werkt dit adhv losse excel lijsten of mails op dit moment ? Is er een vorm van automatisatie ?

**Customer Onboarding:** 
- Hoe wordt nieuwe klant-tenant in AWS opgezet ? Is dit volledig IaC of nog manuele "klik" acties nodig ?

**IAM**
- Hoe gebeurt onboarding en offboarding van nieuwe gebruikers in klantenomgevingen ? Gebeurt dit manueel ?
- Welke diensten vallen onder onboarding en offboarding ? (AD, M365, VPN, SaaS tools, etc)


**Compliance Reporting:** 
- Worden security scans automatisch gedraaid en gerapporteerd ?
- Is er veel copy paste werk dat de consultants doen voor kwartaalrapportages ?

**Certificate Management (PKI):**
- Worden SSL certificaten van klanten die bijna verlopen automatisch vernieuwd ?

**Zero-Trust Access Lifecycle (JIT)**
In een high-security omgeving wil je geen permanente admin-rechten.
--> Onderzoek hoe consultants momenteel toegang krijgen tot klantomgevingen. Is dit veilig en auditeerbaar?

**Automated Incident Containment (SOC Playbooks)**
- Onderzoek bij de security analisten welke acties zij ALTIJD uitvoeren bij een standaard aanval.

In een later stadium kunnen we eventueel ook een AI insteek toevoegen waarbij we processen zullen zoeken waar beslissingen worden genomen op basis van ongestructureerde data (email klant of vage log entry) of waarbij de kennis van een LLM model gebruikt kan worden als "hook" of verrijking van minder duidelijke data.

Voor de PoC is de combinatie van deze 2 het doel:​

1. **De "Hook":** Gebruik een HR-trigger (Offboarding) omdat dit tastbaar is voor management en risico verlaagt.
2. **De "Engine":** Gebruik IT-automatisering (Terraform/Ansible/Python) om de zware taken uit te voeren (AD user disablen, VM stoppen, Licentie intrekken).

#### **Wat nu ?**

- **Documentatie-audit:** Zoek naar "Standard Operating Procedures" (SOP's) in de Secamo-omgeving. Alles wat in een PDF-stappenplan staat, kan in een orchestrator.
-  **Data-flow mapping:** Teken voor één proces (bv. patching) uit waar de data momenteel "blijft liggen" (bv. in een mailbox).
-  **Stakeholder Mapping:** Identificeer wie de "proceseigenaar" is van de bovenstaande vermelde processen binnen Secamo.

### Opdracht 2: Bestaande tooling analyseren

#### Ticketingsystemen

**Klantafhankelijk!**
Het soort ticketingsysteem hangt dus af van de klant, we moeten dus een abstractielaag hebben in onze orchestrator die eenzelfde functie (bv. create_ticket()) kan uitvoeren ongeacht het ticketingsysteem. Dit wilt dus zeggen dat we in de orchestrator een "abstracte adapter klassen" gaan moeten schrijven, met voor elk van de ticketingsystemen een implementatie. 

| Systeem                     | API Type                   | Authenticatie                   | CRUD Support                                              | Documentatie                                  |
| --------------------------- | -------------------------- | ------------------------------- | --------------------------------------------------------- | --------------------------------------------- |
| **Service Desk Plus**       | REST API v3                | API Key (technician-based)      | Volledige CRUD op requests, problems, changes             | REST API docs beschikbaar                     |
| **Jira Service Management** | REST API v2 + Platform API | Personal Access Tokens / OAuth2 | Volledige CRUD + automation webhooks                      | Uitgebreide developer docs ​                  |
| **ServiceNow**              | Table API (OOB)            | OAuth2 / Basic Auth             | CRUD op alle tables (incident, change, problem) YouTube​​ | REST API Explorer included YouTube​           |
| **Freshdesk**               | REST API                   | API Key                         | Volledige ticket lifecycle management                     | API docs beschikbaar, soms counter-intuitive​ |
#### Cloud Omgeving

- AD
- M365
- Azure
- AWS




#### Password management:

Keeper
## 📚 Findings


## 🔗 Resources
- 

---
Linked to: [[_index|Process Orchestrator]]
