# Lab 06 - Risicobeoordeling

**Time:** 25-45 minutes
**Skill:** `/assess-risk`
**Agent:** risk-assessor
**Workflow:** risicobeoordeling
**Goal:** Voer een ISO 27005-conforme risicobeoordeling uit op basis van de gaps, assets, en incident-historie. Lever een risk register met L x I scoring en behandel-voorstellen.

---

## Prerequisites

```bash
for f in gap-analyse/top-20-gaps.md multi-norm/consolidated-gaps.md intake/asset-register.md; do
  ls /workshop/state/iso27001/$f 2>/dev/null || echo "MISSING /workshop/state/iso27001/$f - doe Lab 02/04/05 eerst"
done
```

## Setup

Je hebt nodig:
- `/workshop/state/iso27001/gap-analyse/top-20-gaps.md` (Lab 04)
- `/workshop/state/iso27001/multi-norm/consolidated-gaps.md` (Lab 05)
- `/workshop/labs/lab-iso27001/starter-env/context/incidenten-2025.md`
- `/workshop/state/iso27001/intake/asset-register.md`

```bash
cd /workshop/
claude
```

---

## Steps

### 1. Stel dreigingslandschap vast

```
Op basis van /workshop/labs/lab-iso27001/starter-env/context/incidenten-2025.md plus actuele dreigings-informatie voor de drinkwater-sector, stel een korte threat-landscape vast (1 paginas):

1. Dreigingsactoren relevant voor AquaPolder:
   - Opportunistische cybercriminelen (ransomware, phishing-campagnes)
   - Geadvanceerd-georganiseerde criminelen (financieel gemotiveerd, BEC)
   - State-sponsored / APT (low likelihood maar high impact gegeven kritieke infra)
   - Insider threat (klein, gedeeld account-misbruik)

2. Top-10 dreigingsscenario's specifiek voor AquaPolder. Voorbeelden om mee te starten:
   - Ransomware op SAP -> facturatie en betaling 1-2 weken stuk
   - Lateral movement via Siemens-account -> SCADA-manipulatie -> waterkwaliteit-incident
   - Klantdata-lek uit klantportaal -> privacy-incident + reputatie
   - BEC met factuur-manipulatie -> financieel verlies
   - DDoS op klantportaal -> reputatieschade
   - Insider-misbruik SAP_ALL -> financiele fraude
   - Sub-processor-compromise (MSSP) -> toegang tot Sentinel-data
   - OT-vendor-toegang-misbruik (Siemens-account zoals Q3 2025 maar dan geslaagd)
   - Phishing-credential-theft -> M365-mailbox-takeover
   - Fysieke toegang zuiveringen -> sabotage drinkwaterproductie

3. Per scenario: kwalitatieve initiele likelihood (1-5) en impact (1-5).

Sla op naar /workshop/state/iso27001/risk/threat-landscape.md.
```

### 2. Risk register met /assess-risk

```
/assess-risk

Bouw een risk register voor AquaPolder conform ISO 27005. Gebruik de risk-assessor agent.

Bronnen:
- /workshop/state/iso27001/risk/threat-landscape.md
- /workshop/state/iso27001/gap-analyse/top-20-gaps.md
- /workshop/state/iso27001/multi-norm/consolidated-gaps.md
- /workshop/state/iso27001/intake/asset-register.md (criticality info)

Voor elk geidentificeerd risico:

1. **Risk-ID**: RISK-001, RISK-002, etc.
2. **Beschrijving**: 1-3 zinnen
3. **Asset(s) geraakt**: [ASSET-XX-NNN] referenties
4. **Gerelateerde gap(s)**: GAP-XXX referenties
5. **Dreigingsbron**: actor-type (zie threat-landscape)
6. **Likelihood (L)**: 1-5 met onderbouwing
   - 1 = Zeer onwaarschijnlijk
   - 2 = Onwaarschijnlijk
   - 3 = Mogelijk
   - 4 = Waarschijnlijk
   - 5 = Vrijwel zeker
7. **Impact (I)**: 1-5 met onderbouwing
   - 1 = Verwaarloosbaar (< 10k EUR equivalent)
   - 2 = Klein (10-100k)
   - 3 = Gemiddeld (100k-1M)
   - 4 = Groot (1-10M)
   - 5 = Catastrofaal (> 10M of imago-vernietigend)
   - LET OP: voor drinkwater is een gezondheids-incident automatisch I=5
8. **Inherent risicoscore**: L x I (1-25)
9. **Bestaande controls**: welke maatregelen mitigeren al?
10. **Residual risicoscore**: L x I na bestaande controls
11. **Risk treatment**:
    - Accept (residual < 6 EN binnen risico-appetit)
    - Mitigate (residual >= 6, voorgestelde aanvullende maatregel)
    - Transfer (verzekering / outsourcing)
    - Avoid (activiteit staken)
12. **Eigenaar** (welke stakeholder is verantwoordelijk?)

Genereer minimaal 20-30 risico's verspreid over IT, OT, en organisatorisch.

Sla op naar /workshop/state/iso27001/risk/risk-register.md.
```

### 3. Bouw de risicomatrix

```
Genereer /workshop/state/iso27001/risk/risk-matrix.md met:

Een 5x5 likelihood-impact tabel waarin elke risico-ID in de juiste cel staat (inherent EN residueel als 2 aparte matrices).

Markeer met kleuren / labels:
- Laag (score 1-4): groen, "Accept"
- Gemiddeld (5-9): geel, "Mitigate of accept met monitoring"
- Hoog (10-15): oranje, "Mitigate"
- Kritiek (16-25): rood, "Mitigate met urgentie"

Voor elk Hoog/Kritiek risico in de residuele matrix:
- Onderbouw waarom het zo hoog blijft ondanks bestaande controls
- Wat is de minimale aanvullende maatregel om naar Gemiddeld te zakken?

Verwacht: 2-5 Kritiek + 5-10 Hoog risico's. Indien je minder hebt, herzie je onderbouwingen - dit zou ongebruikelijk laag zijn voor een NIS2-essentiele entiteit met ISMS in deze maturity.
```

### 4. Ketenrisico-analyse (NIS2 Artikel 21 lid 2(d))

```
Genereer /workshop/state/iso27001/risk/keten-risico.md met een specifieke analyse van leveranciers-risico's. Per Tier 1-leverancier (Siemens, CyberWatch, DeltaConsult, Microsoft, Equinix):

1. Toegang/afhankelijkheid: wat doet deze leverancier voor AquaPolder?
2. Toegangs-niveau tot systemen/data
3. Bekende incidenten of zwakheden (Q3 2025 incident voor Siemens)
4. Mitigations in plaats (contractueel + technisch)
5. Restrisico-score
6. Aanbevolen verbeteringen
```

### 5. Quality-review met kernwaarden-bewaker

```
Laat kernwaarden-bewaker de risk-register reviewen:

- Worden risico-scores onderbouwd met evidence?
- Zijn er risico's "stiekem omhoog gebracht" om verkoop te rechtvaardigen?
- Zijn er risico's onderschat ondanks duidelijke bewijzen in incident-log?
- Risk treatment: wordt "Mitigate" overgebruikt waar "Accept" verdedigbaar is?

Schrijf naar /workshop/state/iso27001/risk/kernwaarden-review-risk.md.
```

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| Inherent EN residueel risico onderscheiden | ISO 27005 vereist beide om de waarde van bestaande controls te tonen |
| Drinkwater-specifieke impact-schaal | Gezondheids-incident = automatisch I=5 (niet onderhandelbaar) |
| Ketenrisico apart behandeld | NIS2 Artikel 21 lid 2(d) eist dit expliciet |
| Risk-treatment-voorstellen koppelen naar gaps en controls | Onderbouwing voor Lab 07's roadmap |

---

## Expected output

`/workshop/state/iso27001/risk/`:

- `threat-landscape.md` met 10 dreigingsscenario's
- `risk-register.md` met 20-30 risico's, L x I scoring, treatment voorstellen
- `risk-matrix.md` met 5x5 matrix (inherent + residueel)
- `keten-risico.md` met leveranciers-risico-analyse
- `kernwaarden-review-risk.md` met de review

Verwachte top-risico's voor AquaPolder:
- Ransomware op SAP (waarschijnlijk Hoog of Kritiek)
- OT-vendor-toegang-misbruik (Hoog gezien Q3 2025 precedent)
- Klantdata-lek / klantportaal compromise (Hoog vanwege reputatie)
- BEC factuur-fraude (Gemiddeld-Hoog)
- Gezondheid-impact via waterkwaliteit-manipulatie (Catastrofaal impact, lage likelihood = Hoog)

## Success Criteria

- Minimaal 20 risico's in register
- Elk risico heeft expliciete L x I onderbouwing
- Risk-matrix toont inherent EN residueel
- Ketenrisico-analyse adresseert alle Tier 1-leveranciers
- Geen Kritiek-risico zonder bestemming in Lab 07's roadmap

---

**Next:** [Lab 07 - ATT&CK Positionering + Menukaart](lab-07-attck-menukaart.md)
