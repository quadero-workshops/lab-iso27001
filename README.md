# ISO 27001 Gap-Analyse - Workshop Labs

8 hands-on labs that walk you through a full ISO 27001:2022 gap-analyse engagement using the `team-iso27001` config. All labs share one fictional client: **AquaPolder N.V.**

> **Prerequisite:** Run `workshop-pick team-iso27001` before starting. `team-iso27001` becomes the active team at `/workshop/` and its 15 agents, 14 skills, and 8 workflows become available to Claude Code.

## Scenario

AquaPolder N.V. is a mid-sized Dutch drinkwaterbedrijf (~250.000 aansluitingen, 4 zuiveringsinstallaties). Onder de Cyberbeveiligingswet (NIS2 NL-implementatie) kwalificeren zij als **essentiele entiteit** (drinkwater = Annex I-sector) en moeten zij voor 2027 aantoonbaar voldoen aan de zorgplicht. De directie wil daarnaast in 2027 een ISO 27001:2022 certificering halen om dit aantoonbaar te maken richting toezichthouders, klanten en gemeentes.

Er is geen volwassen ISMS aanwezig. In Q3 2025 hebben twee OT-engineers een phishing-mail geopend en credentials van een gedeeld Siemens-remote-support account zijn buitgemaakt. Het incident werd pas elf dagen later ontdekt door de MSSP. Geen lateral movement, maar wel een wake-up call.

Je bent ingehuurd voor een **4-weekse gap-analyse**: **intake -> normalisatie -> toetsing -> rapportage**, met drie rapporten voor drie doelgroepen (IT-management, directie, externe auditor).

De labs volgen de engagement chronologisch. Elke lab produceert een deliverable in `/workshop/state/` die de volgende lab als input gebruikt.

## Taalkeuze

AquaPolder is een Nederlandse klant en de gehele lab-set (instructies + deliverables) is in het Nederlands geschreven. De team-iso27001 skills en agents zijn ook overwegend Nederlands (`/parse-consultant-notes`, `/map-annex-a`, `compliance-auditor`, `isms-architect`, `kernwaarden-bewaker`). Een enkele agent-naam zoals `attack-mapper` is Engels gebleven omdat MITRE ATT&CK als framework Engelstalig is en vertaling verwarring zou geven. Roep skills aan met de exacte naam zoals in de lab vermeld.

## Lab Index

| # | Lab | Skill | Time |
|---|-----|-------|------|
| 01 | [Engagement Kickoff](lab-01-engagement-kickoff.md) | `/parse-consultant-notes` | 25-45 min |
| 02 | [Asset Inventarisatie](lab-02-asset-inventarisatie.md) | `/classify-assets` | 25-45 min |
| 03 | [Annex A Mapping](lab-03-annex-a-mapping.md) | `/map-annex-a` | 25-45 min |
| 04 | [Gap-Analyse + Maturity](lab-04-gap-analyse.md) | `/assess-control-gap` + `/score-maturity` | 25-45 min |
| 05 | [Multi-Norm (IEC 62443 + NIS2)](lab-05-multi-norm.md) | `/map-iec62443` + `/assess-nis2` | 25-45 min |
| 06 | [Risicobeoordeling](lab-06-risicobeoordeling.md) | `/assess-risk` | 25-45 min |
| 07 | [ATT&CK Positionering + Menukaart](lab-07-attck-menukaart.md) | `/map-mitre-attack` + `/generate-menukaart` | 25-45 min |
| 08 | [Drie Rapporten + Publish](lab-08-rapporten.md) | `/create-*-report` x 3 | 25-45 min |

> Tijds-schattingen gaan uit van de complete loop: agent-prompt, state-file schrijven, kernwaarden-bewaker review toepassen. Lab-03/04 zitten praktisch aan de bovenkant van de range door de 93 Annex A controls.

## Suggested Sequences

**Quickstart (~2 uur):** 01 -> 02 -> 03 -> 08

**Multi-norm focus (~3 uur):** 01 -> 02 -> 03 -> 05 -> 08

**Risk + roadmap focus (~4 uur):** 01 -> 02 -> 04 -> 06 -> 07 -> 08

**Full engagement (6-8 uur, met pauzes):** 01 -> 02 -> 03 -> 04 -> 05 -> 06 -> 07 -> 08

## Setup

> **Note:** `/workshop/labs/` is read-only by design - any files you create or change here will be wiped on the next container boot. All lab output lands in `/workshop/state/`, which is persistent.

```bash
workshop-pick team-iso27001    # activate team-iso27001 config
cd /workshop/
claude
```

When Claude Code is running, point it at the lab you want to start:

```
Read /workshop/labs/lab-iso27001/lab-01-engagement-kickoff.md and walk me through it.
```

## Materials

All shared scenario materials live in [`starter-env/`](starter-env/):

- [`client-brief.md`](starter-env/client-brief.md) - intake summary from kickoff
- [`consultant-notes/`](starter-env/consultant-notes/) - 5 raw interview transcripts (CIO, SOC-analist, OT-engineer, CFO, leverancier-SCADA)
- [`environment/`](starter-env/environment/) - current-state artefacten (IT-landschap, netwerk-topologie, OT-architectuur, bestaande controls)
- [`context/`](starter-env/context/) - regulatory context (NIS2 applicability) en incident history
