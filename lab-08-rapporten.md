# Lab 08 - Drie Rapporten + Publish

**Time:** 25-45 minutes
**Skill:** `/create-executive-summary` + `/create-technical-report` + `/create-audit-report` + kernwaarden-bewaker
**Agents:** executive-translator + rapporteur + kernwaarden-bewaker
**Workflow:** rapportage-generatie + kwaliteitsreview
**Goal:** Stitch alle artefacten uit Labs 01-07 tot drie doelgroep-specifieke rapporten, kwaliteits-review ze, en publiceer ze.

---

## Prerequisites

Alle eerdere lab-deliverables moeten bestaan:
```bash
for d in intake gap-analyse multi-norm risk positioning; do
  ls /workshop/state/iso27001/$d/ >/dev/null 2>&1 || echo "MISSING /workshop/state/iso27001/$d/ - doe Lab 01-07 eerst"
done
```

## Setup

Je hebt onder `/workshop/state/iso27001/` de volgende artefacten:

- `intake/parsed-notes.md` + `asset-register.md` + `asset-stats.md` (Lab 01-02)
- `gap-analyse/annex-a-mapping.md` + `gap-detail.md` + `soa-concept.md` + `top-20-gaps.md` + `maturity-scores.md` (Lab 03-04)
- `multi-norm/iec62443-assessment.md` + `nis2-assessment.md` + `consolidated-gaps.md` (Lab 05)
- `risk/risk-register.md` + `risk-matrix.md` + `keten-risico.md` (Lab 06)
- `positioning/attck-mapping.md` + `menukaart.md` + `prioritering.md` (Lab 07)

De Quadero rapport-template staat in het team-iso27001 scaffold: `/workshop/templates/reports/quadero-report-template.html` (+ CSS sibling). De PDF-generator is `/workshop/scripts/generate_report.py`, usage: `python generate_report.py <input.html> -o <output.pdf>`.

### Workshop platform commands gebruikt in deze lab

Deze lab gebruikt twee workshop-platform commando's die in eerdere labs nog niet aan bod kwamen:

- `publish` - kopieert een gerenderd rapport naar `/workshop/published/` zodat het zichtbaar is op je seat-URL.
- `workshop-export` - zipt de complete `/workshop/state/` directory voor take-home.

Zie de deelnemer-handleiding voor de volledige commando-referentie.

```bash
cd /workshop/
claude
```

---

## Steps

### 1. Outline en inventory

```
Lijst alle artefacten in /workshop/state/iso27001/. Groepeer ze per rapport-doelgroep:

- **Technisch rapport** (IT-managers): gap-detail, soa-concept, attck-mapping, iec62443-assessment, asset-register subset (HW/SW/Netwerk), risk-register, keten-risico
- **Executive summary** (Directie/CFO): top-20-gaps, maturity-scores, risk-matrix, menukaart, prioritering
- **Audit rapport** (Externe auditor): annex-a-mapping (volledig), soa-concept, nis2-assessment, evidence-mapping

Schrijf de outline naar /workshop/state/iso27001/rapportage/outline.md met section-tree voor elk rapport.
```

### 2. Executive summary met /create-executive-summary

```
/create-executive-summary

Schrijf de Executive Summary voor de directie van AquaPolder N.V. (CEO Anouk Verheijen + CFO Thomas Hertzog). Gebruik de executive-translator agent.

Constraints:
- Max 5 paginas (ongeveer 1800 woorden)
- Geen technisch jargon zonder uitleg
- Refereer NIET aan stakeholders bij naam, refereer aan rol
- Open met de "one decision the CEO needs to make next"
- Sluit af met de drie scenario's (do-nothing / minimum-NIS2 / volledige-ISO-cert) met indicatieve kosten en risico-reductie

Structuur:
1. Aanleiding (1 alinea): NIS2 + ISO-cert ambitie
2. Bevindingen op hoofdlijn (max 1 paginas): top-5 bevindingen, traffic-light per Annex A domein, maturity-scores t.o.v. branche-benchmark
3. Risico (1 paginas): top-5 risico's met L x I, totale risico-exposure (kwalitatief)
4. Voorgestelde aanpak (1-2 paginas): bundels van menukaart, totale investering 2026-2027, key-decisions voor bestuur
5. Conclusie en aanbeveling (0.5 paginas)

Bronnen: top-20-gaps.md, maturity-scores.md, risk-matrix.md, menukaart.md, prioritering.md.

Sla op naar /workshop/state/iso27001/rapportage/executive-summary.md.

CRITISCH: Geen ongedekte claims. Elke bewering MOET traceerbaar zijn naar een [NOTA-XXX], [GAP-XXX] of [RISK-XXX].
```

### 3. Technisch rapport met /create-technical-report

```
/create-technical-report

Schrijf het technisch rapport voor de IT-managers (CIO Lieke + OT-manager Robert). Gebruik de executive-translator agent. Max 50 paginas exclusief bijlagen.

Structuur:
1. Inleiding en scope (2 paginas)
2. IT-architectuur en omgeving (3-5 paginas, op basis van it-landscape + network-topology)
3. OT-architectuur en omgeving (3-5 paginas)
4. Gap-analyse per Annex A domein (15-25 paginas, dit is het hart van het rapport)
   - A.5 (Organizational): top-20 controls met gap en remediation
   - A.6 (People)
   - A.7 (Physical)
   - A.8 (Technological)
5. IEC 62443 OT-beoordeling (3-5 paginas)
6. ATT&CK positionering (2-3 paginas)
7. Risico-overzicht (2-3 paginas)
8. Remediation-roadmap technisch (3-5 paginas, koppelt aan menukaart blokken)
9. Bijlagen (asset-register, evidence-mapping, etc.)

Stijl: technisch precies, met implementatie-aanbevelingen. Refereer aan concrete tools, configuraties, en patroon-architecturen.

Sla op naar /workshop/state/iso27001/rapportage/technisch-rapport.md.
```

### 4. Audit rapport met /create-audit-report

```
/create-audit-report

Schrijf het audit rapport voor externe auditors (toekomstige ISO 27001-certificeringsauditor en de NIS2-cyber-toezichthouder RDI). Gebruik de executive-translator + compliance-auditor agent (cross-check).

Structuur volgens ISO 27001:2022 clausuur-structuur:
1. Document control (versie, datum, scope, vertrouwelijkheid)
2. Executive Summary (1 paginas, formeel)
3. Methodology
4. Compliance status per clausuur 4-10 (max 1 pagina per clausuur)
5. Compliance status per Annex A domein (cijfermatige overzicht)
6. Statement of Applicability (volledige tabel, alle 93 controls)
7. Non-conformiteits-rapporten (NCR), genummerd, met evidence
8. Compliance-percentages per domein
9. NIS2 zorgplicht-toets (Artikel 21 per onderdeel)
10. Aanbeveling certificeerbaarheid (indicatief, geen formele NC categorisatie)

Stijl: formeel, neutraal, evidence-based. Geen advisory-taal ("we adviseren") maar feitelijke observaties ("control X.Y is niet geimplementeerd, evidence Z toont aan dat...").

Sla op naar /workshop/state/iso27001/rapportage/audit-rapport.md.
```

### 5. Kwaliteitsreview met kernwaarden-bewaker

```
Laat kernwaarden-bewaker alle drie de rapporten reviewen:

Voor elk rapport, valideer:
1. Elke bewering is traceerbaar naar een artefact in /workshop/state/iso27001/
2. Geen overpromise-taal ("garandeert", "volledig", "100% veilig")
3. Geen ongedekte aanbevelingen (elk advies moet een gap of risico adresseren)
4. Consistentie tussen rapporten:
   - Aantallen NCs, gaps, risico's komen overeen
   - Maturity-scores zijn identiek
   - Financiele bedragen consistent
5. Quadero kernwaarden:
   - Mensen maken het verschil (concreet, niet abstract)
   - Blijf jezelf uitdagen (eerlijke gap-erkenning)
   - Samen uit, samen thuis (eigenaarschap helder)
   - Geef het kleur (concreet en specifiek)
   - Handel alsof het van jou is (professioneel, niet vrijblijvend)

Schrijf de review naar /workshop/state/iso27001/rapportage/kernwaarden-review-rapporten.md. Lijst eventuele aanpassingen die in de rapporten doorgevoerd moeten worden.
```

Pas de review terug op de rapporten als nodig.

### 6. Render naar HTML met Quadero-template

```
Read /workshop/templates/reports/quadero-report-template.html en de CSS sibling om de template's variabelen en structuur te begrijpen.

Render de drie rapporten naar HTML:
- /workshop/state/iso27001/rapportage/build/executive-summary.html
- /workshop/state/iso27001/rapportage/build/technisch-rapport.html
- /workshop/state/iso27001/rapportage/build/audit-rapport.html

Als de template een placeholder-systeem gebruikt ({{ title }}, {{ body }}, etc.), populeer die. Als er een Python-generator script bestaat in /workshop/scripts/, gebruik dat.

Behoud de markdown-bronnen naast de HTML.
```

### 7. Publish

```
Publiceer de rapporten naar de workshop-share zodat collega's ze kunnen bekijken:

publish /workshop/state/iso27001/rapportage/build

Controleer met `ls /workshop/published/` dat de rapporten zijn geplaatst, en open de URL die de publish-command teruggeeft in een nieuwe browser-tab.

Maak ook een index.html die naar alle drie de rapporten linkt (executive-summary, technisch-rapport, audit-rapport).
```

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| Doelgroep-bewust schrijven: zelfde gap, andere woorden | Directie leest niet wat IT leest, en wat auditor leest is weer anders |
| kernwaarden-bewaker reviewt het eindpakket, niet losse secties | Kwaliteit komt door deliverable-level review |
| Cross-rapport-consistentie checken | Auditor merkt het direct als getallen niet kloppen tussen rapporten |
| `publish` is de officiele route naar gedeelde output | Schrijven naar `/workshop/published/` direct is geen conventie |

---

## Expected output

`/workshop/state/iso27001/rapportage/`:

- `outline.md`
- `executive-summary.md` (max 5 paginas)
- `technisch-rapport.md` (max 50 paginas)
- `audit-rapport.md` (formeel, met SoA)
- `kernwaarden-review-rapporten.md`
- `build/` met drie HTML-versies + index.html

Plus: `/workshop/published/<seat-id>/` bevat de rapporten, zichtbaar op `apps.workshop.quaderolabs.com/<jouw-seat>/`.

## Success Criteria

- Drie rapporten bestaan in markdown en HTML
- Executive summary <= 5 paginas
- Audit rapport bevat een complete SoA-tabel met alle 93 controls
- Kernwaarden-review uitgevoerd en aanpassingen verwerkt
- Rapporten zijn gepubliceerd en bereikbaar via de seat-URL
- Cross-rapport getallen (gaps, NCs, risk-counts) zijn consistent

---

## Wrap-up

Optioneel: run `workshop-export` om alles in `/workshop/state/iso27001/` als zip naar `/workshop/state/exports/` te zetten voor take-home.

```bash
workshop-export iso27001
```

Je hebt nu een volledig engagement-kwaliteits deliverable-pakket gegenereerd via het team-iso27001 agents en skills. Hetzelfde patroon - een team-config, een doorlopend scenario, gelinkte skills die elkaar's output gebruiken - is hoe je een echte Quadero ISO 27001-engagement zou draaien.
