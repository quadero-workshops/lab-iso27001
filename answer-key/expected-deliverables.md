# Expected Deliverables - lab-iso27001

Per-lab inventaris van wat onder `/workshop/state/iso27001/` moet bestaan na succesvolle uitvoering. Gebruik tijdens begeleide sessies om snel te valideren of een cursist klaar is voor de volgende lab.

> **Niet voor cursisten** - dit bestand is voor trainers/reviewers.

---

## Na Lab 01 - Engagement Kickoff

```
/workshop/state/iso27001/intake/
  parsed-notes.md            # opgeschoonde + geclassificeerde interview-notities
  stakeholder-map.md         # rollen + zorgen per stakeholder
  scope-statement.md         # in/out scope met expliciete onderbouwing
```

Quality markers:
- parsed-notes bevat traceerbare verwijzingen [NOTA-XXX] per claim
- Geen anonieme uitspraken - elke claim heeft een bron
- Lieke, Karin, Robert, Thomas, Niek en Anouk komen alle 6 in stakeholder-map voor
- Q3 2025 phishing-incident is als context, niet als scope-item opgenomen

## Na Lab 02 - Asset Inventarisatie

```
/workshop/state/iso27001/intake/
  asset-register.md          # alle assets met classificatie
  asset-stats.md             # totalen per type/klasse
```

Quality markers:
- Asset-register bevat IT (M365, Azure, SAP, klantportaal) EN OT (WinCC OA, PI System, 17 PLCs, HMIs)
- Confidentiality/Integrity/Availability classificatie per asset
- Eigenaar genoemd per asset (verwijzing naar interview)
- `[ASSET-XXX]` referenties consistent (genummerd, geen gaten)

## Na Lab 03 - Annex A Mapping

```
/workshop/state/iso27001/gap-analyse/
  annex-a-mapping.md         # alle 93 controls met voorlopige status
  soa-concept.md             # Statement of Applicability concept
```

Quality markers:
- Alle 93 Annex A controls aanwezig (A.5=37, A.6=8, A.7=14, A.8=34)
- Voorlopige status: Volledig / Deels / Niet / Niet Beoordeelbaar
- Elke claim heeft [NOTA-XXX] of [ASSET-XXX] verwijzing
- Conservatieve houding: bij twijfel "Deels" of "Niet Beoordeelbaar", niet "Volledig"

## Na Lab 04 - Gap-Analyse + Maturity

```
/workshop/state/iso27001/gap-analyse/
  gap-detail.md              # formele beoordeling per control
  maturity-scores.md         # CMM per domein + totaal
  top-20-gaps.md             # geprioriteerde gap-lijst
  isms-quality-review.md     # isms-architect kwaliteitsreview
```

Quality markers:
- gap-detail heeft formele status + gap-beschrijving + remediation-richting + evidence-quote per control
- maturity-scores: A.5 (1.0-1.5), A.6 (1.5-2.0), A.7 (2.0-2.5), A.8 (1.8-2.5), totaal (1.5-2.0) - orde van grootte
- top-20-gaps onderscheidt quick wins (<= 4 weken, <= 25k EUR) van structurele projecten
- Hypothetische sector-benchmark expliciet als aanname gemarkeerd (zie lab-04 fix in commit 94a1504)

## Na Lab 05 - Multi-Norm (IEC 62443 + NIS2)

```
/workshop/state/iso27001/multi-norm/
  iec62443-assessment.md     # OT-onderdeel tegen IEC 62443 FRs
  nis2-assessment.md         # NIS2 zorgplicht + meldplicht + registratie + bestuurdersaansprakelijkheid
  consolidated-gaps.md       # overlap-zicht tussen ISO/IEC/NIS2
```

Quality markers:
- NIS2-assessment dekt artikel 21 lid 2 (a-i, 9 onderdelen) consistent
- NCSC + RDI worden correct genoemd als CSIRT/toezichthouder (NIET CSIRT-Drinkwater, NIET ILT voor cyber-toezicht)
- IEC 62443 FRs benoemd, niet ALL maar minimaal FR 1/2/3/5/7 met OT-context
- consolidated-gaps maakt expliciet welke gaps tegelijk meerdere normen raken (waarde-argument voor klant)

## Na Lab 06 - Risicobeoordeling

```
/workshop/state/iso27001/risk/
  risk-register.md           # risico's per ISO 27005 methodologie
  risk-matrix.md             # L x I matrix
  keten-risico.md            # leveranciers / sub-processors / Bird&Co / Siemens
```

Quality markers:
- ISO 27005-methodologie expliciet (likelihood x impact, schaal benoemd)
- Risk-register bevat minimaal 12-20 risico's met RISK-XXX nummering
- Q3 2025 phishing-incident is een gerealiseerd risico, NIET als toekomstig risico
- Keten-risico noemt Siemens (gedeeld support-account), Bird&Co (klantportaal), MSSP CyberWatch, DeltaConsult (SAP_ALL)

## Na Lab 07 - ATT&CK Positionering + Menukaart

```
/workshop/state/iso27001/positioning/
  attck-mapping.md           # Enterprise + ICS matrix-dekking
  heatmap.md                 # defense-in-depth per tactiek
  menukaart.md               # 6-10 vervolgblokken met prijzen
  prioritering.md            # Q2/Q3-Q4/2027 bundels
  kernwaarden-review-menukaart.md
```

Quality markers:
- ATT&CK Enterprise (14 tactics) + ICS (12 tactics) beide aanwezig
- ATT&CK dreigingsactoren onderbouwd met evidence uit risk-register/incidenten-2025 (zie lab-07 fix in commit 94a1504)
- Menukaart: 6-10 blokken, elk met traceerbare GAP-XXX + RISK-XXX referenties
- Prioritering respecteert quick win drempel (<= 4 weken / <= 25k EUR)
- Totaal: 350-650k EUR voor 2026, 500-800k EUR voor 2027 (orde van grootte)

## Na Lab 08 - Drie Rapporten + Publish

```
/workshop/state/iso27001/report/
  executive-summary.md       # max 5 pagina's, voor directie
  technical-report.md        # 30-50 pagina's, voor IT-management
  audit-report.md            # formeel, voor certificeringsauditor
  soa-final.md               # volledige Statement of Applicability
  build/                     # HTML + PDF renders
  kernwaarden-review.md
```

Quality markers:
- Drie rapporten zijn echt verschillend (niet 3 versies van hetzelfde)
- Executive summary <= 5 paginas, geen jargon, eindigt met 1 beslissing voor bestuur
- Audit-report verwijst naar RDI (NIS2-cyber-toezicht) niet ILT
- PDF gerenderd via `python /workshop/scripts/generate_report.py`
- `publish` commando uitgevoerd, output zichtbaar in `/workshop/published/`
