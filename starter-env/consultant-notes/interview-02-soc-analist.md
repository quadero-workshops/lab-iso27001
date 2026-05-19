# Interview 02 - Karin Visser, SOC-coordinator (extern via MSSP CyberWatch B.V.)

**Datum**: 2026-04-16
**Locatie**: Microsoft Teams (Karin werkt vanuit Utrecht)
**Duur**: 60 minuten
**Aanwezig**: Karin Visser (SOC-coordinator), Erik Grasman (Quadero, interviewer)

---

**EG**: Karin, hoe ben je officieel verbonden met AquaPolder?

**KV**: Ik ben in dienst bij CyberWatch, een MSSP in Utrecht. AquaPolder is een van mijn drie accounts. Ik draai geen shifts mee in het SOC zelf - daar zit een team van 12 - maar ik ben het aanspreekpunt voor AquaPolder. Tier 2/3 escalaties komen bij mij, en ik doe maandelijks een review met Lieke.

**EG**: Vertel eens over de SOC-dekking. Wat zien jullie?

**KV**: We hebben een Sentinel-workspace, gevoed door:
- M365 audit logs (volledig, sinds 2024)
- Azure activity logs (volledig)
- Defender for Endpoint (alle 250 endpoints van AquaPolder)
- Defender for Cloud Apps (M365 en een handvol SaaS-apps)
- AzureSQL audit logs van het klantportaal
- Edge firewall logs (Fortinet)
- VPN-logs van de OT-VPN (sinds Q4 2025, na het incident)

Wat we NIET zien:
- SAP S/4HANA security-events
- OT-netwerk verkeer
- PLC-toegangs-logs
- Engineering workstation telemetrie (die zijn niet Intune-managed)
- Historian (PI System) toegangs-logs

**EG**: Hoeveel alerts per dag?

**KV**: Order of magnitude 30-50 echte tickets per dag, na deduplication en filtering. Van die 30-50 komt 80% uit de M365-kant. Phishing-meldingen via de Outlook Report-button zijn meestal het meest, dan failed login bursts, dan af en toe een MFA-fatigue-poging.

**EG**: Hoe gaan jullie om met OT?

**KV**: Eerlijk antwoord: we hebben nul echte zichtbaarheid op OT-laag. We zien de VPN-logins voor remote support nu wel, maar wat er daarna binnen het OT-netwerk gebeurt is een black box. Dat is een gap die ik in elke maandelijkse review benoem.

**EG**: Het Q3 2025 incident - wat gebeurde er vanuit jullie perspectief?

**KV**: We hebben een UEBA-rule die alarm slaat als een gebruiker buiten kantoortijden inlogt vanuit een ongebruikelijke geo-locatie. Die ging af op een vrijdagavond om 22:47, login vanuit een Bulgaars IP-adres op een service-account dat normaal alleen overdag vanuit Nederland inlogt. De alert ging naar Tier 1 op zaterdagochtend, die heeft 'm geclassificeerd als low-priority omdat hij dacht dat het een nieuwe leverancier-werknemer was.

Pas op woensdag, toen iemand bij CyberWatch het dossier oppakte voor de wekelijkse threat-hunting, viel het kwartje. Onderzoek toonde aan dat het Siemens-remote-support-account was, dat de credentials waren geharvest via phishing 11 dagen eerder, en dat er in de tussentijd 6 succesvolle VPN-logins waren geweest. Geen activiteit op de PLC's gedetecteerd, maar dat is ook omdat we geen PLC-logging hebben. Forensic check door Siemens toonde geen wijzigingen aan SCADA-configuratie.

**EG**: Tijd-tot-detectie was 11 dagen. Wat is jullie SLA?

**KV**: Voor critical alerts 15 minuten respons, voor high 1 uur, voor medium 4 uur, voor low best effort binnen 24 uur. Deze alert was als medium geclassificeerd en is binnen SLA opgepakt, maar de Tier 1-analyst heeft 'm op basis van een verkeerde aanname als false positive afgevinkt zonder verder onderzoek. Dat is intern bij ons besproken, en we hebben de UEBA-rule voor service-accounts naar high gepromoveerd.

**EG**: Welke specifieke detecties hebben jullie nu lopen?

**KV**: Standaard Microsoft analytics, plus een set custom KQL-queries voor AquaPolder-specifieke patronen. Onder andere:
- Failed login burst >5 per minuut op een account
- MFA-fatigue patroon (>10 push-prompts in 5 min)
- Login vanuit nieuw land voor een gebruiker
- Mass-download van SharePoint
- Suspicious mailbox-rule creation
- Admin-rol-toekenning buiten PIM
- AzureSQL geometric login pattern (klantportaal-misbruik)

Plus een handful threat-feeds via TI-connector.

**EG**: Wat zou je veranderen als je carte blanche had?

**KV**: Drie dingen. Een: een OT-passieve-monitoring oplossing - Claroty, Nozomi of Dragos - aansluiten op de OT-netwerk-mirror-port. Geen interventie, alleen zicht. Twee: PLC-toegangs-logs en HMI-logs onsluiten richting Sentinel. Drie: SAP integreren - SAP heeft tegenwoordig een Sentinel-connector via Enterprise Threat Detection, dat zou ik morgen aanzetten.

**EG**: Incident response - hoe gestructureerd is dat?

**KV**: Wij hebben aan onze kant een gestructureerd IR-runbook per scenario-type. Aan AquaPolder-kant is het minder gestructureerd. Er is een IR-plan uit 2019, dat is twee paginas, en de telefoonnummers daarin zijn deels niet meer geldig. We hebben een keer een tabletop gedaan in 2024, daar kwam uit dat AquaPolder onvoldoende geoefend is in besluitvorming bij een major incident. Dat is daarna niet opgevolgd.

**EG**: NIS2 meldplicht - kunnen jullie aan de 24-uurs early-warning voldoen?

**KV**: Wij kunnen aan onze kant het alert binnen 15 min uit Sentinel halen. De vraag is wie aan AquaPolder-kant beslist of het een meldingsplichtig incident is en wie het CSIRT-Drinkwater belt. Dat is nu niet gedefinieerd. Volgens mij is dat de CIO of de CEO, maar het staat niet zwart op wit.

**EG**: Vulnerabilities en patch management?

**KV**: Defender for Endpoint draait vulnerability assessment op de Windows-endpoints. Patching is geautomatiseerd via Intune voor de werkplekken. Server-patching is half-handmatig, gedraaid door Lieke's team. Voor SAP en OT: niet ons domein, daar kom ik niet bij.

**EG**: Laatste vraag: wat zou je willen dat AquaPolder anders deed?

**KV**: Twee dingen. Een: ze moeten iemand intern hebben die security-eigenaar is, niet de CIO 'erbij'. Twee: ze moeten OT serieus nemen. We zien nu een fractie van wat er gebeurt. Als iemand morgen via Siemens-VPN een PLC herprogrammeert, weet ik dat pas als er water uit een verkeerde kraan komt.

---

[OPEN VRAGEN]:
- Wat is de exacte retentie van Sentinel logs? (Karin noemde "ongeveer een jaar" - verifieren)
- Welke threat intel feeds zijn aangesloten?
- Wie heeft toegang tot de Sentinel-workspace vanuit AquaPolder-kant?
- Hoe vaak draait de threat-hunting query-suite, en wat is de scope?

[OPMERKING]: Karin noemt expliciet dat een Tier 1-analyst de eerste alert verkeerd heeft gehinten. Dit is relevant voor zowel de leveranciersbeoordeling (A.5.19-22) als de detectie/respons-controls (A.8.15-A.8.16).
