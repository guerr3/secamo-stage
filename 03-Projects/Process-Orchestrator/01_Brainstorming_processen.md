Secamo profileert zich als "Trusted Advisor" en focust op IT/OT & Cloud Security. Een aantal mogelijke processen (zonder voorkennis of interviews) die zouden geautomatiseerd kunnne worden zijn:
## Proces 1: The Secure Landing Zone Vending Machine

- **Huidige situatie (Waarschijnlijk):** Een engineer draait manueel een Terraform script voor een nieuwe klantomgeving, maakt dan manueel accounts aan in de password manager, en configureert de backup-policy apart.
    
- **Flow:**
    
    1. Trigger: Sales verandert status in CRM naar "Active".
    2. Orchestrator: Start Terraform (AWS VPC opzetten).
    3. Orchestrator: Roept Ansible aan (Security agents installeren).
    4. Orchestrator: Maakt tickets aan in Jira voor manuele nazorg.

## Proces 2: Automated Threat Isolation & Forensics 

- **Huidige situatie:** SOC ziet een "High Risk" malware alert. Ze bellen de klant, wachten op toestemming, en vragen dan aan Cloud team om de VM te isoleren. Tijdverlies: uren.
    
- **Flow:**
    
    1. Trigger: EDR (Endpoint Detection & Response) detecteert Ransomware.
    2. Orchestrator: Past direct de AWS Security Group aan ("Isolate").
    3. Orchestrator: Neemt een snapshot van de disk voor bewijsmateriaal (Forensics).
    4. Orchestrator: Stuurt een "Critical Alert" naar het Slack-kanaal van het SOC met een "De-isolate" knop.

## Proces 3: Vulnerability Management Loop

- **Huidige situatie:** Scanner (bv. Nessus/Tenable) draait. Rapport is 500 pagina's. Engineer mailt klant: "Je moet patchen". Klant mailt terug: "Gedaan". Engineer moet opnieuw scannen om te checken.
    
- **Flow:**
    
    1. Trigger: Scan vindt "Critical Vulnerability".
    2. Orchestrator: Checkt of er een exploit bestaat (Threat Intel).
    3. Orchestrator: Maakt ticket aan bij klant.
    4. _Wacht-stap:_ Orchestrator wacht tot ticket status "Resolved" is.
    5. Orchestrator: Start _automatisch_ een her-scan enkel op die asset om te verifiëren.
    

