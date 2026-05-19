# AquaPolder N.V. - Engagement Intake Brief

**Opgesteld door**: Erik Grasman (Quadero), na kickoff-gesprek 2026-04-14
**Klant-contact**: Lieke van Bemmel (CIO), `lieke.vanbemmel@aquapolder.nl`
**Engagement-vorm**: 4 weken, gap-analyse ISO 27001:2022 met multi-norm component (IEC 62443, NIS2)

---

## Klantorganisatie

AquaPolder N.V. is een middelgroot Nederlands drinkwaterbedrijf met hoofdkantoor in Zwolle. De organisatie levert drinkwater aan circa 250.000 aansluitingen in delen van Overijssel en Gelderland en exploiteert 4 zuiveringsinstallaties. AquaPolder telt ongeveer 180 medewerkers.

Geografisch:
- 4 zuiveringsinstallaties (Zwolle-noord, Kampen, Hardenberg, Wijhe)
- 1 hoofdkantoor (Zwolle, kantoor + central control room)
- 12 boosterstations en distributielocaties (onbemand)

## Stakeholders

| Naam | Rol | Belang | Hoofdzorg |
|---|---|---|---|
| Lieke van Bemmel | CIO | DRI van het traject | NIS2 deadline en directie-vertrouwen |
| Karin Visser | SOC-coordinator (extern via MSSP) | Detectie en respons | Beperkte zichtbaarheid in OT |
| Robert Koster | Manager OT-engineering | OT-platform en SCADA | Continuiteit drinkwaterlevering, "geen verstoring tijdens audits" |
| Thomas Hertzog | CFO | Budgetbeslissingen | Inzicht in remediation-kosten, "geen open einden" |
| Niek Janssen | Leveranciersmanager | Contractbeheer Siemens, MSSP, SAP-partner | Aantoonbaarheid leveranciersbeoordeling onder NIS2 |
| Anouk Verheijen | Bestuurslid (CEO) | Eindverantwoordelijke | Certificering 2027 als marketing-asset richting gemeentes |

## Drijfveren

1. **Wettelijke verplichting**: Onder de Cyberbeveiligingswet (NL-implementatie van NIS2-richtlijn EU 2022/2555) is AquaPolder een **essentiele entiteit** (drinkwater = Annex I, sector 1: drinkwater). Zorgplicht (artikel 21), meldplicht (artikel 23), registratieplicht en bestuurdersaansprakelijkheid zijn van toepassing.
2. **Strategisch**: Het bestuur wil in **2027 ISO 27001:2022 gecertificeerd** zijn als zichtbaar bewijs richting toezichthouders (ILT, ACM), provincies en aandeelhoudende gemeentes.
3. **Operationeel**: Het Q3 2025 phishing-incident heeft het bestuur de schrik op het lijf gejaagd. Sindsdien staat security op de agenda van elke bestuursvergadering.

## Engagement Scope

**In scope:**
- Volledige IT-omgeving (kantoor, M365, Azure, ERP, klantportaal)
- OT-omgeving op alle 4 zuiveringsinstallaties (SCADA, PLC, HMI)
- Toetsing tegen ISO 27001:2022 (alle 93 Annex A controls)
- Multi-norm: IEC 62443 (alleen OT-onderdeel) en NIS2 (volledig)
- Drie rapportagevormen (technisch, executive, audit)
- Indicatieve remediation-roadmap en menukaart met vervolgblokken

**Out of scope:**
- Penetration testing (apart traject, mogelijk vervolg)
- Implementatie van remediation (alleen advisering)
- Fysieke audits op zuiveringsinstallaties (alleen op basis van consultantnotities en aangeleverde documentatie)
- Audit van het laboratorium-LIMS (wordt door derde partij gedraaid, valt buiten scope)

## "Goed" bij oplevering

- Volledige asset-inventarisatie met traceerbare bronverwijzingen
- Per Annex A control een gedefinieerde status met evidence en gap-onderbouwing
- Risicomatrix volgens ISO 27005 met expliciete L x I onderbouwing
- Multi-norm rapport waarin overlap tussen ISO 27001 / IEC 62443 / NIS2 zichtbaar is, zodat de klant niet driemaal hetzelfde betaalt
- Geprioriteerde 24-maands remediation-roadmap met quick wins, fasen en indicatieve kosten
- Drie rapporten: technisch (~30-50 pg), executive summary (max 5 pg), audit (formeel met SoA-concept)
- Menukaart met 5-8 vervolgblokken (prijs-geindiceerd) die de klant zelf kan kiezen

## Beschikbare input

- 5 interview-transcripts in `consultant-notes/` (CIO, SOC, OT, CFO, leverancier-contact)
- IT-landschap-document met applicaties en cloud-footprint
- Netwerktopologie (high-level, geen volledige firewall-regels)
- OT-architectuur met Purdue-model levels
- Lijst bestaande beveiligingsmaatregelen
- Q3 2025 incident-rapport (intern opgesteld, geen NCSC-melding gedaan - dat was nog niet verplicht)
- NIS2-applicability assessment (door AquaPolder zelf uitgevoerd, te valideren)

## Aandachtspunten en risico's voor de opdracht

- **OT-toegang**: Robert Koster vraagt om "geen verstoring tijdens audits" - de gap-analyse moet evidence-based zijn op basis van documentatie en interviews, niet op live-tests in de SCADA.
- **Single-source bias**: Veel input komt uit slechts 5 interviews. Markeer expliciet welke conclusies aanvullend onderzoek vereisen.
- **Leverancierscontracten**: Niek Janssen heeft aangegeven dat de Siemens-contracten "ergens in een SharePoint" staan - niet gestructureerd centraal beheerd. Dit gaat zichtbaar zijn in A.5.19-A.5.22 (leveranciersrelaties).
- **Vertrouwelijkheid**: De gap-analyse-resultaten zijn extreem gevoelig (kritieke infrastructuur). Markeer alle deliverables als 'VERTROUWELIJK - NIET DISTRIBUEREN' tot expliciete vrijgave door Lieke.
