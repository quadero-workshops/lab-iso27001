# Lab 02 - Asset Inventarisatie

**Time:** 25-45 minutes
**Skill:** `/classify-assets`
**Agent:** asset-analyst
**Workflow:** intake-en-normalisatie (stap 2)
**Goal:** Categoriseer de bevindingen uit Lab 01 plus de environment-documenten tot een gestandaardiseerde asset-inventarisatie volgens de 6 ISO 27001 asset-typen.

---

## Prerequisites

```bash
ls /workshop/state/iso27001/intake/parsed-notes.md 2>/dev/null || echo "MISSING - doe Lab 01 eerst"
```

## Setup

Je hebt `/workshop/state/iso27001/intake/parsed-notes.md` van Lab 01. Als die er niet is, doe Lab 01 eerst.

```bash
cd /workshop/
claude
```

---

## Steps

### 1. Verbreden van de input

Behalve de parsed-notes hebben we ook environment-documentatie die assets beschrijft:

```
Lees:
- /workshop/labs/lab-iso27001/starter-env/environment/it-landscape.md
- /workshop/labs/lab-iso27001/starter-env/environment/network-topology.md
- /workshop/labs/lab-iso27001/starter-env/environment/ot-architecture.md
- /workshop/labs/lab-iso27001/starter-env/environment/existing-controls.md

Geef per document een 3-bullet samenvatting van welke asset-categorieen erin terugkomen.
```

### 2. Classificeer assets met /classify-assets

```
/classify-assets

Classificeer alle assets uit de volgende bronnen tot een gestructureerde asset-inventarisatie van AquaPolder N.V.:

Bronnen:
- /workshop/state/iso27001/intake/parsed-notes.md (geparseerde bevindingen)
- /workshop/labs/lab-iso27001/starter-env/environment/it-landscape.md
- /workshop/labs/lab-iso27001/starter-env/environment/network-topology.md
- /workshop/labs/lab-iso27001/starter-env/environment/ot-architecture.md

Volg deze regels:

1. Categoriseer naar 6 asset-typen volgens team-iso27001 conventie:
   - HW (Hardware)
   - SW (Software, applicaties)
   - HR (Mensen, rollen)
   - PR (Processen, procedures)
   - LV (Leveranciers, derde partijen)
   - DA (Data, informatie-objecten)

2. Geef elk asset een uniek ID: [ASSET-<TYPE>-NNN] (bv. ASSET-HW-001, ASSET-SW-001).

3. Per asset vermeld:
   - Naam / korte beschrijving
   - Eigenaarschap (asset owner), of "Onbekend" indien niet vermeld
   - Criticality (1-5, met 5=bedrijfskritisch)
   - Locatie (logisch: Zwolle-DC / Apeldoorn-DC / Azure / Zuivering-X / SaaS / Mensen)
   - Bron-referentie (bv. [NOTA-012] of [it-landscape.md sectie 4])
   - Status indicator (Volledig gedocumenteerd / Onvolledig / Onbekend)

4. Bij onvoldoende informatie: markeer als "Onbekend - aanvullend onderzoek nodig" maar neem het asset wel op (in plaats van weglaten).

Sla op naar /workshop/state/iso27001/intake/asset-register.md.
```

Verwacht: 70-120 assets verspreid over de 6 typen.

### 3. Bouw asset-statistieken

```
Genereer /workshop/state/iso27001/intake/asset-stats.md met:

1. Telling per asset-type (HW, SW, HR, PR, LV, DA)
2. Telling per criticality-level (1, 2, 3, 4, 5)
3. Top-10 bedrijfskritische assets (criticality 5), met owner en bron
4. Lijst assets met owner "Onbekend" (deze worden in Lab 03 onderwerp van mapping naar A.5.9-A.5.13)
5. Lijst data-assets (DA-type) met indicatie van vertrouwelijkheids-niveau (gokt op basis van context, want classificatie ontbreekt - dit is een bekende gap)
```

### 4. Aandachts-tabel voor volgende labs

```
Maak /workshop/state/iso27001/intake/asset-attention.md met een korte tabel:

Voor elk van deze focus-onderwerpen die in latere labs terugkomen, lijst de gerelateerde asset-IDs:
- OT-systemen (SCADA, PLC, HMI) - Labs 03 en 05
- Leveranciers met systeem-toegang - Lab 03 en 05
- Identity en authentication - Lab 03 en 06
- Data-assets die mogelijk PII bevatten - Lab 04 en 06
- Logging- en monitoring-assets - Lab 03 en 06
```

### 5. Validatie

```
Cross-check: elke claim in /workshop/state/iso27001/intake/asset-register.md MOET een bronverwijzing hebben naar parsed-notes.md OF een environment-document. Identificeer assets zonder bron en plaats ze in een sectie "TO VERIFY".
```

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| 6 asset-typen geforceerd | ISO 27001 verwacht een complete inventarisatie, geen ad-hoc-lijst |
| Criticality-rating verbindt asset aan business-impact | Voorbereidt risk-assessment in Lab 06 |
| "Onbekend" wordt expliciet gemarkeerd ipv weglaten | Audit-conform: een gap zien is beter dan een gat verbergen |
| Data-assets zonder classificatie zijn een directe A.5.12 gap | Volgende lab gaat dit oppakken |

---

## Expected output

`/workshop/state/iso27001/intake/`:

- `asset-register.md` met 70-120 assets, [ASSET-TYPE-NNN] geid
- `asset-stats.md` met telling per type en criticality, top-10 bedrijfskritisch
- `asset-attention.md` met gefocuste sub-lijsten voor latere labs

Verwacht ratio's:
- HW: 25-40 assets (endpoints, servers, PLC's, HMI, sensors)
- SW: 15-25 assets (M365, SAP, WinCC OA, PI, klantportaal, etc.)
- HR: 8-15 (rollen en functies, niet individuele namen)
- PR: 10-20 (procedures, processen)
- LV: 8-12 (Siemens, MSSP, SAP-partner, Microsoft, etc.)
- DA: 10-20 (klantgegevens, financiele data, OT-procesdata, etc.)

## Success Criteria

- `asset-register.md` bestaat met >= 70 assets
- Elke asset heeft uniek [ASSET-TYPE-NNN] ID
- Elke asset heeft bron-verwijzing (geen "uit het niets")
- Criticality 1-5 toegekend (motiveerd voor de top-10)
- Assets met owner "Onbekend" zijn als zodanig gemarkeerd

---

**Next:** [Lab 03 - Annex A Mapping](lab-03-annex-a-mapping.md)
