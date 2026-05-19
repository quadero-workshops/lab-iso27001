# AquaPolder N.V. - IT-landschap

**Versie**: 2026-Q1
**Bron**: IT-afdeling AquaPolder, gevalideerd in interview 01 (Lieke van Bemmel)
**Markering**: VERTROUWELIJK

## 1. Identity en directory

- **Entra ID** (Microsoft) als enige identity provider
- ~250 actieve gebruikers (180 medewerkers + 50 leveranciers + 20 service-accounts)
- Conditional Access met basisbeleid:
  - MFA verplicht voor alle gebruikers behalve service-accounts (~20 uitgezonderd, bewust)
  - Block sign-in from anonymous IP
  - Require compliant device voor toegang tot SharePoint
  - Country-based block voor non-Europe (uitzondering: VS, met MFA)
- PIM voor Global Admin, Exchange Admin, SharePoint Admin (sinds 2024 Q3)
- Joiner-Mover-Leaver via HR-ticket naar IT, geen automatisering

## 2. End-user computing

- 250 endpoints, allemaal **Windows 11 Pro** (Enterprise SKU)
- Intune-managed sinds 2024-Q1
- Defender for Endpoint deployed op alle endpoints
- BitLocker enabled, recovery keys in Entra ID
- Edge als standaard browser, Chrome toegestaan maar niet officieel ondersteund
- Office 365 Apps deployed via Intune

**Niet onder Intune**:
- ~8 engineering workstations (Windows 10 LTSC 2019) in OT-omgeving - managed door OT-team
- ~5 macOS-laptops voor marketing/communicatie - JAMF-managed door externe partij

## 3. Productivity en collaboration

- **Microsoft 365 E5** voor 200 gebruikers, **E3** voor 80
- Exchange Online voor email
- SharePoint Online + OneDrive voor documenten
- Teams voor chat en meetings (geen externe sharing aan onbekende tenants)
- Defender for Cloud Apps (Cloud App Security) actief op M365 en een handvol SaaS
- Power BI Premium voor BI-rapportages

## 4. Servers en datacenter

### On-prem datacenter Zwolle
- 30 servers, mix Windows Server 2019/2022 en RHEL 8
- VMware vSphere 7 hosting platform
- Storage: NetApp FAS-cluster, 80 TB usable
- 4 RHEL servers hosten SAP S/4HANA productie
- 4 Windows servers voor file services en print

### On-prem datacenter Apeldoorn (DR-site)
- 12 servers (DR-replicas via Veeam)
- Storage: NetApp, 40 TB usable

### Azure (subscription: `aquapolder-prod-001`)
- ~25 resources, redelijk recent opgezet (sinds 2023)
- Tenant federated met on-prem Entra ID Connect
- Workloads:
  - **Klantportaal**: 2 App Services (Linux, .NET 8), Azure SQL Database (Hyperscale), Azure Front Door
  - **Data-platform**: 1 Synapse-workspace voor verbruiks-analyse, 1 Data Factory
  - **Logging**: 1 Sentinel-workspace (gedeeld met MSSP CyberWatch)
  - **Backup**: Azure Backup voor M365 (mailbox + SharePoint + OneDrive)
- Microsoft Defender for Cloud enabled (Standard tier op kritieke resources)

## 5. Kernapplicaties

| Applicatie | Functie | Hosting | Eigenaar |
|---|---|---|---|
| SAP S/4HANA | ERP (finance, asset, HR-admin) | On-prem Zwolle | Thomas Hertzog (CFO) |
| Klantportaal | Verbruiksinzage + betaling | Azure App Service | Lieke van Bemmel (CIO) |
| WinCC OA | SCADA voor zuiveringen | On-prem (per installatie) | Robert Koster (OT) |
| PI System | Data historian | On-prem Zwolle central | Robert Koster (OT) |
| Lucanet | Financiele consolidatie | Cloud (Lucanet SaaS) | Thomas Hertzog (CFO) |
| Tagetik | Management reporting | Cloud (Wolters Kluwer SaaS) | Thomas Hertzog (CFO) |
| HR-self-service | Verlof, declaraties | Cloud (Visma Easycruit) | HR |
| Payroll | Loonadministratie (uitbesteed) | Visma SaaS | HR |

## 6. Netwerk-aansluitingen

- Externe internet-aansluiting: 2x Eurofiber 1 Gbit symmetrisch (active-passive)
- Edge firewall: Fortinet FortiGate 600F-cluster
- VPN: Fortinet SSL-VPN voor remote werken (~180 gebruikers met VPN-recht) + dedicated OT-VPN
- Filiaal-aansluitingen: per zuiveringsinstallatie een Eurofiber dark fiber naar Zwolle DC, met locale Fortinet 40F als back-up via 4G

## 7. Software development en CI/CD

- Klantportaal-development door externe partij (Bird&Co B.V. uit Deventer)
- Source code in private GitHub.com org (`aquapolder-org`)
- CI/CD: GitHub Actions naar Azure
- Geen interne developers; alle wijzigingen via Bird&Co tickets

## 8. Bekende uitzonderingen / legacy

- 1 server (Windows Server 2012 R2) draait nog "het oude meldsysteem" - wordt Q4 2026 uitgefaseerd
- ~12 Windows 10 laptops zijn nog niet gemigreerd naar Windows 11 (planning Q2 2026)
- Een handful van shared mailboxes met basic auth voor legacy-applicatie-integratie (planning Q2 2026 deprecate)
