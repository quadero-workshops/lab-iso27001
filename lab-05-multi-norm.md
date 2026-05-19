# Lab 05 - Multi-Norm (IEC 62443 + NIS2)

**Time:** 12-15 minutes
**Skill:** `/map-iec62443` + `/assess-nis2`
**Agent:** iec62443-specialist + nis2-analyst
**Workflow:** multi-norm-assessment
**Goal:** Voer parallel een IEC 62443-beoordeling voor de OT-omgeving uit en een NIS2-zorgplicht-toets. Identificeer overlap met de ISO 27001-gap-analyse om duplicatie te voorkomen.

---

## Setup

Je hebt nodig:
- `/workshop/state/iso27001/gap-analyse/gap-detail.md` (Lab 04)
- `/workshop/labs/lab-iso27001/starter-env/environment/ot-architecture.md`
- `/workshop/labs/lab-iso27001/starter-env/context/nis2-applicability.md`

```bash
cd /workshop/
claude
```

---

## Steps

### 1. IEC 62443: OT-zones en conduits

```
/map-iec62443

Voer een IEC 62443-beoordeling uit voor de OT-omgeving van AquaPolder. Gebruik de iec62443-specialist agent.

Bron-input:
- /workshop/labs/lab-iso27001/starter-env/environment/ot-architecture.md
- /workshop/state/iso27001/intake/parsed-notes.md (filter op OT-relevante NOTA's)
- /workshop/state/iso27001/intake/asset-register.md (filter op OT-assets)

Lever:

1. **Zones en conduits diagram** (in markdown / ASCII): identificeer de zones in AquaPolder's OT-omgeving conform Purdue-model. Voor elke zone:
   - Naam (bv. "Zone 0 - Process L0", "Zone 3 - Site Operations")
   - Bijbehorende assets [ASSET-XX-NNN]
   - Trust level (laag/midden/hoog)
   - Bestaande controls

   Voor elke conduit (verbinding tussen zones):
   - Welke zones verbindt het
   - Welke controles (firewalls, data diodes, ACL's)
   - Risico-niveau (laag/midden/hoog)

2. **Foundational Requirements (FR) beoordeling** voor de 7 IEC 62443 FRs:
   - FR1: Identification and Authentication Control
   - FR2: Use Control
   - FR3: System Integrity
   - FR4: Data Confidentiality
   - FR5: Restricted Data Flow
   - FR6: Timely Response to Events
   - FR7: Resource Availability

   Per FR: status (Volledig / Deels / Niet) + evidence + gap-beschrijving + indicatief gewenste Security Level (SL1-SL4) op basis van AquaPolder's risico-profiel.

3. **Cross-mapping naar ISO 27001 Annex A**: voor elke IEC 62443-gap noem de overlappende A.x.y controls. Voorkom dat dezelfde gap dubbel gerapporteerd wordt in de eindrapportage.

Sla op naar /workshop/state/iso27001/multi-norm/iec62443-assessment.md.
```

### 2. NIS2: Zorgplicht (Artikel 21) toetsing

```
/assess-nis2

Voer de NIS2-zorgplicht-toets uit voor AquaPolder als essentiele entiteit. Gebruik de nis2-analyst agent.

Bron-input:
- /workshop/labs/lab-iso27001/starter-env/context/nis2-applicability.md
- /workshop/state/iso27001/intake/parsed-notes.md
- /workshop/state/iso27001/gap-analyse/gap-detail.md

Lever:

1. **Kwalificatie-validatie**: bevestig de zelf-uitgevoerde NIS2-applicability-assessment. Bevestig of corrigeer:
   - Essentiele entiteit-status (drinkwater = Annex I sector 1)
   - Omvang-drempel (180 medewerkers, >10M omzet)
   - Aanvullende dochterondernemingen / activiteiten?

2. **Toets per zorgplicht-component** (Artikel 21 lid 2 a t/m i):
   - Status (Compliant / Deels / Non-compliant)
   - Evidence in AquaPolder bronnen
   - Verwijzing naar overlappende Annex A controls

3. **Meldplicht-readiness** (Artikel 23):
   - Kan AquaPolder een early warning binnen 24u bij CSIRT-Drinkwater plaatsen?
   - Wie beslist? Wie communiceert? Is er een geoefend pad?
   - Gap-beschrijving

4. **Bestuurdersaansprakelijkheid-readiness**:
   - Is het bestuur aantoonbaar geinformeerd?
   - Is er periodieke risk-update (kwartaal)?
   - Bestaat er een formele cyber-governance-besluitvorming?

5. **Top-5 NIS2-specifieke gaps** die NIET via ISO 27001 worden afgedekt (bv. specifieke sectorale meldplicht-procedure).

Sla op naar /workshop/state/iso27001/multi-norm/nis2-assessment.md.
```

### 3. Consolideer multi-norm gaps

```
Genereer /workshop/state/iso27001/multi-norm/consolidated-gaps.md met:

Een tabel die voor elke gap toont:
- Gap-ID
- Beschrijving
- ISO 27001 controls (mogelijk meerdere)
- IEC 62443 FRs (mogelijk meerdere)
- NIS2 artikelen (mogelijk meerdere)
- Prioriteit (gebaseerd op combinatie van normen die het raakt)

Doel: voorkom dat de klant 3x voor "hetzelfde" wordt aangerekend. Een gap die alle 3 normen raakt is hoogste prioriteit; een gap die alleen IEC 62443 raakt heeft alleen relevantie voor de OT-zijde.
```

### 4. Sanity-check met isms-architect

```
Laat isms-architect de multi-norm-output reviewen:

- Zijn cross-mappings tussen normen consistent en niet te ruim?
- Is de prioriteit-logica logisch onderbouwd?
- Zijn er gaps die in de basis-gap-analyse zaten maar in de multi-norm zijn weggevallen (of v.v.)?

Schrijf naar /workshop/state/iso27001/multi-norm/isms-review-multinorm.md.
```

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| OT-zones expliciet getekend | IEC 62443 vereist een document van zones & conduits |
| Cross-mapping voorkomt duplicatie | Klant verwacht dat consultant ziet welke maatregelen aan meerdere normen voldoen |
| NIS2 heeft eigen artikelen, niet alleen vertaalbaar naar Annex A | Bv. registratie-plicht en meldplicht hebben geen 1-op-1 Annex A-control |
| Multi-norm gap-consolidatie wordt het hart van de menukaart | Een "blok" in de menukaart adresseert meerdere norm-eisen |

---

## Expected output

`/workshop/state/iso27001/multi-norm/`:

- `iec62443-assessment.md` met zones, conduits, FRs en SL-niveaus
- `nis2-assessment.md` met zorgplicht + meldplicht + governance
- `consolidated-gaps.md` met cross-norm-gap-tabel
- `isms-review-multinorm.md` met de review

Verwacht resultaat:
- IEC 62443: SL2 als ambitie-niveau voor AquaPolder OT (proportioneel voor drinkwater). Huidige stand: SL1 voor veel FRs, met FR1/FR2 het slechtst (gedeeld account, no auth, etc.)
- NIS2: zorgplicht-componenten op (a) risico-analyse, (d) ketenrisico, (f) cyber-hygiene en (h) asset/access-management zijn de grootste gaps. Meldplicht is procedural haalbaar maar niet geoefend.

## Success Criteria

- `iec62443-assessment.md` beoordeelt alle 7 FRs
- `nis2-assessment.md` adresseert alle 9 zorgplicht-componenten (Artikel 21 lid 2 a-i) plus meldplicht en governance
- Cross-mappings zijn gevalideerd door isms-architect
- Consolidated-gaps tonen overlap zichtbaar

---

**Next:** [Lab 06 - Risicobeoordeling](lab-06-risicobeoordeling.md)
