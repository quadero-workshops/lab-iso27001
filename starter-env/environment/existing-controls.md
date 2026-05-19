# AquaPolder N.V. - Bestaande beveiligingsmaatregelen

**Versie**: 2026-Q1
**Bron**: Compilatie uit interviews 01-05 + IT-documentatie
**Markering**: VERTROUWELIJK

> Dit document beschrijft de **status quo** van beveiligingsmaatregelen bij AquaPolder. Het dient als input voor:
> 1. Gap-analyse: welke controls zijn aanwezig vs vereist (Annex A)
> 2. MITRE ATT&CK positioning: welke dreigingstactieken zijn gedekt
> 3. Maturity scoring: hoe volwassen is de implementatie per control

## A.5 - Organizational Controls

| Aspect | Status quo | Maturity (indicatief CMM) |
|---|---|---|
| Information Security Policy | Beleidsdocument 2022, 4 pagina's, niet recent gereviewed | 1 (Initial) |
| Information Security Roles | CIO is verantwoordelijk, geen CISO. Karin (extern) is SOC-coordinator. | 1-2 |
| Segregation of Duties | SAP rol-gebaseerd ingericht met SoD-controles, maar SAP_ALL voor CFO+IT-manager | 2-3 |
| Management Responsibilities | Bestuur ontvangt kwartaal-update vanaf Q4 2025 | 2 |
| Contact with Authorities | Geen formeel contact met NCSC of toezichthouders | 0 |
| Contact with Special Interest Groups | Deelname Vewin technisch-overleg cyber (4x/jaar), aansluiting op WaterISAC | 2 |
| Threat Intelligence | TI via MSSP (Sentinel-connector). Geen interne TI-functie | 2 |
| Supplier Relationships | Procedure bestaat voor leveranciers > 50k. NIS2-update in progress | 1-2 |
| Information Security in Project Management | Geen formeel security-by-design in projecten | 0-1 |
| Inventory of Assets | Asset-register voor fysieke assets in SAP. Information assets niet formeel ingericht | 1 |
| Acceptable Use of Assets | Acceptable Use Policy 2022, gecommuniceerd aan nieuwe medewerkers | 1 |
| Return of Assets | Onboarding/offboarding checklist via HR | 2 |
| Classification of Information | Geen data-classificatie. Alles "intern" by default | 0 |
| Labelling of Information | Geen labelling | 0 |
| Information Transfer | Geen formeel beleid, M365 sensitivity-labels niet uitgerold | 0 |
| Access Control | Entra ID rol-gebaseerd, Conditional Access | 3 |
| Identity Management | Entra ID JML via HR-tickets | 2 |
| Authentication Information | M365 MFA verplicht, password-policy in Entra ID. Geen separate guidance | 2 |
| Access Rights | Periodieke review SAP rolltoekenning. M365 review ad-hoc | 2 |
| Information Security in Supplier Relationships | Checklist 2022, niet NIS2-compliant | 1 |
| Addressing Information Security in Supplier Agreements | Generieke MSA-clausules, geen incident notification clausule | 1 |
| Managing Information Security in the ICT Supply Chain | Geen sub-processor-mapping | 0 |
| Monitoring, Review and Change Management of Supplier Services | Ad-hoc | 1 |
| Information Security for Use of Cloud Services | Default M365 / Azure security baselines, geen formeel beleid | 2 |
| Information Security Incident Management Planning | IR-plan 2019, 2 paginas. Telefoonnummers deels outdated | 1 |
| Assessment and Decision on Information Security Events | MSSP doet eerste triage, geen formele besluitvorming aan AquaPolder-kant | 1-2 |
| Response to Information Security Incidents | Ad-hoc, post-incident reviews ontbreken vaak | 1 |
| Learning from Information Security Incidents | Q3 2025 incident: enkele aanpassingen gedaan, geen structurele opvolging | 1 |
| Collection of Evidence | Niet geformaliseerd | 0 |
| Information Security During Disruption | BCM-plan voor drinkwater-continuiteit, niet voor cyber-incident-induced disruption | 1-2 |
| ICT Readiness for Business Continuity | DR-site Apeldoorn, getest 2x/jaar voor productie-systemen | 3 |
| Legal, Statutory, Regulatory Requirements | NIS2-traject in gang. AVG sinds 2018. Geen sectorspecifieke compliance-check | 2 |
| Intellectual Property Rights | Geen specifieke maatregelen | 1 |
| Protection of Records | M365 retention policies voor email, beperkt voor SharePoint | 1-2 |
| Privacy and Protection of PII | DPIA voor klantbetalings-stroom (2023). Geen Privacy Officer | 2 |
| Independent Review | Externe accountant kijkt naar IT-controles voor jaarrekening. Geen pen-test, geen audit | 1 |
| Compliance with Policies, Rules and Standards | Geen interne compliance-audits | 0-1 |
| Documented Operating Procedures | IT-procedures grotendeels gedocumenteerd. OT-procedures fragmentair | 2 |

## A.6 - People Controls

| Aspect | Status quo | Maturity |
|---|---|---|
| Screening | Pre-employment screening voor management-rollen. Geen security clearance proces | 1 |
| Terms and Conditions of Employment | Standaard NL arbeidsovereenkomst met confidentiality-clausule | 2 |
| Information Security Awareness, Education and Training | Welkomstpakketje 1A4 over phishing. Eenmalige awareness-sessie post-Q3-incident. Geen jaarlijkse training, geen phishing-simulaties | 0-1 |
| Disciplinary Process | Generiek disciplinair-beleid, niet security-specifiek | 1 |
| Responsibilities After Termination or Change of Employment | Onboarding/offboarding checklist | 2 |
| Confidentiality or Non-disclosure Agreements | NDA voor alle medewerkers en kritieke leveranciers | 2 |
| Remote Working | M365 + Fortinet VPN + Defender for Endpoint. Compliant device required | 3 |
| Information Security Event Reporting | Outlook Report-button (phish-button) actief. Andere events ad-hoc | 2 |

## A.7 - Physical Controls

| Aspect | Status quo | Maturity |
|---|---|---|
| Physical Security Perimeters | Hoofdkantoor Zwolle: hekwerk, ANPR, bewaakte ingang | 3 |
| Physical Entry | Hoofdkantoor: badges (Salto). Zuiveringen: mix (Zwolle-noord badges, andere sleutels) | 1-2 (zuiveringen) / 3 (kantoor) |
| Securing Offices, Rooms and Facilities | Standaard kantoor-security, kasten met sloten voor gevoelige docs | 2 |
| Physical Security Monitoring | CCTV op hoofdkantoor en zuiveringen, retentie 30 dagen | 2 |
| Protecting Against Physical and Environmental Threats | UPS, brandblussers, water/inbraak-detectie in DC's | 2-3 |
| Working in Secure Areas | Niet geformaliseerd voor SCADA-ruimtes | 1 |
| Clear Desk and Clear Screen | Beleid bestaat, naleving wisselend | 1-2 |
| Equipment Siting and Protection | DC-omgevingen op orde. Zuiveringen: variatie | 2 |
| Security of Assets Off-premises | Laptop-beleid met BitLocker + remote-wipe via Intune | 3 |
| Storage Media | M365-data in cloud. USB-sticks beperkt door Intune. Geen formele media-management | 2 |
| Supporting Utilities | Generatoren bij DC + zuiveringen, dual-power | 3 |
| Cabling Security | Standaard fabric-security in DC's | 2 |
| Equipment Maintenance | Per leverancier, niet centraal aangestuurd voor security-aspect | 2 |
| Secure Disposal or Re-use of Equipment | Externe partij (eRecycling B.V.) met destructie-certificaten | 2-3 |

## A.8 - Technological Controls (technische maatregelen, 34 controls)

| Aspect | Status quo | Maturity |
|---|---|---|
| User Endpoint Devices | Intune-managed Windows 11, Defender for Endpoint | 3 |
| Privileged Access Rights | PIM voor M365 admin-rollen. SAP_ALL nog niet opgesplitst | 2 |
| Information Access Restriction | Entra ID role assignments, SharePoint perms | 2 |
| Access to Source Code | GitHub.com private org met branch protection | 2-3 |
| Secure Authentication | MFA M365, MFA Fortinet-VPN. Niet uniform op alle systemen (SAP_ALL, OT-domain zonder MFA) | 2 |
| Capacity Management | Standaard infrastructuur-monitoring | 2 |
| Protection Against Malware | Defender for Endpoint + Defender for Cloud + Defender for O365 | 3 |
| Management of Technical Vulnerabilities | Defender for Endpoint vuln-mgmt. Niet voor SAP, niet voor OT | 2 |
| Configuration Management | Intune baselines voor endpoints. Server baselines niet geformaliseerd | 2 |
| Information Deletion | Bewaar-termijn beleid bestaat, naleving variabel | 1-2 |
| Data Masking | Niet toegepast | 0 |
| Data Leakage Prevention | Defender for Cloud Apps DLP op M365 (basis), geen Exact-Match policies | 2 |
| Information Backup | Veeam IT + Veeam OT. Klant-database (Azure SQL) auto-backup | 2-3 |
| Redundancy of Information Processing Facilities | DC-redundantie tussen Zwolle en Apeldoorn | 3 |
| Logging | M365, Azure, Fortinet -> Sentinel. SAP, OT-deels ontbreken | 2 |
| Monitoring Activities | MSSP 24x7 op aangesloten bronnen. Niet OT, niet SAP | 2 |
| Clock Synchronisation | NTP via Microsoft, Fortinet en eigen Stratum 2 server | 3 |
| Use of Privileged Utility Programs | Beperkt via Intune. OT-engineering tools (TIA Portal) op engineering workstations | 1-2 |
| Installation of Software on Operational Systems | Intune restrictie voor endpoints. Servers: handmatig | 2 |
| Networks Security | Fortinet + segmentatie. Microsegmentatie binnen DC niet doorgevoerd | 2 |
| Security of Network Services | Standaard hardening Fortinet | 2-3 |
| Segregation of Networks | OT/IT-scheiding via OT-Border firewall. Wel bidirectional regel naar PI | 2 |
| Web Filtering | Fortinet web filter, M365 SafeLinks | 2-3 |
| Use of Cryptography | M365 native, BitLocker, TLS overal. Geen formeel sleutelbeheer | 2 |
| Secure Development Life Cycle | Niet ingericht (klantportaal-dev via Bird&Co) | 1 |
| Application Security Requirements | Geen formele requirements-set | 0-1 |
| Secure System Architecture and Engineering Principles | Geen formele architectuur-principes documentatie | 1 |
| Secure Coding | Niet geformaliseerd | 0-1 |
| Security Testing in Development and Acceptance | OWASP-scan eenmalig 2024 op klantportaal. Niet recurring | 1 |
| Outsourced Development | Bird&Co contract, security-eisen redelijk maar niet diepgaand | 1-2 |
| Separation of Development, Test and Production Environments | Azure: dev/test/prod environments. SAP: dev/test/prod. OT: alleen prod | 2-3 (IT) / 0 (OT) |
| Change Management | IT change board kwartaalmatig. OT: ad-hoc | 2 (IT) / 0-1 (OT) |
| Test Information | Anoniem voor klantportaal, niet voor SAP-tests | 1 |
| Protection of Information Systems During Audit Testing | Niet specifiek geadresseerd | 1 |

## Aansluitend bij ISO 27001:2022 Clauses (4-10)

| Clause | Status | Notities |
|---|---|---|
| 4. Context of the Organization | Deels | Stakeholder-mapping ad-hoc |
| 5. Leadership | Deels | Bestuurs-commitment groeiend, niet formeel ISMS-policy |
| 6. Planning | Niet | Geen formele risk-treatment plan |
| 7. Support | Deels | Resources allocated (budget), competenties verschillen |
| 8. Operation | Niet | Geen procesgebaseerde operationele controls-set |
| 9. Performance Evaluation | Niet | Geen formele metingen, geen interne audits, geen management review |
| 10. Improvement | Niet | Geen CAPA-proces |

## Samenvatting: gap-anatomie

- **Sterke punten**: M365 / Azure / endpoint security, BCM voor drinkwater-continuiteit
- **Aanwezig maar onvolwassen**: IR-plan, leveranciersbeheer, change management
- **Grote gaten**: ISMS-structuur (Clauses 6-10), data-classificatie, OT-security volwassenheid, security awareness, formele incident response, SoD-uitzonderingen (SAP_ALL)
