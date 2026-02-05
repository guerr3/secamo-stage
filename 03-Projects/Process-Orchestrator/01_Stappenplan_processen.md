Hier is het **5-Stappenplan** om verborgen bedrijfsprocessen te ontdekken en analyseren
## Stap 1: Definieer je "Target Criteria" (Het Filter)

Niet alles moet geautomatiseerd worden. Voordat je op jacht gaat, moet je weten waar je naar zoekt. Gebruik deze criteria om een proces te kwalificeren voor jouw orchestrator PoC:

- **Frequentie:** Minstens wekelijks of 50+ keer per jaar (anders loont de moeite niet).
- **Complexiteit:** Raakt minstens 2 verschillende systemen (bv. Ticket Systeem + Cloud Console). _Als het maar 1 systeem is, is een simpel script vaak al genoeg._
- **Menselijke Factor:** Bevat wachttijden ("Wachten op goedkeuring van Xander") of copy-paste werk.
- **Foutgevoeligheid:** Gaat het wel eens mis als iemand moe is? (bv. verkeerde firewall regel).
## Stap 2: De "Jacht" (Discovery Phase)

Kies één tactiek per dag in Week 1/2 om input te verzamelen zonder mensen lastig te vallen.

- **Tactiek A: De Ticket-Archeologie (Service Desk/Jira)**
    
    - _Actie:_ Vraag (read-only) toegang tot het ticketsysteem. Zoek op termen als _"reset"_, _"provision"_, _"access"_, _"create"_, _"certificaat"_.
        
    - _Doel:_ Vind tickets die altijd dezelfde oplossing hebben ("Done manual").
        
- **Tactiek B: De "Koffie-automaat" Vraag**
    
    - _Vraag:_ "Wat is het saaiste klusje dat je elke vrijdagmiddag moet doen voor je naar huis mag?"
        
- **Tactiek C: De "Schaduw-IT" Check**
    
    - _Actie:_ Kijk in de gedeelde Slack/Teams kanalen. Waar sturen mensen manueel berichtjes als "Ik heb de server herstart" of "Kan iemand dit goedkeuren?" Dit zijn jouw orchestrator-triggers.
        

## Stap 3: Analyseer 3 Nieuwe Domeinen (Naast SOC/Cloud)

Op basis van MSSP-trends en jouw security-focus, zijn dit drie "goudmijnen" voor extra processen bij Secamo:[manageengine+1](https://www.manageengine.com/siem-mssp/what-is-mssp/top-mssp-usecases.html)

## Domein A: Identity Lifecycle Management (IAM)

_Offboarding is vaak een groter risico dan onboarding._

- **Het Proces:** Medewerker bij klant X gaat uit dienst.
    
- **De Pijn:** HR mailt Secamo. Secamo moet accounts blokkeren in AD, M365, VPN, en specifieke SaaS-tools. Vaak wordt er eentje vergeten (security lek!).
    
- **Jouw Kansen:**
    
    - Trigger: Ticket "Offboarding Jan Janssen".
        
    - Orchestrator: Disable AD user -> Revoke M365 license -> Wacht op bevestiging -> Archive mailbox.
        
    - _Tools:_ PowerShell / Graph API.[[resilientsecurity](https://resilientsecurity.be/when-custom-reporting-breaks-automation-a-one-identity-offboarding-story/)]​
        

## Domein B: Compliance Evidence Gathering (ISO 27001 / SOC2)

_De "screenshots maken" hel._

- **Het Proces:** Voor de audit moet Secamo bewijzen dat backups werken en firewalls dicht staan. Nu: Engineers maken maandelijks screenshots.
    
- **De Pijn:** Tijdrovend, saai, en soms vergeten (groot probleem bij audit).
    
- **Jouw Kansen:**
    
    - Orchestrator: Draait elke 1e van de maand.
        
    - Actie: Haalt backup logs op (AWS API) + Export van Security Groups.
        
    - Output: Genereert een PDF en uploadt deze naar de Audit-map (SharePoint).
        
    - _AI Bonus:_ Laat AI checken of er afwijkingen zijn in de logs.​
        

## Domein C: Certificate Management (PKI)

_De stille killer van uptime._

- **Het Proces:** SSL certificaten van klanten verlopen. Iemand moet dit in de gaten houden en vernieuwen.
    
- **De Pijn:** Iemand mist de email -> website down -> paniek.
    
- **Jouw Kansen:**
    
    - Orchestrator: Checkt dagelijks expiry dates.
    - Actie (T-30 dagen): Maakt ticket aan.
    - Actie (T-7 dagen): Stuurt Slack alert naar engineer "NU VERLENGEN".
    - Actie (T-0): (Geavanceerd) Automatische renewal via Let's Encrypt/AWS ACM.​

## Stap 4: Mapping (Input-Process-Output)

Zodra je een proces hebt gevonden, breng het in kaart op 1 A4'tje. Gebruik dit formaat om het "verkoopbaar" te maken aan je stagebegeleider:

| Onderdeel            | Vraag                       | Voorbeeld (Certificate Renewal)                                                      |
| -------------------- | --------------------------- | ------------------------------------------------------------------------------------ |
| **Trigger**          | Wat start dit proces?       | Datum (certificaat verloopt < 30 dagen).                                             |
| **Input Data**       | Welke info heb je nodig?    | Domeinnaam, Expiry Date, Klantcontact.                                               |
| **Stappen (Nu)**     | Wat doet de mens?           | 1. Checkt kalender. 2. Logt in op provider. 3. Download cert. 4. Upload op server.   |
| **Pijn**             | Waar gaat het mis?          | "Vergeten te checken", "Wachtwoord provider kwijt".                                  |
| **Stappen (Straks)** | Wat doet jouw Orchestrator? | 1. API check expiry. 2. Auto-renew (of ticket aanmaken). 3. Deploy op Load Balancer. |
| **Winst**            | Wat levert het op?          | 0% downtime risico, 2 uur werk bespaard per maand.                                   |

## Stap 5: Validatie & Keuze

Leg je top 3 voor aan Xander (je begeleider).

- _Pitch:_ "Ik heb 3 processen gevonden. **Proces A** (Onboarding) is het meest zichtbaar, maar **Proces B** (Certificaten) is technisch eenvoudiger voor een snelle _quick win_ in week 3. Welke heeft de hoogste prioriteit voor Secamo?"
    

**Tip voor Week 1:** Begin met **Domein A (Offboarding)**. Dit is technisch tastbaar (AD/M365 accounts) en scoort enorm hoog op security-waarde, wat perfect past bij jouw profiel en de kernactiviteiten van Secamo.