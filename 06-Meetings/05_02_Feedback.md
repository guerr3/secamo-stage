---
date: 2026-05-02
time: 10:58
tags:
  - meeting
  - stage
  - publish
attendees:
  - Xander
  - Warre
type:
  - feedback
  - vragen
  - antwoorden
  - planning
---
# 🤝 Meeting | Feedback

**Datum**: 2026-02-05
**Tijd**: 10:58 - 11.30
**Locatie**: Teams
**Aanwezig**:
- [[Xander Boedt]]
- [[Warre Gehre]]

## 📋 Agenda

1. Planning overlopen
2. Vragen beantwoorden
3. 

## 📝 Notities & Discussie

### Vragen:

**Welke workflows zou ik als eerste mee kunnen beginnen ? Heb je hier een voorbeeld van ?**

Workflow voor Maxime: 
- Vulnerability Assesement/Management door vuln alerts te indexeren vanuit defender en te clusteren (bv. zelfde apparaat, klant) en als ticket loggen in Jira. 
- Alert enrichement is een leuke extra. --> polling
- Bv. Gebruiker heeft notepad ++ --> vulnerability gevonden --> indexeren --> clusteren (zijn er andere apparaten die notepad gebruiken ?) --> tickets loggen
- Let bij polling op rate limiting

**Welke licenties moet ik vanuit gaan dat de standaard is ? Dat ik niet begin te werken met features die bij klanten helemaal niet aanwezig zijn.**

- Niet vanuit gaan dat iedereen E5 heeft ! 
- Ik ga een M365 tenant krijgen en hiermee moet ik het doen. Deze tenant zal gebruikt worden om M365 configuratie te testen zoals gebruikers aanmaken etc.
- Ik ga ook een AWS tenant krijgen. Deze tenant zal fungeren als "backend" voor de process orchestrator waarop de interne functies die nodig zijn voor workflows zullen runnen.

**Wat is het primair notificatiekanaal voor bv. approval gates van workflows ? **
- Jira Tickets kunnen ook approval gates bevatten (dus bv. workflow maakt, wanneer goedkeuring vereist is, nieuw ticket met approval gate)
- Alternatief: Meldingen via Teams/Mail

**Hoe moet ik de ticketsysteems bekijken in de context van connectors ?**
Bv. Jira, ServiceNow, ServiceDesk Plus, etc

- Ticket templates zorgen voor een gestandardiseerde manier om data te verkrijgen van tickets
- Tickets zijn de startpunten (=triggers) die workflows starten
- Tickets kunnen ook aangemaakt worden tijdens een workflow voor bv. een approval

2 soorten tickets:
- Service requests : bv. gebruiker aanmaken
- Incident requests: bv. Iets gebeurd in de nacht zoals malware op een VM

Let bij ticketsysteems op rate limiting als we kiezen voor polling



### Extra opmerkingen

- Core vereiste van de orchestrator 
	- --> automatische deployment met heldere documentatie --> bv. aws step functions, docker image, terraform scripts, ansible scripts --> moet automatisch gedeployed kunnen worden
	- Moet modulair zijn en simpel zijn om nieuwe workflows te creeeren
- Niet teveel tunnelvisie over n8n: mijn taak is om de BESTE oplossing te vinden, niet de beste implementatie van een bepaalde oplossing
- Voorkeur tot self hosted via hun gehuurde racks in het datacenter (AWS)
- Traceability is zeer belangrijk !



## ✅ Beslissingen

- 
- 

## 🎯 Action Items

- [ ] [@Warre] 
- [ ] [@Xander] 

## 📅 Volgende Meeting

**Datum**: /
**Onderwerpen**: #vragen #meeting #feedback 

## 🔗 Links
- Project: [[01_TECH_REQUIREMENTS]]
- Related: 
	- [[01_ANALYSE_RESEARCH]]
	- [[04_02_DAILY]]

---
**Daily note**: [[2026-02-04]]
