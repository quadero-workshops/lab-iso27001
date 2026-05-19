# Interview 01 - Lieke van Bemmel, CIO

**Datum**: 2026-04-15
**Locatie**: Zwolle, kantoor 3e verdieping
**Duur**: 75 minuten
**Aanwezig**: Lieke van Bemmel (CIO, geinterviewde), Erik Grasman (Quadero, interviewer)
**Notitietype**: Letterlijke aantekeningen (geanonimiseerd waar nodig)

---

**EG**: Lieke, dank voor je tijd. Kun je in eigen woorden vertellen waar AquaPolder nu staat qua information security?

**LvB**: Eerlijk antwoord: we zijn een drinkwaterbedrijf, geen tech bedrijf. We hebben jarenlang gedacht dat security iets is dat de leverancier wel regelt. Microsoft regelt M365. Siemens regelt SCADA. Wij regelen het water. Dat werkt natuurlijk niet meer. Sinds NIS2 staat het bestuur erop.

**EG**: Beschrijf eens de IT-omgeving op hoofdlijnen.

**LvB**: We zitten volledig in Microsoft 365. Entra ID is de identity provider, alle gebruikers daar. Daarnaast hebben we Azure - een subscription, redelijk recent opgezet (~2 jaar). Daar draait ons klantportaal, een custom .NET app waarin klanten hun verbruik kunnen zien en betalingen doen. De backend daarvan staat in Azure SQL.

Verder hebben we on-prem nog een SAP S/4HANA omgeving, gehost op vier RHEL-servers in ons eigen datacenter in Zwolle. Dat draait alle financiele administratie en assets. Daar willen we eigenlijk vanaf, naar SAP cloud, maar dat is een meerjaren-traject.

We hebben rond de 250 endpoints, allemaal Windows 11, Intune-managed sinds 2024. Daarvoor was het een puinhoop, dus dat hebben we behoorlijk opgeruimd.

**EG**: En de OT-kant?

**LvB**: Daar zit ik officieel ook op, maar in de praktijk runt Robert dat. Vraag het hem maar uit. Wat ik weet: 4 zuiveringsinstallaties met Siemens WinCC OA als SCADA, PI System als historian, en ongeveer 120 PLC's verspreid. Het OT-netwerk is fysiek gescheiden van IT - "air gap" zegt Robert altijd, maar er zijn wel raakvlakken. Engineering workstations zitten in een eigen subnet, en er is een gedeeld Siemens-account voor remote support.

**EG**: Dat gedeelde Siemens-account - is dat het account waar het Q3 2025 incident mee speelde?

**LvB**: Klopt. Twee OT-engineers kregen een phishing-mail, een opende hem, en die voerde credentials in op een nep-Siemens-portaal. Daarna konden de aanvallers via VPN inloggen op een sprong-server in de OT-DMZ. We hebben het pas 11 dagen later gezien doordat de MSSP een UEBA-alert had op login-patroon. Onderzoek toonde geen lateral movement, dus we zijn met een blauw oog weggekomen. Sindsdien is MFA toegevoegd op de VPN en is het wachtwoord gewijzigd, maar de structurele issues - gedeeld account, geen segmentatie tussen OT-DMZ en SCADA - die staan er nog.

**EG**: Is er een formeel ISMS?

**LvB**: Nee. We hebben een security policy uit 2022, vier pagina's. Er zijn losse procedures - incident response uit 2019, backup-strategie uit 2023 - maar geen samenhangend systeem. Geen Statement of Applicability, geen formele risk register. We willen dat opbouwen vanuit deze gap-analyse.

**EG**: Wie binnen AquaPolder is verantwoordelijk voor security?

**LvB**: Officieel ik, als CIO. In de praktijk: niemand fulltime. Karin is SOC-coordinator maar zit zelf bij de MSSP. We hebben geen dedicated CISO. Dat moet veranderen, maar dat is een 2027-discussie.

**EG**: Hoe zit het met security awareness en training?

**LvB**: Vrijwel niets. Bij in-dienst krijgt iemand een welkomstpakketje waar 1 A4 over phishing in zit. Daarna stilte. Geen jaarlijkse training, geen phishing-simulaties. Na het Q3 incident hebben we een eenmalige awareness-sessie gehad voor IT en OT, maar dat was reactief, niet structureel.

**EG**: Backups?

**LvB**: Veeam op de IT-kant, repliceert naar een tweede datacenter in Apeldoorn. Voor M365 hebben we Azure Backup. We testen restores eens per kwartaal voor productie-systemen, niet voor alles. Voor de OT-kant: de PLC-configuraties worden door Siemens periodiek geback-upt - we hebben zelf geen kopie. Dat is een gap die we kennen.

**EG**: Logging en monitoring?

**LvB**: M365 audit logs gaan naar onze MSSP, die heeft Sentinel. Azure-logs idem. SAP-logs - daar kom ik niet zo goed uit, dat is een vraag voor de SAP-partner. OT-logging: PI System logt natuurlijk de procesdata, maar security-events op het OT-netwerk hebben we vrijwel niet. PLC-toegangs-logs bestaan, maar worden niet gemonitord.

**EG**: Toegangsbeheer in M365?

**LvB**: Entra ID met Conditional Access. MFA verplicht voor alle gebruikers behalve een paar service-accounts (daar zit ik ook al een tijd op). We hebben PIM voor admin-rollen, redelijk recent uitgerold. Joiner-mover-leaver loopt via HR die een ticket maakt voor IT - geen automatisering, maar het werkt redelijk.

**EG**: Wat is je eigen grootste zorg?

**LvB**: Dat we ergens een blinde vlek hebben die we niet kunnen overzien. Bijvoorbeeld leveranciers - we werken met Siemens, SAP, MSSP, en een handvol kleine partijen. Onder NIS2 moeten we leveranciersrisico beoordelen en aantoonbaar maken. Dat hebben we nu nul georganiseerd.

En, eerlijk: ik maak me zorgen over de OT-kant. Niet zozeer omdat Robert zijn werk niet goed doet, maar omdat OT-security een specialisme is dat wij intern niet hebben.

**EG**: Verwachtingen ten aanzien van deze gap-analyse?

**LvB**: Drie dingen. Een: een lijst waarmee ik richting het bestuur kan zeggen "dit moet, dit zou moeten, dit is nice-to-have". Twee: een prioritering op basis van risico, niet op basis van implementatiegemak. En drie: iets waarmee we de externe auditor over twee jaar kunnen laten zien dat we het serieus doen.

**EG**: Helder. Laatste vraag: zijn er onderwerpen waarvan jij denkt "die ga je niet in andere interviews horen, maar je moet ze wel weten"?

**LvB**: Twee dingen. Een: we hebben een lopende rechtszaak rondom een datalek uit 2023 met een aannemer die per ongeluk klantgegevens op een onbeveiligde FTP heeft gezet. Dat is buiten scope voor jouw onderzoek, maar het verklaart waarom het bestuur zo terughoudend is over publiek communiceren over security.

Twee: de relatie tussen IT en OT is historisch gespannen. Robert is een prima vent maar zijn team voelt zich vaak gepasseerd. Hou daar rekening mee in je interview met hem.

---

[ONDUIDELIJK] - Lieke noemde "een gedeeld account voor lab-omgevingen" maar werd toen onderbroken. Vervolgvraag noodzakelijk.

[OPEN VRAGEN]:
- Welke logging-retentie hanteert de MSSP?
- Welke service-accounts hebben geen MFA?
- Hoe wordt klantbetaaldata in het portaal opgeslagen (PCI-DSS relevant?)
- Bestaat er een data-classificatie?
