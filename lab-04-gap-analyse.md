# Lab 04 - Gap-Analyse + Maturity Scoring

**Time:** 12-15 minutes
**Skill:** `/assess-control-gap` + `/score-maturity`
**Agent:** compliance-auditor + maturity-scorer
**Workflow:** gap-analyse (stappen 2-3)
**Goal:** Verdiep de Annex A mapping van Lab 03 tot een formele gap-beoordeling per control en bepaal de CMM-maturity-scores per domein.

---

## Setup

Je hebt nodig:
- `/workshop/state/iso27001/gap-analyse/annex-a-mapping.md` (Lab 03)
- `/workshop/state/iso27001/intake/parsed-notes.md` en `asset-register.md`

```bash
cd /workshop/
claude
```

---

## Steps

### 1. Verdiep de gap-beoordeling met /assess-control-gap

```
/assess-control-gap

Voer per Annex A control een formele gap-beoordeling uit op basis van /workshop/state/iso27001/gap-analyse/annex-a-mapping.md. Gebruik de compliance-auditor agent.

Voor elke control geef:

1. **Definitieve status** (precisering van Lab 03 voorlopige status):
   - Volledig (control is geimplementeerd en operationeel)
   - Deels (control is geimplementeerd voor een deel - geef geschat implementatie-percentage 10-90%)
   - Niet (control niet geimplementeerd)
   - Niet Beoordeelbaar (onvoldoende evidence in bronnen)

2. **Gap-beschrijving** bij Deels of Niet:
   - Wat ontbreekt concreet
   - Verwijzing naar [NOTA-XXX] / [ASSET-XXX] die de gap aantoont
   - Mogelijke compenserende maatregelen die nu wel actief zijn

3. **Indicatieve remediation-richting** (1-2 zinnen, geen plan):
   - Wat zou implementatie inhouden? Geen tijdlijn of kosten yet.

4. **Evidence-quote**: 1 letterlijk citaat uit de bron-notitie ter staving (waar mogelijk)

Belangrijke aandachtspunten:

- Conservatief beoordelen: bij twijfel "Deels" in plaats van "Volledig"
- "Compenserende maatregelen" mogen aanwezig zijn maar vervangen niet de originele control
- Indien een control betrekking heeft op OT EN IT: beoordeel beide separaat en geef een gecombineerde status (lager van de twee)

Sla op naar /workshop/state/iso27001/gap-analyse/gap-detail.md. Structureer per Annex A domein, met een sectie per control.
```

### 2. Bereken maturity scores met /score-maturity

```
/score-maturity

Bereken CMM-maturity-scores per Annex A domein op basis van /workshop/state/iso27001/gap-analyse/gap-detail.md. Gebruik de maturity-scorer agent.

CMM-schaal:
- 0 - Non-existent
- 1 - Initial (ad-hoc)
- 2 - Repeatable (basisprocessen, niet gestandaardiseerd)
- 3 - Defined (gestandaardiseerd, gedocumenteerd, gecommuniceerd)
- 4 - Managed (gemeten, gemonitord, voorspelbaar)
- 5 - Optimizing (continue verbetering, geautomatiseerd)

Voor elke van de 4 Annex A domeinen (A.5, A.6, A.7, A.8):

1. Bereken een gewogen gemiddelde score per domein (controls met meer evidence wegen meer)
2. Geef een 2-zins onderbouwing per score
3. Vergelijk met indicatieve branche-benchmarks:
   - Drinkwater-sector NL hypothetische baseline (illustratief, niet uit publieke bron - markeer in rapport als aanname): A.5=1.8, A.6=2.0, A.7=2.5, A.8=2.2
   - ISO 27001-gecertificeerde organisaties: minimaal CMM 3 per domein (algemeen geaccepteerde drempel)

4. Identificeer per domein de 3 controls met laagste maturity (CMM 0-1)
5. Bereken een gewogen totaalscore over alle 93 controls

Sla op naar /workshop/state/iso27001/gap-analyse/maturity-scores.md.
```

### 3. Top-20 kritieke gaps

```
Genereer /workshop/state/iso27001/gap-analyse/top-20-gaps.md met:

De 20 belangrijkste gaps geprioriteerd op:
- Impact op certificering (kan certificering blokkeren?)
- Impact op NIS2-zorgplicht
- Implementatie-complexiteit (laag/midden/hoog, indicatief)
- Combinatie met andere gaps (clusters)

Per gap:
- ID (GAP-001 etc.)
- Control(s) waarop het betrekking heeft
- Beschrijving (1-2 zinnen)
- Bron-evidence
- Categorie (quick win / structureel project / strategisch)

Voor "Quick win" hanteer richtlijn: <= 4 weken implementatie, <= 25k EUR.
```

### 4. Kwaliteits-check met isms-architect

```
Laat isms-architect /workshop/state/iso27001/gap-analyse/gap-detail.md en /workshop/state/iso27001/gap-analyse/maturity-scores.md reviewen op:

1. Consistentie: zijn maturity-scores consistent met gap-statussen? (bv. een domein met veel "Niet" zou niet plotseling CMM 4 moeten zijn)
2. Cross-references: worden gaps correct gekoppeld aan evidence?
3. Onderbouwing: heeft elke "Niet" of "Deels" een traceerbare basis?

Schrijf de review naar /workshop/state/iso27001/gap-analyse/isms-quality-review.md.
```

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| Compenserende maatregelen mogen genoteerd maar tellen niet als compliant | ISO 27001 audit-conform - de originele control blijft "Deels" |
| OT en IT scoring kan verschillen binnen dezelfde control | A.8.5 (Secure Auth) is voor M365 = Volledig, voor OT-domain = Deels |
| Branche-benchmark plaatst AquaPolder relatief | Helpt management om "waar staan we" te beantwoorden - markeer in rapport expliciet als illustratieve aanname (geen publieke bron) of vervang door eigen benchmark |
| Top-20 wordt input voor Lab 06 (risico) en Lab 07 (roadmap) | Quickwins-onderscheiding is essentieel voor menukaart |

---

## Expected output

`/workshop/state/iso27001/gap-analyse/`:

- `gap-detail.md` met per-control gap-beoordeling
- `maturity-scores.md` met CMM scores per domein + totaal
- `top-20-gaps.md` met geprioriteerde gap-lijst
- `isms-quality-review.md` met de kwaliteits-review

Verwachte CMM-scores (orde-van-grootte, exact afhankelijk van model):
- A.5 (Organizational): 1.0 - 1.5
- A.6 (People): 1.5 - 2.0
- A.7 (Physical): 2.0 - 2.5
- A.8 (Technological): 1.8 - 2.5
- Gewogen totaal: 1.5 - 2.0

Conclusie: substantieel gat tot de certificeerings-baseline (CMM 3.0+).

## Success Criteria

- `gap-detail.md` bevat formele beoordeling voor alle 93 controls (geen lege rijen)
- `maturity-scores.md` heeft scores per domein met onderbouwing
- `top-20-gaps.md` lijst quickwins separaat van structurele projecten
- Geen control heeft een score "Volledig" zonder evidence-referentie

---

**Next:** [Lab 05 - Multi-Norm (IEC 62443 + NIS2)](lab-05-multi-norm.md)
