# Interview 03 - Robert Koster, Manager OT-engineering

**Datum**: 2026-04-17
**Locatie**: Zuiveringsinstallatie Zwolle-noord, kantoorruimte naast Central Control Room
**Duur**: 90 minuten
**Aanwezig**: Robert Koster (Manager OT, geinterviewde), Erik Grasman (Quadero, interviewer)

---

**EG**: Robert, dank voor de tijd. Laten we met de basis beginnen - hoe is OT bij AquaPolder opgebouwd?

**RK**: We hebben vier zuiveringsinstallaties. Elke installatie heeft een eigen lokale automatiserings-architectuur, maar ze zijn allemaal aangesloten op een central control room hier in Zwolle. We werken met Siemens als hoofdleverancier sinds 2008. Toen is de eerste generatie WinCC geinstalleerd, en in 2018-2019 zijn we gemigreerd naar WinCC OA met een redundante server-opstelling.

**EG**: Wat draait er precies?

**RK**: Per installatie:
- 2 WinCC OA servers (active-passive, fysieke Siemens IPC's)
- 1 Engineering workstation (Windows 10 LTSC 2019, niet Intune-managed, want Siemens compatibility)
- 1 of 2 HMI's in de control room (Siemens TP1500 Comfort touchscreens)
- 20-40 PLC's (Siemens S7-1500, sommige nog S7-300 die we de komende 2 jaar uitfaseren)
- Diverse sensors, drukmeters, kwaliteitsanalysers (FLO, pH, troebelheid, chloor)

In totaal over de 4 installaties:
- 8 WinCC OA servers
- 4 engineering workstations
- ~40 HMI's incl. kleine touchpanels op pomp-stations
- ~120 PLC's
- Honderden sensoren en actuatoren

Central:
- 2 redundante PI System servers (data historian, alle proces-data)
- 1 reporting-server voor maand- en jaarrapportages voor het Vitens-netwerk (uitwisseling waterkwaliteitsdata)
- Een sprongserver in de OT-DMZ voor remote support van Siemens

**EG**: Welke ICS protocol-stack?

**RK**: PROFINET op de OT-fabrieksniveau-bussen, OPC UA tussen WinCC OA en de PI System, en S7comm voor de directe PLC-communicatie. Geen Modbus, geen DNP3 - dat hebben we historisch niet gebruikt.

**EG**: Hoe ziet het netwerk eruit, simpel uitgelegd?

**RK**: Purdue-model achtig. Niet by-the-book, maar in de buurt.
- Level 0: Sensors/actuators (PROFINET, direct aan PLC)
- Level 1: PLC's
- Level 2: HMI's, lokale supervisory
- Level 3: WinCC OA servers, PI System, engineering workstations
- Level 3.5 / DMZ: hier zit de sprongserver voor Siemens-remote-support. Lieke noemt dit "OT-DMZ" maar het is eigenlijk een Level 3 segment met een firewall ervoor naar IT.
- Level 4: Kantoor-IT (M365, SAP, kantoorwerkplekken)

De firewall tussen Level 3.5 en Level 4 is een Fortinet die door IT wordt beheerd. We hebben unidirectional data flow van PI System naar een SQL-replica in het IT-netwerk voor management reporting - dat loopt over een gewone firewall-regel, geen data diode.

**EG**: Hoe ziet remote toegang eruit?

**RK**: Siemens heeft een remote-support contract. Zij loggen ongeveer 2-4 keer per maand in voor onderhoud, patches, of incident-support. Dat gaat via een Fortinet SSL-VPN, en dan SSH-tunnel naar de sprongserver. Voor het Q3 incident gebruikten ze een gedeeld account 'siemens-support' met wachtwoord-only authenticatie. Sinds het incident is MFA toegevoegd (Microsoft Authenticator op een dienst-telefoon bij Siemens).

Daarnaast hebben wij zelf - mijn 12 OT-engineers - VPN-toegang naar het OT-netwerk via Fortinet voor wanneer we vanuit huis moeten ingrijpen bij een storing. Dat gebruiken we ongeveer 5-10 keer per maand, vooral 's nachts. Elk OT-engineer heeft een persoonlijk VPN-account met MFA.

**EG**: Het gedeelde Siemens-account voor het incident - bestaat dat nog?

**RK**: Ja, helaas wel. Siemens kan technisch geen individuele accounts gebruiken vanwege hoe hun support-portal werkt - alle hun engineers loggen in onder een team-account dat AquaPolder aan hun kant geleverd heeft. We hebben er druk op gezet via Niek (leveranciersmanagement) maar het is een tegenslag in het contract.

**EG**: Patching van OT-systemen?

**RK**: WinCC OA: 2 keer per jaar een patch-window van een nacht, in overleg met Siemens. Patches komen van Siemens, getest in hun lab, dan bij ons uitgerold tijdens een laagvraag-moment in de zuivering. PLC firmware: 1 keer per 2-3 jaar, of bij een security-advisory van Siemens. Engineering workstations: hier zit een gat. Die draaien Windows 10 LTSC 2019 en krijgen handmatig kwartaal-patches van mijn team. Geen Intune, geen WSUS - we downloaden van Microsoft Update Catalog en patchen handmatig.

Sensoren en actuatoren: zelden gepatched, vaak boot-time-only firmware. Levensduur van die hardware is 10-15 jaar, dus we zitten met legacy.

**EG**: Wat zijn de PLC-toegangscontroles?

**RK**: S7-1500 PLC's hebben in TIA Portal ingestelde 'Access Levels' met een wachtwoord per niveau. Engineering-level (full read/write) heeft een ander wachtwoord per zuiveringsinstallatie. HMI/read-only access heeft een eigen wachtwoord, die zit hard-gecodeerd in de WinCC OA configuratie. Wachtwoorden zijn niet uniek per PLC binnen een installatie, en de set is een paar jaar oud.

S7-300 (oudere, uitfaserings-fase) hebben geen toegangs-niveaus. Iedereen met netwerk-toegang kan ze herprogrammeren als hij TIA Portal heeft. Dat is een bekend issue.

**EG**: Audit-logging op PLC-niveau?

**RK**: S7-1500 logt configuratie-wijzigingen lokaal, maar dat moet je actief uitlezen. Wij doen dat niet structureel - alleen na een incident of als we problemen vermoeden. Er gaat niets richting Sentinel.

**EG**: Backup van PLC-configuraties?

**RK**: Siemens neemt elke 6 maanden een backup van de TIA Portal-projecten, die staan ergens op hun infrastructuur. Wij zelf hebben een lokale backup van het laatst-gedeployde project per PLC, gemaakt door de engineer die de deploy heeft gedaan. Dat is geen structureel proces - geen versie-beheer, geen central repository. Een wijziging die Siemens doet zonder ons in te lichten kunnen we nu niet detecteren.

**EG**: Asset-management - heb je een centraal overzicht?

**RK**: Voor de zuiveringsinstallatie als geheel: ja, in SAP. Pomp X is asset Y, PLC Z is asset W. Voor de cyber-security-relevante details (firmware-versie, exposed ports, patch-level, eigenaar) - nee, dat ligt verspreid over Excel-sheets per installatie.

**EG**: Wat moet er voor NIS2 in de OT-kant?

**RK**: Eerlijk, ik weet niet alles van NIS2. Wat ik begrijp: aantoonbare zorgplicht voor de cyber-resilience van de operationele technologie. Dus: risk assessment, controls, monitoring, response, business continuity. Aan dat laatste werken we al lang - we hebben uitgebreide BCM-plannen voor drinkwater-continuiteit. Maar specifiek cyber-resilience: dat is nieuw.

**EG**: Hoe staat het met de cyber-volwassenheid van het OT-team?

**RK**: Mijn engineers zijn experts in proces-automatisering. Cyber security is voor ons een neven-onderwerp dat de laatste 5 jaar steeds zwaarder weegt. We hebben er geen formele training in. Twee van mijn engineers hebben een GICSP getekend om ervaring op te doen - dat is een Global Industrial Cyber Security Professional certificaat. De rest leert on-the-job.

**EG**: Wat zou je veranderen als budget geen issue was?

**RK**: Drie dingen, in volgorde:
1. Een echte OT-DMZ inrichten met een data diode richting IT. Geen bidirectionele firewall-regel meer, maar fysiek unidirectioneel.
2. Een passieve OT-monitor (Claroty/Nozomi) aansluiten op de mirror-poorten van de Level 2-switches. Dat geeft ons asset-inventarisatie, anomaliedetectie, en threat-intel zonder dat het de proces-continuiteit raakt.
3. PLC-versie-beheer en centrale backup. Git voor PLC-projects.

**EG**: Verwachtingen ten aanzien van deze audit?

**RK**: Wees eerlijk. Ik weet dat we issues hebben. Wat ik niet wil is dat de audit een lijst van 200 punten produceert die te onbetaalbaar of te disruptief is om aan te pakken. Wat ik wil is: 10-15 dingen die echt de naald bewegen, met een implementatie-pad dat ik in mijn bestaande team kan inpassen.

**EG**: Laatste: zijn er onderwerpen die we nu nog niet hebben aangeraakt?

**RK**: Twee dingen. Een: we hebben geen formeel OT change management. Een engineer die in de TIA Portal iets aanpast naar een PLC kan dat gewoon, zonder peer review. Dat is operationeel handig maar audit-technisch een gap.

Twee: we hebben fysieke toegang op de zuiveringen die niet uniform geregeld is. Zwolle-noord heeft toegangskaarten met audit-log. Wijhe en Kampen hebben gewone sleutels. Hardenberg heeft een mix. Onder NIS2 zou ik daar uniformiteit verwachten.

---

[OPEN VRAGEN]:
- Hoeveel TIA Portal-wachtwoorden zijn er in totaal in omloop?
- Is er een uniek wachtwoord per zuiveringsinstallatie of een gedeelde set?
- Welke versie WinCC OA draait waar?
- Wat is het patch-niveau van de S7-1500 PLC's (firmware)?
- Hoe vaak worden de PLC-backups die Siemens maakt op restore-baarheid getest?

[OPMERKING] - Robert verwijst meermaals naar "Vitens-netwerk" voor data-uitwisseling. Dit moet verder onderzocht: is dit een sector-brede uitwisseling, en welke datastromen lopen er?
