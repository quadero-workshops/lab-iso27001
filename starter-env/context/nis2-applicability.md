# AquaPolder N.V. - NIS2 Applicability Assessment

**Versie**: 2026-Q1
**Bron**: Interne NIS2-impact-analyse uitgevoerd door Lieke van Bemmel en juridisch adviseur, 2025-12. Bevroren voor deze gap-analyse maar markeer als "te valideren".
**Markering**: VERTROUWELIJK

> Onder de Cyberbeveiligingswet (Nederlandse implementatie van EU 2022/2555, NIS2-richtlijn) kwalificeert AquaPolder N.V. als **essentiele entiteit**. Dit document onderbouwt die kwalificatie en lijst de bijbehorende verplichtingen.

## 1. Kwalificatie als Essentiele Entiteit

### Sector
- **NIS2 Annex I**: Sector 1 - Drinkwater
- **Type entiteit**: Drinkwaterbedrijf dat drinkwater aan publiek levert

### Omvang
- ~180 medewerkers (boven de small/medium drempel van 50 conform NIS2 Artikel 2 lid 1)
- Jaaromzet > 10 miljoen EUR (boven de small/medium drempel van 10 mln)

### Conclusie
Op basis van sector (kritieke infrastructuur drinkwater) en omvang valt AquaPolder N.V. binnen scope als **essentiele entiteit**. Voor essentiele entiteiten geldt de zwaarste set NIS2-verplichtingen.

## 2. Verplichtingen onder de Cyberbeveiligingswet

### Pijler 1: Zorgplicht (Artikel 21)

Essentiele entiteiten moeten passende en evenredige technische, operationele en organisatorische maatregelen treffen om risico's voor de netwerken en informatiesystemen te beheersen.

Specifieke aandachtsgebieden uit Artikel 21 lid 2:
- (a) Risico-analyse en beleid voor informatiesystemen
- (b) Incidenthandhaving (detectie, respons, herstel)
- (c) Bedrijfscontinuiteit en crisismanagement
- (d) **Ketenrisico, leveranciersbeoordeling**
- (e) Beveiliging bij verwerving, ontwikkeling en onderhoud
- (f) Beleid en procedures m.b.t. cyber-hygiene en training
- (g) Beleid en procedures m.b.t. cryptografie en encryptie
- (h) Beleid m.b.t. personeels-, asset- en toegangsbeheer
- (i) Multi-factor authenticatie of continue authenticatie

### Pijler 2: Meldplicht (Artikel 23)

Incident-rapportage richting het NCSC (Nationaal Cyber Security Centrum, de Nederlandse overheids-CSIRT) en de bevoegde toezichthouder:
- **Vroege waarschuwing**: binnen **24 uur** na bekend-worden van een significant incident
- **Incident-melding**: binnen **72 uur** met uitgebreidere informatie
- **Eindrapport**: binnen **1 maand** met root-cause en lessen

Significant incident = veroorzaakt of kan veroorzaken: ernstige operationele verstoring, financiele schade, of impact op andere natuurlijke of rechtspersonen.

### Pijler 3: Registratieplicht

AquaPolder moet zich registreren bij de Rijksinspectie Digitale Infrastructuur (RDI) als essentiele entiteit, met bijgewerkte contactgegevens, sectorale activiteit, IP-ranges, en namen van bestuurders die eindverantwoordelijk zijn.

Status: **Registratie ingediend Q1 2026**, ontvangst-bevestiging ontvangen.

### Pijler 4: Toezicht en bestuurdersaansprakelijkheid

- Bestuurders zijn persoonlijk aansprakelijk voor naleving van de zorgplicht
- Cyber-toezicht: Rijksinspectie Digitale Infrastructuur (RDI) is in de Cyberbeveiligingswet aangewezen als toezichthouder voor de zorg- en meldplicht
- Sectoraal toezicht op drinkwaterkwaliteit en -veiligheid: ILT (Inspectie Leefomgeving en Transport), zoals nu al onder de Drinkwaterwet
- Boetes: tot 10 miljoen EUR of 2% van de wereldwijde omzet (hoogste geldt)
- Tijdelijk verbod op managementfuncties bij ernstige nalatigheid

## 3. Mapping richting Annex A controls

Veel NIS2-eisen overlappen met ISO 27001:2022 Annex A. Indicatieve mapping:

| NIS2 Art 21 lid 2 | ISO 27001 Annex A primair | Annex A secundair |
|---|---|---|
| (a) Risico-analyse | A.5.7 (Threat Intelligence) | A.6 series, A.5.1 |
| (b) Incidenthandhaving | A.5.24-A.5.27 (Incident Management) | A.8.15, A.8.16 |
| (c) BCM | A.5.29, A.5.30 (BCM) | A.8.13, A.8.14 |
| (d) Ketenrisico | A.5.19-A.5.23 (Supplier) | A.5.31 (Legal) |
| (e) SDLC | A.8.25-A.8.34 (Development) | A.8.21, A.8.22 |
| (f) Cyber-hygiene + training | A.6.3 (Awareness) | A.5.7 (TI), A.8.7 (Malware) |
| (g) Cryptografie | A.8.24 (Cryptography) | A.8.20, A.8.21 |
| (h) Asset/access management | A.5.9-A.5.13 (Assets) | A.5.15-A.5.18 (Access) |
| (i) MFA | A.5.17 (Authentication) | A.8.5 (Secure Auth) |

## 4. Sector-specifieke aspecten

### CSIRT en sectorale informatiedeling
AquaPolder rapporteert significante incidenten aan het **NCSC** (Nationaal Cyber Security Centrum, de Nederlandse overheids-CSIRT). Voor sector-specifieke threat intelligence en lessons learned maakt AquaPolder gebruik van:
- **WaterISAC**: internationale ISAC voor de drinkwater- en afvalwatersector
- **Vewin** (Vereniging van waterbedrijven in Nederland): brancheoverleg, periodieke oefeningen, coordinatie met NCSC

### Sector-overleg
AquaPolder neemt deel aan het Vewin technisch-overleg cyber (4x per jaar). Onderwerpen: threat landscape, incident lessons learned, beste praktijken.

### Vewin-richtlijnen
Vewin heeft sector-specifieke richtlijnen die complementair zijn aan NIS2:
- Vewin Beveiligings-richtlijn 2023 (versie nu in update voor NIS2-alignment)
- ISA-99 / IEC 62443 aanbeveling voor OT
- Watermissions-architectuur (anti-fraud op klantportaal-betalingen)

## 5. Stand 2026-Q1

### Wat is op orde
- Registratie bij RDI gedaan
- Bewustzijn bij bestuur (kwartaalupdate)
- Aansluiting op NCSC en WaterISAC voor incident- en threat-intel-deling
- Deelname Vewin technisch-overleg cyber
- Defender / Sentinel / MSSP-aansluiting voor IT
- BCM voor drinkwater-continuiteit (robuust)

### Wat ontbreekt / gap
- Geen formeel NIS2-compliance dashboard
- Incident-response-organisatie binnen AquaPolder (besluitvorming, escalatie, communicatie) onvoldoende gedrild
- Leveranciersbeoordeling NIS2-conform niet ingericht (zie interview-05)
- OT-monitoring vrijwel afwezig
- Geen formele cyber-hygiene-training-cyclus (alleen ad-hoc post-Q3 2025)
- Geen periodieke security-test-cyclus (pen-test, OT-assessment)

### Risico op handhaving
Bij ontdekte non-compliance is de waarschijnlijke handhavings-route:
1. Waarschuwing van de bevoegde toezichthouder (RDI voor de cyber-zorg-/meldplicht) met aanwijzing tot correctie
2. Boete bij niet-naleving
3. Bestuurders-aansprakelijkheid bij ernstige nalatigheid

RDI heeft in 2025 aangekondigd in 2026 actief toezicht te starten op essentiele entiteiten onder de Cyberbeveiligingswet, waaronder drinkwaterbedrijven.

## 6. Te valideren in gap-analyse

- Is de zelf-uitgevoerde NIS2-applicability assessment correct in alle aspecten?
- Zijn er activiteiten / dochterondernemingen van AquaPolder die niet meegenomen zijn in de scoping?
- Vallen verwerkingsovereenkomsten (bv. Bird&Co als ontwikkelpartner) onder de keten-verplichting?
- Welke specifieke NIS2-deadlines gelden in 2026 vs 2027?
