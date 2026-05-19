# AquaPolder N.V. - Incident Log 2025

**Versie**: 2026-Q1
**Bron**: Compilatie uit MSSP-rapportages + interne notities
**Markering**: VERTROUWELIJK - alleen voor gap-analyse-doelen

> Dit document beschrijft beveiligings-relevante incidenten en bijna-incidenten van AquaPolder N.V. in kalenderjaar 2025. Dient als input voor:
> - Risicobeoordeling (dreigings-realisme, geen hypothetische scenario's)
> - Identificatie van patroon-zwakheden
> - Validatie van detectie en respons in de praktijk

## Incident #1: Q3 2025 - Phishing + Credential Theft (OT-Support-account)

**Datum aanvang**: 2025-08-15 (phishing-mail ontvangen)
**Datum eerste login attacker**: 2025-08-15 22:47 CEST
**Datum ontdekking**: 2025-08-26 (CyberWatch threat-hunting)
**Tijd-tot-detectie**: 11 dagen
**Status**: Gesloten (geen lateral movement, account-credentials geroteerd, MFA toegevoegd)
**Classificatie**: High (potentieel critical bij escalatie)

### Tijdlijn
- **2025-08-15 14:32**: Phishing-mail bezorgd bij twee OT-engineers (`r.koster@aquapolder.nl`, `m.hendriksen@aquapolder.nl`). Onderwerp: "Siemens Customer Portal - Action required: password expiration". Afzender domain look-alike: `siemenscustomersupport-portal.com` (legitiem domein is `siemens.com`).
- **2025-08-15 14:48**: Een engineer (m.hendriksen) klikt en voert credentials in op nep-portaal. De credentials zijn voor het gedeelde Siemens-support-account, niet voor zijn persoonlijke AquaPolder-account.
- **2025-08-15 22:47**: Eerste login op de Fortinet SSL-VPN onder het Siemens-support-account, vanuit IP `185.34.122.x` (Bulgaars colocation provider). UEBA-alert "Login from unusual geo" wordt gegenereerd.
- **2025-08-16 09:14**: Tier 1 SOC-analist bij CyberWatch behandelt de alert. Misinterpreteert als "nieuwe leverancier-werknemer" (op basis van weekend-timing) en sluit ticket als false positive.
- **2025-08-15 t/m 2025-08-26**: 6 succesvolle VPN-logins onder het Siemens-account vanuit verschillende IP-adressen in Oost-Europa en Zuid-Azie. Reconnaissance op de jumphost (vermoedelijk). Geen wijzigingen aan SCADA-configuratie gedetecteerd (maar dat is ook niet gemonitord op PLC-niveau).
- **2025-08-26 11:02**: CyberWatch threat-hunting query identificeert het patroon. Eskalatie naar Karin Visser en Lieke van Bemmel.
- **2025-08-26 11:30**: Account password gewijzigd. VPN-sessies gekilled. Forensic analyse opgestart.
- **2025-08-27 t/m 2025-08-29**: Forensic-analyse door Siemens-forensics-partner en CyberWatch. Geen lateral movement naar PLC's geconstateerd. Geen wijzigingen aan WinCC OA-configuraties. Wachtwoord verandering van het account naar nieuwe sterke string.
- **2025-09-15**: MFA toegevoegd aan Fortinet SSL-VPN voor het Siemens-account. Awareness-sessie voor OT en IT (eenmalig).
- **2025-09-30**: Post-incident review intern. Geen NCSC-melding gedaan (NIS2 was nog niet in werking voor essentiele entiteiten).

### Root cause
1. Gebrek aan security awareness training (engineer herkent een look-alike domein niet)
2. Geen MFA op het Siemens-support-account (was wachtwoord-only)
3. Gedeeld account zonder per-individu-tracking
4. UEBA-alert verkeerd geclassificeerd door Tier 1-analyst

### Lessons learned (gedocumenteerd)
- MFA op alle VPN-toegang, ook voor service-accounts
- Awareness moet structureel, niet alleen post-incident
- UEBA-rules voor service-accounts moeten op high priority staan (gedaan, structureel doorgevoerd door CyberWatch)
- Sessie-recording op jumphost-toegang aanbevolen (open actie, nog niet uitgevoerd)
- Per-individu-tracking via wekelijkse Siemens-lijst (gedeeltelijk uitgevoerd sinds Q1 2026)

### Wat NIET is opgelost
- Het gedeelde account bestaat nog (contractuele beperking)
- Geen passieve OT-monitoring (zou hebben geholpen om lateral movement te detecteren)
- Geen formele besluitvormings-structuur aan AquaPolder-kant voor major incidents

## Incident #2: 2025-05-12 - DDoS-attempt op klantportaal

**Datum**: 2025-05-12 13:08
**Status**: Gesloten (gemitigeerd door Azure DDoS Protection Standard)
**Classificatie**: Medium

### Verloop
Volumetric DDoS aanval (~12 Gbps SYN-flood) op de klantportaal Front Door endpoint. Azure DDoS Protection Standard detecteerde en mitigeerde binnen 90 seconden. Klantportaal had ~2 minuten verminderde response time, geen volledige outage.

Vermoedelijke aanvaller: niet geidentificeerd. Mogelijk script-kiddie campagne (was rond dezelfde tijd dat andere nutsbedrijven werden geraakt). Geen claim of ransom-bericht ontvangen.

### Lessons
- DDoS Protection Standard werkt zoals bedoeld
- Klantportaal-monitoring-alerts moeten beter (10-min vertraging tussen DDoS-start en eerste interne alert)

## Incident #3: 2025-02-20 - MFA-fatigue tegen finance-medewerker

**Datum**: 2025-02-20 09:45 - 09:51
**Status**: Gesloten (geen compromise)
**Classificatie**: Medium

### Verloop
Een finance-medewerker ontving 14 MFA push-prompts in 6 minuten via Microsoft Authenticator. Ze drukte op "Approve" bij de 9e prompt. Conditional Access blokkeerde de daaropvolgende login omdat het van een onbekend device + onbekend land kwam.

UEBA-alert (>10 MFA-prompts) genereerde een Tier 1-ticket, opgepakt binnen 12 minuten. Account-password reset uitgevoerd. Awareness-mail naar alle finance-medewerkers.

### Lessons
- Number Matching MFA uitgerold sinds Q2 2025 (in plaats van push-alleen)
- Conditional Access werkt zoals bedoeld

## Incident #4: 2025-11-04 - Phishing-poging via Teams ext-message

**Datum**: 2025-11-04 (multi-day campagne)
**Status**: Gesloten
**Classificatie**: Low

### Verloop
Drie medewerkers ontvingen via Teams een chat-bericht van een onbekende externe gebruiker met een link naar een nep-DocuSign-pagina. Twee meldden via Report-button bij Defender for O365. Een derde klikte op de link maar voerde geen credentials in (was alert door eerdere phishing-trainingen).

Sinds dit incident is Teams external messaging beperkt tot federated-only.

## Bijna-incidenten / observaties (niet als incident geclassificeerd)

- **2025-07**: Een leverancier (bouwbedrijf, geen IT-leverancier) stuurde een aangepaste IBAN voor factuur-betaling via een normaal-uitziende email. CFO-controle ontdekte dat de IBAN niet bij dat bedrijf hoorde. Bevestigd telefonisch en niet betaald. Mogelijk BEC-poging.
- **2025-09**: Een ex-medewerker (uit dienst sinds Juli 2025) probeerde 4 weken na vertrek nog in te loggen op M365. Account was correct gedisabled, maar de logs lieten zien dat hij wist welke applicaties hij gebruikte. Onderzoek toonde geen kwaadwillende intentie aan, eerder een poging om persoonlijke data op te halen.
- **2025-12**: Een Siemens-engineer logde tijdens normale support-werkzaamheden 's nachts in op de jumphost. Geen alert ging af (jumphost-login alleen logd, niet ge-alert). Dit is genoteerd als detectie-gap.

## Patronen

1. **OT-zichtbaarheid is laag**: incident #1 had niet 11 dagen onontdekt mogen blijven. Verbetering van OT-monitoring is essentieel.
2. **Awareness werkt**: na phishing-incident in incident #4 herkenden de meeste medewerkers een soortgelijke poging. Maar de schaal van training is te beperkt.
3. **MFA werkt**: in incident #3 voorkwam Conditional Access een compromise ondanks dat de gebruiker een Approve had gegeven.
4. **Tier 1 SOC-analyse kan misgaan**: incident #1 had via Tier 1 misclassificatie 11 dagen vertraging. Threat-hunting-cyclus heeft gered.
5. **Single-account-leveranciers**: het gedeelde Siemens-account is een terugkerend risico-thema. Niet alleen Siemens - ook DeltaConsult (SAP) heeft per-engineer-toegang via een gedeeld pool-account.

## Implicaties voor risk-assessment

- Dreigingsactor categorie: opportunistic (DDoS, phishing-massa-campagnes) tot motivated (Q3 2025 incident lijkt meer gericht, mogelijk pre-positioning voor latere aanval)
- Tijd-tot-detectie van 11 dagen voor een geslaagde compromise is realistisch en moet meegenomen in dreigings-scenario's
- BEC-pogingen zijn een onderschat risico voor finance-processen
