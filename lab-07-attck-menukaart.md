# Lab 07 - ATT&CK Positionering + Menukaart

**Time:** 25-45 minutes
**Skill:** `/map-mitre-attack` + `/generate-menukaart`
**Agent:** attack-mapper + remediation-planner
**Workflow:** positionering-en-menukaart
**Goal:** Plot bestaande controls op MITRE ATT&CK matrix om dekkings- en diepte-gaten te tonen, en vertaal dit naar een geprioriteerde menukaart met vervolgblokken inclusief indicatieve prijzen.

---

## Prerequisites

```bash
for f in risk/risk-register.md gap-analyse/top-20-gaps.md; do
  ls /workshop/state/iso27001/$f 2>/dev/null || echo "MISSING /workshop/state/iso27001/$f - doe Lab 04/06 eerst"
done
```

## Setup

Je hebt nodig:
- `/workshop/state/iso27001/risk/risk-register.md` (Lab 06)
- `/workshop/state/iso27001/gap-analyse/top-20-gaps.md` (Lab 04)
- `/workshop/labs/lab-iso27001/starter-env/environment/existing-controls.md`
- `/workshop/labs/lab-iso27001/starter-env/context/incidenten-2025.md`

```bash
cd /workshop/
claude
```

---

## Steps

### 1. MITRE ATT&CK Positionering

```
/map-mitre-attack

Plot AquaPolder's bestaande controls op de MITRE ATT&CK Enterprise en ICS matrices. Gebruik de attack-mapper agent.

Bron-input:
- /workshop/labs/lab-iso27001/starter-env/environment/existing-controls.md
- /workshop/labs/lab-iso27001/starter-env/context/incidenten-2025.md (relevante TTPs)
- /workshop/state/iso27001/risk/risk-register.md (dreigingsactoren en scenario's)

Voor de Enterprise matrix:

1. **Dekking per tactiek**: voor elk van de 14 ATT&CK Enterprise tactics (Initial Access t/m Impact), bereken dekkingspercentage (welke technieken zijn afgedekt door bestaande controls).

2. **Diepte per techniek**: voor de top-20 meest relevante techniques (gegeven dreigingsactoren), beoordeel diepte:
   - Preventie aanwezig (Defender, MFA, segmentatie, etc.)
   - Detectie aanwezig (Sentinel rule, MSSP alert)
   - Respons-procedure aanwezig (IR-runbook)

3. **Blinde vlekken**: top-10 techniques die noch preventie noch detectie hebben.

Voor de ICS matrix (relevant voor OT-zijde):

1. **Tactiek-dekking**: 12 ICS tactics (Initial Access, Execution, Persistence, Privilege Escalation, Evasion, Discovery, Lateral Movement, Collection, Command and Control, Inhibit Response Function, Impair Process Control, Impact)

2. Identificeer relevante techniques die voor AquaPolder van toepassing zijn (drinkwater + Siemens + WinCC OA)

3. Beoordeel dekking: vrijwel geen voor OT (zoals interview-02 al aangaf).

Voorbeelden van dreigingsactor-categorieen om mee te koppelen (cursist motiveert de selectie op basis van risk-register en `incidenten-2025.md` - niet als gegeven feit overnemen):
- TA505 / Pikabot - financiele criminaliteit, BEC en ransomware-deployment (relevant gezien Q3 2025 phishing-incident en SAP_ALL toegang)
- Volt Typhoon - state-sponsored, kritieke-infra targeting (relevant: drinkwater = Annex I, NIS2-essentiele entiteit)
- Sandworm - ICS-gerichte aanvallen, NL-relevant 2024 (relevant gezien Siemens-SCADA stack en gedeeld Siemens-account)
- Insiders - opportunistisch credential-misbruik (relevant gezien SAP_ALL, gedeelde OT-operator-accounts, gedeeld Siemens-support-account)

Cursist mag andere actoren toevoegen of bovenstaande verwerpen mits onderbouwd.

Sla op naar /workshop/state/iso27001/positioning/attck-mapping.md. Maak een visuele weergave (markdown-tabel of ASCII) van de matrices.
```

### 2. Defense-in-Depth heatmap

```
Genereer /workshop/state/iso27001/positioning/heatmap.md met:

Een tabel waarin de rijen Enterprise-tactics zijn en kolommen "Preventie", "Detectie", "Respons" zijn. Vul per cel:
- Green: volledige dekking
- Yellow: gedeeltelijke dekking
- Red: geen dekking

Plus een vergelijkbare heatmap voor ICS-tactics.

Conclusie-paragraaf (max 5 zinnen): waar staat AquaPolder defense-in-depth-mature?
```

### 3. Genereer menukaart met /generate-menukaart

```
/generate-menukaart

Genereer de remediation-menukaart voor AquaPolder. Gebruik de remediation-planner agent.

Bronnen:
- /workshop/state/iso27001/gap-analyse/top-20-gaps.md
- /workshop/state/iso27001/multi-norm/consolidated-gaps.md
- /workshop/state/iso27001/risk/risk-register.md
- /workshop/state/iso27001/positioning/attck-mapping.md

Lever 6-10 vervolgblokken. Per blok:

1. **Blok-ID**: BLOK-001, etc.
2. **Naam** (zakelijk, niet technisch): bv. "OT-zichtbaarheid en respons", "Identity-fundament", "Awareness-programma 2026"
3. **Beschrijving** (3-5 zinnen): wat zit er in dit blok, wat is het concrete resultaat
4. **Gaps geadresseerd**: GAP-XXX referenties (mogelijk meerdere)
5. **Risico's geadresseerd**: RISK-XXX referenties (mogelijk meerdere)
6. **Normen-dekking**: ISO 27001 controls + IEC 62443 FRs + NIS2 artikelen
7. **Indicatieve prijs** (range, bv. 25-45k EUR voor een blok)
8. **Doorlooptijd**: bv. 6-10 weken
9. **Afhankelijkheden**: andere blokken die voor of na moeten
10. **Categorisatie**: Quick win / Tactisch / Strategisch
11. **ROI-onderbouwing** (2-3 zinnen): wat is de waarde voor de klant?

Voorbeelden van blokken die je verwacht (verifieer of ze passen):
- BLOK: ISMS-fundament (governance, scope, beleid) - structureel
- BLOK: OT passieve monitoring - tactisch
- BLOK: Identity-hardening (SAP_ALL split, OT-domein MFA) - quick win + tactisch
- BLOK: Leveranciersbeheer NIS2-conform - tactisch
- BLOK: Awareness-programma 2026 (kwartaal-fishing-sim + training) - quick win
- BLOK: Incident response geoefend (tabletop, runbook-update) - quick win
- BLOK: OT-segmentatie en data diode - strategisch
- BLOK: SAP-monitoring naar Sentinel - tactisch
- BLOK: PLC-versie-management en backup-discipline - tactisch
- BLOK: Pre-certificeerings-audit ISO 27001 - strategisch

Totale orde-van-grootte: 6-10 blokken, samen 350-650k EUR voor 2026, 500-800k EUR voor 2027 (OT-zwaarder).

Sla op naar /workshop/state/iso27001/positioning/menukaart.md.
```

### 4. Prioriteits-route

```
Genereer /workshop/state/iso27001/positioning/prioritering.md:

1. **Quickwin-bundel (Q2 2026)**: 3-4 blokken die samen <= 150k EUR en <= 12 weken zijn. Doel: zichtbare beweging in 3 maanden.

2. **Tactische-bundel (Q3-Q4 2026)**: 2-3 blokken die de NIS2-zorgplicht-fundament leggen. Voor budget 2026.

3. **Strategische-bundel (2027)**: 2-3 blokken voor de ISO 27001-certificering en OT-volwassenheid.

Per bundel:
- Total prijs (range)
- Risico-reductie (welke RISK-XXX gaan naar Laag/Gemiddeld)
- Certificeerings-impact (welke Annex A controls schuiven naar Volledig)

Een Gantt-stijl tijdlijn (ASCII / markdown-tabel) over 2026-2027 met alle blokken.
```

### 5. Kernwaarden-review

```
Laat kernwaarden-bewaker de menukaart reviewen:

- Worden klantdoelen (NIS2-compliance, ISO-cert, risico-reductie) zichtbaar bediend?
- Zijn prijzen onderbouwd of plucked-from-thin-air?
- Is er overpromise (bv. "garandeert compliance") of underpromise?
- Voldoen de blokken aan Quadero "menselijke maat" (concreet, behapbaar)?

Schrijf naar /workshop/state/iso27001/positioning/kernwaarden-review-menukaart.md.
```

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| ATT&CK matrix maakt dreigings-gerichte conversatie mogelijk | Klant vertrouwt structurele frameworks meer dan ad-hoc adviezen |
| Menukaart is klant-keuze-tool, geen verplicht plan | Klant kan zelf prioriteren binnen budget |
| Bundeling op tijdslot zorgt voor flow | Vermijdt "alles tegelijk" overrompeling |
| Cross-norm dekking per blok | Klant ziet dat een blok meerdere normen tegelijk afdekt |

---

## Expected output

`/workshop/state/iso27001/positioning/`:

- `attck-mapping.md` met Enterprise + ICS matrix-dekking
- `heatmap.md` defense-in-depth visualisatie
- `menukaart.md` met 6-10 vervolgblokken
- `prioritering.md` met Q2/Q3-Q4/2027-bundels en tijdlijn
- `kernwaarden-review-menukaart.md` met de review

## Success Criteria

- ATT&CK-dekking toont per tactiek een percentage
- Menukaart bevat 6-10 blokken met expliciete prijzen
- Elk blok adresseert minimaal 1 risico EN 1 gap met traceerbare referenties
- Bundeling toont logische voortgang Q2 -> Q4 -> 2027

---

**Next:** [Lab 08 - Drie Rapporten + Publish](lab-08-rapporten.md)
