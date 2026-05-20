# Evaluation Rubric - lab-iso27001

Snelle pass/fail-checklist per lab voor trainers/reviewers. Voor de volledige verwachte deliverables zie `expected-deliverables.md`.

> **Niet voor cursisten** - dit bestand is voor trainers/reviewers.

---

## Lab 01 - Engagement Kickoff
- [ ] Alle 6 stakeholders genoemd in stakeholder-map (Lieke, Karin, Robert, Thomas, Niek, Anouk)
- [ ] Scope is expliciet (in/out met onderbouwing)
- [ ] Q3 2025 phishing-incident is context, geen scope-item
- [ ] parsed-notes heeft traceerbare [NOTA-XXX] verwijzingen

## Lab 02 - Asset Inventarisatie
- [ ] Asset-register bevat IT (M365, Azure, SAP, klantportaal) EN OT (WinCC OA, PI System, ~17 PLCs, HMIs)
- [ ] Confidentiality/Integrity/Availability per asset
- [ ] Eigenaar genoemd per asset
- [ ] [ASSET-XXX] genummerd zonder gaten

## Lab 03 - Annex A Mapping
- [ ] Alle 93 controls aanwezig (A.5=37, A.6=8, A.7=14, A.8=34)
- [ ] Status conservatief (geen "Volledig" zonder evidence)
- [ ] Elke claim heeft [NOTA-XXX] of [ASSET-XXX] verwijzing
- [ ] "Niet Beoordeelbaar" wordt gebruikt waar evidence ontbreekt (niet ingevuld met aannames)

## Lab 04 - Gap-Analyse + Maturity
- [ ] Formele status per control: Volledig / Deels (XX%) / Niet / Niet Beoordeelbaar
- [ ] Maturity-scores per domein binnen orde-van-grootte (totaal 1.5-2.0)
- [ ] Top-20 gaps onderscheidt quickwin / structureel / strategisch
- [ ] Branche-benchmark expliciet als aanname/illustratief gemarkeerd

## Lab 05 - Multi-Norm (IEC 62443 + NIS2)
- [ ] NIS2 artikel 21 lid 2 (a-i, 9 onderdelen) volledig behandeld
- [ ] Toezicht correct: cyber -> RDI, drinkwaterveiligheid -> ILT
- [ ] Meldplicht naar NCSC (NIET CSIRT-Drinkwater, NIET ILT)
- [ ] IEC 62443 dekt minimaal FR 1/2/3/5/7 met OT-context
- [ ] consolidated-gaps maakt multi-norm-overlap zichtbaar

## Lab 06 - Risicobeoordeling
- [ ] ISO 27005-methodologie expliciet (L x I schaal benoemd)
- [ ] 12-20 risico's met RISK-XXX nummering
- [ ] Q3 2025 phishing als gerealiseerd risico (niet toekomstig)
- [ ] Keten-risico noemt Siemens, Bird&Co, CyberWatch, DeltaConsult
- [ ] Risico-rating consistent (geen "kritiek" zonder evidence)

## Lab 07 - ATT&CK + Menukaart
- [ ] Enterprise (14) + ICS (12) tactics behandeld
- [ ] Dreigingsactoren onderbouwd met evidence uit risk-register/incidenten-2025
- [ ] Menukaart: 6-10 blokken met GAP-XXX + RISK-XXX traceerbaarheid
- [ ] Quickwin-drempel (<= 4 wk, <= 25k EUR) gerespecteerd
- [ ] Totaal-orde 350-650k EUR (2026) + 500-800k EUR (2027)

## Lab 08 - Drie Rapporten + Publish
- [ ] Drie inhoudelijk verschillende rapporten (niet 3 versies van 1)
- [ ] Executive summary <= 5 pag, geen jargon, eindigt op 1 bestuursbeslissing
- [ ] Audit-rapport noemt RDI als cyber-toezichthouder
- [ ] PDF gerenderd via `generate_report.py`
- [ ] `publish` uitgevoerd, file zichtbaar in `/workshop/published/`
- [ ] kernwaarden-bewaker review toegepast op finale rapporten

---

## Common-fault checklist (rode vlaggen bij review)

Negatieve indicatoren - signalen dat cursist te snel doorgereisd is of Claude te gretig laten invullen:

- Claims zonder bronverwijzing
- "Volledig" status zonder evidence-citaat
- ATT&CK dreigingsactoren als feit gepresenteerd zonder AquaPolder-onderbouwing
- Vewin-sectorrapport benchmarks niet als aanname gemarkeerd
- "CSIRT-Drinkwater" of "ILT" als cyber-toezichthouder (zou allebei moeten zijn opgeschoond)
- Vitens als data-uitwisselingspartner (zou Vewin/WaterISAC moeten zijn)
- "120 PLC's" zonder onderbouwing (correct: ~17 hoofdcontrollers + honderden remote-IO)
- Equinix als production-DC (is offsite-backup)
- Tijdschattingen onhaalbaar: cursist die in 10 min "93 controls gemapt" claimt - check de kwaliteit
- Rapport overpromise: "fully compliant", "guaranteed", "complete"
