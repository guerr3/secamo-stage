---
date: 2026-02-03
stage_dag: "2"
week: 2026-W06
tags:
  - daily
  - stage
  - publish
status: 🌱
---

# 2026-02-03 | Dag 2

[[2026-02-02|← Gisteren]] | [[2026-02-04|Morgen →]] | [[2026-W06|📅 Week]]

## 🎯 Doelen Vandaag

- [ ] Bespreken van opgemaakte planning met Xander --> Zie feedback
- [ ] Starten analyse fase 
	- [ ] Huidige automatiseringsopportuniteiten en uitdagingen in kaart brengen
	- [ ] Bestaande tooling van Secamo


### Overzicht activiteiten

| TAAK                                                                                                                                            | OUTPUT                                     |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| - Huidige automatiseringsopportuniteiten en uitdagingen in kaart brengen<br>- Bestaande Secamo tooling (IaC, CI/CD, cloud footprint) analyseren | Eerste ruwe notities voor Analyse document |

## Feedback

[TODO]

## 📝 Activiteiten & Werk

### Ochtend (09:00-12:30)

In de voormiddag werk ik aan het eerste doel, namelijk de voorbereiding en initieel onderzoek voordat ik mensen ga bevragen over de huidige automatiseringsopportuniteiten en uitdagingen. Dit werk is terug te vinden via deze wikilink:

[[01_ANALYSE_RESEARCH#Opdracht 1 Automatiseringsopportuniteiten in kaart brengen]]

### Middag (13:30-17:30)

In de namiddag heb ik verder gewerkt aan het identificeren van mogelijke processen op basis van de core business van Secamo. Hierbij heb ik over mogelijke processen, interviews worden later in de week afgenomen. 

Brainstorming: [[01_Brainstorming_processen]]

Verder heb ik ook nagedacht over **wat en welke** soort processen in aanmerking zouden komen om te automatiseren. We moeten namelijk niet alles automatiseren. 

Criteria voor processen die in aanmerking komen voor de process orchestrator:

- **Frequentie:** Minstens wekelijks of 50+ keer per jaar (anders loont de moeite niet).
- **Complexiteit:** Raakt minstens 2 verschillende systemen (bv. Ticket Systeem + Cloud Console). _Als het maar 1 systeem is, is een simpel script vaak al genoeg._
- **Menselijke Factor:** Bevat wachttijden ("Wachten op goedkeuring van Xander") of copy-paste werk.
- **Foutgevoeligheid:** Gaat het wel eens mis als iemand moe is? (bv. verkeerde firewall regel).

Tot slot is dit samengebracht in dit document:

[[01_ANALYSE_RESEARCH#2. Kijken naar Secamo's core business (Security & MSP)]]

Bijkomend heb ik ook de huidige tooling en technologieen die gebruikt worden al deels in kaart gebracht: [[01_ANALYSE_RESEARCH#Opdracht 2 Bestaande tooling analyseren]]

Vervolgens heb ik ook een eerste simpele diagram gemaakt: [[high_level_diagram]]

## 💡 Geleerd Vandaag

- 
- 

## 🔗 Links & Referenties
- Projects: [[01_ANALYSE_RESEARCH]]
- Knowledge: [[]]

## 🤔 Vragen & Blokkades

- Toegang tot documenten zoals SOP's van Secamo ?
- Bestaande tools binnen Secamo buiten de ticketingsystemen en cloud omgevingen ?

## ⏱️ Tijdsbesteding

- **Project work**: Xu
- **Research**: Xu
- **Meetings**: Xu
- **Documentation**: Xu

---
**Next**: [[2026-02-04]]
**Stagebegeleider**: [[Xander Boedt]]
