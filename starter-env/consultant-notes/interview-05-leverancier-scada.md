# Interview 05 - Niek Janssen, Leveranciersmanager (focus: Siemens SCADA-contract)

**Datum**: 2026-04-18
**Locatie**: Zwolle, kantoor 2e verdieping
**Duur**: 60 minuten
**Aanwezig**: Niek Janssen (Leveranciersmanager), Erik Grasman (Quadero, interviewer)

---

**EG**: Niek, jij doet leveranciers-management voor alle kritieke leveranciers?

**NJ**: Alle leveranciers vanaf een bepaalde drempel. Mijn portefeuille omvat Siemens, SAP-partner DeltaConsult, MSSP CyberWatch, Microsoft (via de reseller Insight), en de afval-/onderhoudspartners voor de zuiveringen. Onder dat niveau loopt het via inkoop, niet via mij.

**EG**: Beginnen we met Siemens. Wat is de contract-structuur?

**NJ**: We hebben een Master Services Agreement uit 2018, met daaronder periodieke aanhangsels. Het is een combinatie van:
- Een meerjaren-onderhoudscontract op de WinCC OA installaties (2024-2028)
- Een remote-support afspraak met SLA-tijden
- Project-aanhangsels per upgrade- of uitbreidings-project
- Reserve-onderdelen-overeenkomst voor PLC's en HMI's

Totale jaarlijkse betaling aan Siemens is ongeveer 480k EUR.

**EG**: Welke security-clausules zitten in het contract?

**NJ**: Eerlijk antwoord: het MSA uit 2018 is bedrijfsbreed-template van Siemens, en de security-clausules zijn relatief generiek. Confidentiality, sub-processors, return of data bij beeindiging. Geen specifieke bepalingen over:
- Notification bij security incidents bij Siemens-zijde
- Recht om audits uit te voeren bij Siemens
- Vereisten aan Siemens-employees met toegang tot ons systeem (training, certificeringen)
- Toepassing van NIS2-vereisten (was er in 2018 niet)

We zijn nu in onderhandeling over een addendum voor NIS2-compliance, maar dat sleept al sinds Q4 2025 omdat Siemens een eigen template wil gebruiken.

**EG**: Het gedeelde Siemens-support-account - hoe zit dat contractueel?

**NJ**: Daar zit een impasse. Siemens werkt vanuit een centrale "Customer Support Portal" waar al hun engineers met persoonlijke accounts inloggen. Vanuit dat portaal openen zij sessies naar klant-omgevingen via een team-credential dat de klant levert. Dat team-credential is bij ons het gedeelde 'siemens-support' account.

Wij hebben gevraagd of we per Siemens-engineer een individueel account kunnen leveren. Siemens zegt: technisch mogelijk maar contractueel niet, omdat hun support-bemensing fluctueert (verlof, vakantie, change of role) en wij dan continu moeten provisionen/deprovisionen.

Workaround die Siemens voorstelt: zij delen wekelijks een lijst met namen van engineers die in de afgelopen periode hebben ingelogd vanuit hun account. Dat hebben we sinds Q1 2026, en is een eerste stap.

**EG**: Bestaat er een formele beoordeling van Siemens als leverancier?

**NJ**: Een keer per 3 jaar doen we een leveranciers-audit voor de top-5 leveranciers. Voor Siemens was de laatste audit in 2022. Er staat geen formele cyber-component in, het is voornamelijk SLA-prestaties en commerciele relatie. We hebben in 2025 een gesprek met Siemens gevoerd over hun ISO 27001 certificering (zij zijn gecertificeerd) maar geen formele evidence opgevraagd of beoordeeld.

**EG**: NIS2 verplicht ketenbeoordeling van leveranciers. Hoe zit dat?

**NJ**: Dat is precies waar ik nu mee bezig ben. Onder de Cyberbeveiligingswet artikel 21 lid 2(d) moeten we leveranciersrisico beoordelen voor leveranciers die toegang hebben tot of essentieel zijn voor onze kritieke processen. Mijn lijst:
- Tier 1 (kritiek): Siemens, MSSP CyberWatch, DeltaConsult (SAP), Microsoft, Hostbedrijf datacenter
- Tier 2 (belangrijk): SAP-payroll-partner, schoonmaak/beveiliging zuiveringen, telefonie-provider
- Tier 3 (overig): kantoor-leveranciers, kleine licenties

Voor Tier 1 willen we voor eind 2026 een geformaliseerd beoordelings-proces met jaarlijkse review. Voor Tier 2 een 2-jaarlijkse review. Tier 3 alleen on-demand.

Het assessment zelf - ik weet nu nog niet exact welke vragenlijst we gaan gebruiken. Lieke heeft gevraagd of jullie iets adviseren.

**EG**: De andere kritieke leveranciers - hoe staat het daar?

**NJ**: Per leverancier:

**MSSP CyberWatch**: contract sinds 2022, vernieuwd in 2025. Bevat redelijke security-clausules omdat dit hun core-business is. ISO 27001 gecertificeerd, ISAE 3402 rapport beschikbaar. Sub-processor lijst beschikbaar. Incident notification verplicht binnen 24u.

**DeltaConsult (SAP)**: contract 2021-2027. Security-clausules zijn matig. ISO 27001 niet, hun hosting-partner wel. Hebben SAP_ALL toegang in onze productie-omgeving voor systeem-administratie. Geen formele audit door ons gedaan.

**Microsoft (via Insight)**: standaard CSP-overeenkomst. Hier zit weinig directe security-risico voor ons in - de Azure Shared Responsibility en M365 services zijn extern gecertificeerd. We controleren wel onze eigen configuratie.

**Datacenter**: Equinix Amsterdam, AM3. Tier III+, multi-customer. Goed gecertificeerd (ISO 27001, ISAE 3402, NEN 7510). Fysieke security is hun domein.

**EG**: Wat staat er in de leveranciers-onboarding nu?

**NJ**: Voor nieuwe leveranciers boven 50k jaarlijks is er een procedure: leverancier vult een checklist in (general business, financial health, security minimum), die wordt door mij + inkoop beoordeeld, en bij twijfel betrekken we Lieke. Voor leveranciers onder 50k loopt het via inkoop alleen. De checklist is uit 2022 en behoeft een NIS2-update.

**EG**: Hoe vaak verlaten leveranciers ons - en wat is dan het deprovisioning-proces?

**NJ**: Vrij weinig, gemiddeld 1-2 per jaar in de Tier 1/2-categorie. Proces: contract opzeggen, financiele afwikkeling, technical deprovisioning (toegang intrekken, accounts verwijderen, certificaten revoken), data return of destruction certificate vragen.

Het technische deprovisioning is in de praktijk soms slordig. We hebben in 2024 ontdekt dat een ex-leverancier een 'service' account had die nog steeds actief was in M365 (3 maanden na contract-einde). Sindsdien is er een Joiner-Mover-Leaver runbook voor leveranciers, maar het is reactief - dependent op of inkoop de juiste ticket open zet.

**EG**: Onder NIS2 zorgplicht 21 lid 2(d) - waar zie je je grootste gap?

**NJ**: Drie dingen:
1. Geen geformaliseerd risico-assessment voor leveranciers
2. Geen incident-notification-clausule in Siemens-MSA
3. Geen periodieke evidence-review van leveranciers-certificeringen (we vertrouwen erop dat ze nog geldig zijn, maar checken het niet)

Plus: geen ketenrisico-analyse, dus we weten niet welke sub-processors onze leveranciers gebruiken die mogelijk onze data of systemen raken. Bijvoorbeeld waar Siemens hun support-tickets opslaat. Geen idee.

**EG**: Wat zou je willen uit deze gap-analyse?

**NJ**: Concrete templates en checklists. Een NIS2-conforme leveranciersbeoordelings-vragenlijst, een addendum-template voor incident notification, en een matrix waarin staat welke clausules in welke contracten moeten zitten gegeven het leverancier-type. Met "doe maar wat zinvol is" kan ik niets, ik heb iets formeels nodig dat ik kan toepassen.

**EG**: Last but not least: heb je input over de SCADA-leverancier-aanpak?

**NJ**: Mijn opvatting: het gedeelde account is een feit waarmee we moeten leven, tenzij we van leverancier wisselen (en dat is geen optie). Maar we kunnen wel:
- Het account auditen aan onze kant: elke login wordt gecorreleerd met de wekelijkse Siemens-namen-lijst
- Just-in-time access: het account is normaliter disabled, en wordt door ons handmatig enabled na een ticket-aanvraag van Siemens
- Session recording: alle sessies van het siemens-support account worden opgenomen (full screen recording) en 6 maanden bewaard
- Per-installatie scoping: het account heeft alleen toegang tot de installatie waarvoor support gevraagd is

Dit is allemaal aan onze kant te bouwen, geen Siemens-instemming nodig.

---

[OPEN VRAGEN]:
- Welke versie heeft de leveranciers-checklist uit 2022?
- Bestaat er een formele Statement of Applicability voor leveranciers?
- Wat is de exacte tekst van de cyber-clausules in het MSA?
- Welke sub-processors gebruikt CyberWatch (MSSP)?
- ISAE 3402 type II rapport van CyberWatch - beschikbaar voor review?

[OPMERKING] - De voorgestelde JIT-aanpak voor het Siemens-account is in lijn met A.8.2 / A.8.3. Vermelden als concrete remediation-actie.
