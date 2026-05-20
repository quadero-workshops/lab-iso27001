# Lab 01 - Engagement Kickoff

**Time:** 25-45 minutes
**Skill:** `/parse-consultant-notes`
**Agent:** asset-analyst
**Workflow:** intake-en-normalisatie (stap 1)
**Goal:** Verwerk de ruwe consultant-notities tot gestructureerde [NOTA-XXX] bevindingen die als bron-evidence voor alle volgende labs dienen.

---

## Setup

> **Note:** `/workshop/labs/` is read-only. Alle output van deze lab landt in `/workshop/state/iso27001/intake/` en is persistent.

Je hebt `workshop-pick team-iso27001` actief. Verifieer met:

```bash
ls /workshop/.claude/agents/  # zou asset-analyst, compliance-auditor, etc. moeten tonen
```

Als die directory leeg is, run dan eerst `workshop-pick team-iso27001`.

Start Claude Code vanuit `/workshop/`:

```bash
cd /workshop/
claude
```

---

## Steps

### 1. Lees de engagement-brief

```
Read /workshop/labs/lab-iso27001/starter-env/client-brief.md en geef in 5 bullets samen: wie is de klant, wat is de aanleiding, wat is in scope, wie zijn de stakeholders, wat is "goed" bij oplevering.
```

Verifieer dat de samenvatting overeenkomt met je eigen lezing van de brief.

### 2. Parse de consultant-notities met /parse-consultant-notes

Activeer de skill expliciet en parse alle 5 interview-transcripts:

```
/parse-consultant-notes

Verwerk de volgende consultant-notities van AquaPolder N.V. tot gestructureerde bevindingen met unieke [NOTA-XXX] IDs. Gebruik de asset-analyst agent. Voor elke interview-transcript:

- /workshop/labs/lab-iso27001/starter-env/consultant-notes/interview-01-cio.md
- /workshop/labs/lab-iso27001/starter-env/consultant-notes/interview-02-soc-analist.md
- /workshop/labs/lab-iso27001/starter-env/consultant-notes/interview-03-ot-engineer.md
- /workshop/labs/lab-iso27001/starter-env/consultant-notes/interview-04-cfo.md
- /workshop/labs/lab-iso27001/starter-env/consultant-notes/interview-05-leverancier-scada.md

Voor elke bevinding genereer:
- Uniek ID [NOTA-001], [NOTA-002], etc.
- Categorie (technisch / organisatorisch / juridisch / leverancier / OT / IT / proces / mens)
- Beschrijving (1-3 zinnen, citeer waar relevant)
- Bron-interview en relevante quote
- Status-indicator (Bestaand / Ontbrekend / Onvolledig / Risico)

Markeer onduidelijke passages met [ONDUIDELIJK] en interpreteer ze niet.

Sla het resultaat op naar /workshop/state/iso27001/intake/parsed-notes.md.
```

Verwacht: 50-80 bevindingen verspreid over de 5 interviews.

### 3. Identificeer kritieke patronen

```
Op basis van /workshop/state/iso27001/intake/parsed-notes.md, identificeer:

1. Patronen waar 2 of meer bevindingen samen wijzen op een onderliggende structurele zwakheid (bv. "geen formeel ISMS" kan in meerdere interviews voorkomen).
2. Tegenstrijdige bevindingen tussen interviews (zelfde onderwerp, verschillende perspectieven).
3. Bevindingen die mogelijk een directe Annex A control raken (vooruitkijkend naar Lab 03).

Schrijf naar /workshop/state/iso27001/intake/patterns.md.
```

### 4. Sanity-check met kernwaarden-bewaker

```
Laat kernwaarden-bewaker /workshop/state/iso27001/intake/parsed-notes.md beoordelen. Specifiek let op:

- Zijn bevindingen objectief geformuleerd en niet gekleurd?
- Worden quotes uit de interviews letterlijk overgenomen of geinterpreteerd?
- Zijn er bevindingen die "overpromisen" of "underpromise" wat de klant heeft gezegd?

Schrijf de review naar /workshop/state/iso27001/intake/kernwaarden-review-parsed-notes.md.
```

Pas de review terug op de parsed-notes als nodig.

---

## What to Notice

| Behaviour | Why it matters |
|-----------|----------------|
| `/parse-consultant-notes` invokes asset-analyst automatisch | De skill kent het juiste agent zonder dat jij die hoeft te noemen |
| Elke bevinding behoudt traceerbaarheid naar bron | Onder ISO 27001 audit moet je elke bewering kunnen onderbouwen |
| Onduidelijke passages worden gemarkeerd, niet ingevuld | Conservatieve beoordeling - liever een gat tonen dan invullen met aannames |
| kernwaarden-bewaker controleert objectiviteit | Quadero norm: rapportage is feitelijk, niet gekleurd |

---

## Expected output

`/workshop/state/iso27001/intake/`:

- `parsed-notes.md` met 50-80 [NOTA-XXX] bevindingen, per interview gegroepeerd
- `patterns.md` met 8-15 herkende patronen en/of tegenstrijdigheden
- `kernwaarden-review-parsed-notes.md` met de review en eventuele correcties

Verwacht hoge thema's die je terug zult zien:
- Geen formeel ISMS / governance (CIO, CFO, OT-manager)
- OT-monitoring gap (CIO, SOC, OT, post-incident)
- Leveranciers-aanpak onder NIS2 (CIO, leveranciers-manager)
- Gedeeld Siemens-support account (CIO, OT, leverancier)
- Awareness-training afwezig (CIO, SOC)
- SAP_ALL voor CFO + IT-manager (CFO interview, A.5.18 implicatie)

## Success Criteria

- `parsed-notes.md` bestaat en bevat minimaal 50 [NOTA-XXX] bevindingen
- Elke bevinding heeft een bron-referentie naar interview-XX
- Onduidelijke passages zijn met [ONDUIDELIJK] gemarkeerd
- kernwaarden-bewaker-review is uitgevoerd en eventuele opmerkingen verwerkt

---

**Next:** [Lab 02 - Asset Inventarisatie](lab-02-asset-inventarisatie.md)
