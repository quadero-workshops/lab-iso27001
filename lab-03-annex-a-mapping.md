# Lab 03 - Annex A Mapping

**Time:** 25-45 minutes
**Skill:** `/map-annex-a`
**Agent:** compliance-auditor
**Workflow:** gap-analyse (stap 1)
**Goal:** Bepaal welke van de 93 Annex A:2022 controls van toepassing zijn op AquaPolder en koppel bevindingen + assets uit Labs 01-02 aan specifieke controls.

---

## Prerequisites

```bash
for f in intake/parsed-notes.md intake/asset-register.md; do
  ls /workshop/state/iso27001/$f 2>/dev/null || echo "MISSING /workshop/state/iso27001/$f - doe Lab 01/02 eerst"
done
```

## Setup

Je hebt nodig:
- `/workshop/state/iso27001/intake/parsed-notes.md` (Lab 01)
- `/workshop/state/iso27001/intake/asset-register.md` (Lab 02)

```bash
cd /workshop/
claude
```

---

## Steps

### 1. Lees de Annex A structuur

```
Refereer aan de Annex A:2022 structuur uit /workshop/.claude/skills/map-annex-a/SKILL.md (of CLAUDE.md sectie "ISO 27001:2022 Annex A Domeinen"). Bevestig dat:

- A.5 - 37 controls (Organizational)
- A.6 - 8 controls (People)
- A.7 - 14 controls (Physical)
- A.8 - 34 controls (Technological)

Totaal: 93 controls.

Vat in 1 zin per domein samen wat de focus is.
```

### 2. Map Annex A met /map-annex-a

```
/map-annex-a

Map de bevindingen en assets van AquaPolder N.V. tegen ALLE 93 Annex A:2022 controls. Gebruik de compliance-auditor agent.

Bronnen:
- /workshop/state/iso27001/intake/parsed-notes.md
- /workshop/state/iso27001/intake/asset-register.md

Voor elke van de 93 controls bepaal:

1. **Toepasselijkheid** (Applicability):
   - Van toepassing (default voor de meeste controls in een NIS2-essentiele entiteit)
   - Niet van toepassing (alleen met expliciete justificatie, bv. A.5.34 - privacy en PII - is van toepassing; A.7.13 - equipment maintenance - is van toepassing; etc.)

2. **Bewijs (Evidence)**:
   - Welke [NOTA-XXX] bevindingen en [ASSET-XXX] assets zijn relevant voor deze control?
   - Citaat uit de bron als illustratie

3. **Voorlopige status-indicator** (preliminary, zal in Lab 04 verfijnd):
   - Aanwezig (control lijkt geadresseerd)
   - Gedeeltelijk aanwezig (controle bestaat maar niet volledig)
   - Niet aanwezig (gap)
   - Niet beoordeelbaar (onvoldoende evidence)

4. **Domein-eigenaar**:
   - Welk lid van het AquaPolder-team is logische eigenaar (CIO, OT-manager, CFO, etc.)?

Sla op naar /workshop/state/iso27001/gap-analyse/annex-a-mapping.md. Maak dit een gestructureerd document met sectie per domein (A.5, A.6, A.7, A.8) en een tabel per domein.

CRITISCH: GEEN control mag missen. Een control die "geen evidence in onze bronnen" heeft, krijgt status "Niet beoordeelbaar".
```

Dit is de zwaarste stap. Verwacht een rapport van 5-10 pagina's.

### 3. Identificeer Statement of Applicability

```
Genereer /workshop/state/iso27001/gap-analyse/soa-concept.md met een Statement of Applicability (SoA) conform ISO 27001:2022 vereisten:

- Een tabel met alle 93 controls
- Kolom 1: Control-nummer (A.5.1 etc.)
- Kolom 2: Control-naam
- Kolom 3: Van toepassing (Ja / Nee)
- Kolom 4: Justificatie (waarom wel of niet)
- Kolom 5: Implementatie-bron-referentie (bv. "Beleidsdocument X.Y" of "Niet geimplementeerd")
- Kolom 6: Status (Aanwezig / Deels / Niet / Niet beoordeelbaar)

Voor "Niet van toepassing"-controls verwacht ik dat dit zeldzaam is. Bij elk Nee een expliciete reden.
```

### 4. Quick-scan: domein-niveau samenvatting

```
Maak /workshop/state/iso27001/gap-analyse/domain-summary.md met per Annex A domein:

- Aantal controls totaal
- Aantal "Aanwezig"
- Aantal "Gedeeltelijk"
- Aantal "Niet"
- Aantal "Niet beoordeelbaar"
- Procentuele indicatie compliance (Aanwezig + 50% * Gedeeltelijk / Totaal)

En een korte conclusie per domein (1-2 zinnen).
```

### 5. Sanity-check met isms-architect

```
Laat de isms-architect agent de mapping reviewen. Specifiek:

- Zijn er controls die als "Niet van toepassing" zijn gemarkeerd waar dit twijfelachtig is voor een drinkwaterbedrijf onder NIS2?
- Zijn er evidence-mappings die te ruim zijn (een bevinding wordt aan te veel controls gekoppeld)?
- Wordt het verschil tussen "Niet aanwezig" en "Niet beoordeelbaar" consistent toegepast?

Schrijf de review naar /workshop/state/iso27001/gap-analyse/isms-review-mapping.md.
```

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| Alle 93 controls worden geadresseerd, ook als "Niet beoordeelbaar" | Volledigheid is een ISO 27001 audit-vereiste |
| Conservatieve beoordeling: bij twijfel "Niet" | Een vals negatief is veiliger dan een vals positief - de auditor doet hetzelfde |
| SoA-tabel mapt direct op control-nummer | Auditor verwacht dit exact format |
| Domain-summary maakt rapportage in Lab 08 makkelijker | Voorbereidt executive summary cijfers |

---

## Expected output

`/workshop/state/iso27001/gap-analyse/`:

- `annex-a-mapping.md` met alle 93 controls gemapt, evidence en voorlopige status
- `soa-concept.md` met de Statement of Applicability-tabel
- `domain-summary.md` met per-domein compliance-indicatie
- `isms-review-mapping.md` met de review

Verwacht orde van grootte:
- A.5 (Organizational, 37): grootste gat. Veel "Niet" / "Deels" rond ISMS-clauses, awareness, supplier management, classificatie
- A.6 (People, 8): mid. Vooral "Deels" op training en awareness
- A.7 (Physical, 14): redelijk. Veel "Aanwezig", maar zuiveringsinstallaties verschillen
- A.8 (Technological, 34): gemengd. IT-side relatief goed (Defender, Intune, MFA), OT-side veel gaten

## Success Criteria

- Alle 93 controls beoordeeld in `annex-a-mapping.md`
- SoA-tabel compleet
- Elke "Gedeeltelijk" / "Niet" heeft minstens 1 evidence-referentie naar parsed-notes of asset-register
- isms-architect review aanwezig

---

**Next:** [Lab 04 - Gap-Analyse + Maturity](lab-04-gap-analyse.md)
