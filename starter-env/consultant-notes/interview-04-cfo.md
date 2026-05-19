# Interview 04 - Thomas Hertzog, CFO

**Datum**: 2026-04-17
**Locatie**: Zwolle, kantoor 4e verdieping (CFO-ruimte)
**Duur**: 45 minuten
**Aanwezig**: Thomas Hertzog (CFO), Erik Grasman (Quadero, interviewer)

---

**EG**: Thomas, vanuit financieel perspectief - waar zit jij in dit verhaal?

**TH**: Twee petten. Een: ik ben verantwoordelijk voor de financiele administratie, dus alles wat met SAP, klantbetalingen en jaarverslag te maken heeft, zit bij mij. Twee: ik ben in het bestuur degene die de cyber-investeringen moet onderbouwen. NIS2 is geen optie, dus dat gaat sowieso gebeuren - maar ik wil weten welke euro waar landt.

**EG**: Welke financiele systemen draaien er?

**TH**: SAP S/4HANA is de kern. Daar draait alles: debiteuren, crediteuren, hoofdboekhouding, asset-administratie, HR-administratie (loonadministratie zelf is uitbesteed aan een externe payroll-provider). Verbruiks-facturatie komt vanuit het klantportaal, wordt periodiek geimporteerd in SAP via een interface-laag.

Bankieren: we hebben Rabobank Direct Connect voor de betalingen. Ongeveer 8.000 betalingen per maand, en circa 12.000 incasso's voor klant-verbruik.

Voor de jaarverslag-rapportage gebruiken we Lucanet (consolidatie) en CCH Tagetik voor management-rapportages.

**EG**: Wie heeft toegang tot SAP?

**TH**: Brede vraag. Mijn team van 14 in finance, plus inkoop (8 mensen), HR (4 mensen), en een handvol mensen in operations voor asset-data. Plus de SAP-implementatiepartner (firma DeltaConsult B.V.) heeft consultants met admin-rechten - zij doen ons systeem-beheer. Plus ikzelf en de IT-manager hebben SAP_ALL.

**EG**: SAP_ALL voor jou en de IT-manager?

**TH**: Klopt. Dat is een erfenis uit de implementatie in 2021. We hebben gezegd "we breken het later netjes op" en dat is nooit gebeurd. Eerlijk gezegd weet ik dat het niet hoort.

**EG**: Hoe gaat bevoegdheidsbeheer in SAP?

**TH**: Rol-gebaseerd. Iedereen krijgt bij in-dienst een set rollen die past bij zijn functie. Aanpassingen lopen via een formulier dat ik of mijn plaatsvervanger goedkeurt. Periodieke review: jaarlijks loop ik per cost-center door wie nog toegang heeft. Dat document zit in SharePoint, maar niet altijd up-to-date.

**EG**: Beleid voor segregation of duties?

**TH**: Vormeel ingericht via SAP-rollen. We hebben SoD-conflict-checks aan de voorkant van rolaanvragen, en kwartaal-rapportages over kritieke SoD-combinaties (bv. wie kan crediteur aanmaken EN betaling goedkeuren). Dat is een van de dingen die we vrij goed op orde hebben - de externe accountant let hier op.

**EG**: Audit-logging in SAP?

**TH**: SAP heeft een security audit log, ingeschakeld. Logs blijven 18 maanden, conform onze richtlijn. Die logs gaan niet naar de MSSP - ze worden binnen SAP geanalyseerd door de SAP-partner als we iets vermoeden, of door de externe accountant tijdens de jaarcontrole. Dit is een gap die ik me bewust ben sinds Karin mij erop wees.

**EG**: Hoe staat het met SAP-patching?

**TH**: De SAP-partner DeltaConsult doet 4 patchcycli per jaar. Daar zit een veranderbeheer-proces op met een test-omgeving. Werkt over het algemeen goed, soms is er een patch die we uitstellen omdat het in een drukke financiele periode valt.

**EG**: Klantbetaaldata - hoe staat het daar mee?

**TH**: Klanten betalen op twee manieren: incasso (de hoofdmoot, IBAN bij ons in beheer) of via iDEAL via de Mollie-koppeling vanuit het klantportaal. Voor incasso bewaren we IBAN-nummers in SAP, beveiligd via standaard SAP-encryptie. Mollie-betalingen zien wij alleen als transactie-status, de betaalgegevens zelf zien wij niet (Mollie is de PSP).

PCI-DSS is dus niet van toepassing op ons, omdat we geen kaart-data verwerken. Wel hebben we een DPIA van 2023 voor de hele klant-betalings-stroom.

**EG**: Cyber-risico's vanuit financieel oogpunt - waar maak jij je het meest zorgen om?

**TH**: Drie dingen:
1. CEO-fraude / business email compromise. Onze inkoop is decentraal, en een kwaadwillende die zich voordoet als een leverancier en een rekening laat aanpassen, kan een serieus bedrag verplaatsen voor we het merken. We hebben verificatie-procedures, maar mensen zijn mensen.
2. Ransomware op SAP. Als SAP een week uit de lucht is, kunnen we niet factureren en niet betalen. De Veeam-backups staan in een tweede datacenter maar niet immutable - een geavanceerde aanvaller zou ze potentieel ook kunnen aantasten.
3. Dataverlies klantgegevens. Niet zozeer financiele schade (de boetes zijn klein vergeleken met onze omzet) maar reputatieschade en politieke schade. We zijn een nutsbedrijf met aandeelhoudende gemeentes - die hebben geen tolerantie voor blunders.

**EG**: NIS2 - bestuurdersaansprakelijkheid - hoe zit dat bij jullie?

**TH**: Onze CEO en ik hebben in december 2025 een NIS2-briefing gehad van een advocaat. De boodschap: aansprakelijkheid voor zorgplicht ligt bij de bestuurders, niet bij de IT-afdeling. De bestuurder moet aantoonbaar geinformeerd zijn over risico's en geinvesteerd hebben in mitigatie. Daarom is dit traject niet alleen een IT-zaak voor ons. We hebben een quarterly cyber-risk-update op de bestuursagenda gezet.

**EG**: Wat betekent dat in de praktijk?

**TH**: Dat ik elke kwartaal een 1-pager wil met: wat is het belangrijkste risico op dit moment, wat doen we eraan, wat hebben we van de toezichthouder gehoord, en wat is de stand van remediation-roadmap. Dat document moet straks ook auditable zijn - dat is een van de redenen waarom deze gap-analyse mij interesseert.

**EG**: Budget voor remediation - heb je een orde van grootte in gedachten?

**TH**: Voor 2026: indicatief 250-400k EUR voor IT-security maatregelen, niet OT. Voor 2027 verwacht ik substantieel meer omdat we dan ook OT-remediation gaan oppakken (geschat 500-800k voor passief OT-monitoring + DMZ-restructuring). Plus 1-1.5 FTE structureel voor een security-officer of CISO-light-rol vanaf 2027.

Dit is allemaal indicatief en hangt af van wat er uit jullie gap-analyse komt. Maar dit zijn de ordes van grootte die we in het bestuur hebben geaccepteerd.

**EG**: Wat is je voorkeur voor de menukaart?

**TH**: Hou de items concreet en relateer ze aan risico. Niet "implementeer Annex A.5.31" - dat zegt mij niets. Maar "stel een data-classificatie op met 3 niveaus en label de top-50 informatie-objecten, prijs 35k, doorlooptijd 8 weken, reduceert risico op A en B" - dat snap ik. En cluster gerelateerde items zodat we logische blokken kunnen kopen.

**EG**: Verzekering?

**TH**: We hebben een cyber-verzekering bij Hiscox sinds 2024. Dekking 5M EUR, met diverse uitsluitingen. Per 2027 wordt de verzekeraar strenger - ze willen aantoonbaar ISO-niveau security zien voor verlenging. Dat is ook een reden voor de 2027-certificering.

**EG**: Laatste: is er iets wat je gevraagd zou willen zien in het audit-rapport?

**TH**: Een appendix met financiele samenvatting: per gap een indicatie van remediation-kosten, of bandbreedtes. En, als je het kunt destilleren: wat is de kans dat een specifiek scenario (ransomware, klantdata-lek, OT-incident) ons treft, en wat is de potentiele financiele impact. Heel ruwe getallen mogen, maar iets om mee te werken.

---

[OPEN VRAGEN]:
- Welke specifieke SoD-rapportages levert SAP nu?
- Bestaat er een formele rol voor segregation of duties oversight in SAP?
- Hoe vaak en hoe diepgaand zijn de Veeam-restores getest?
- Is de cyber-verzekeringspolis te delen voor review onder NIS2-vereisten? (deductible, uitsluitingen)

[OPMERKING] - SAP_ALL bij CFO en IT-manager is een directe A.5.18 / A.8.2 gap. Markeren als high priority quick win.
