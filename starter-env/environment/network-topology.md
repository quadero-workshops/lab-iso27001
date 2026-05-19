# AquaPolder N.V. - Netwerktopologie

**Versie**: 2026-Q1
**Bron**: Netwerkbeheer (Marc de Jong, intern), gevalideerd in interviews 01 + 03
**Markering**: VERTROUWELIJK

## Hoofdtopologie

```
Internet
   |
   +-- ISP-A (Eurofiber primary, 1 Gbit/s)
   +-- ISP-B (Eurofiber backup, 1 Gbit/s)
   |
[Edge Fortinet 600F cluster, HA] (Zwolle datacenter)
   |
   +-- DMZ-1 (publieke services): reverse proxy voor klantportaal
   |   (publishedrelies: Azure Front Door, dus DMZ is beperkt)
   |
   +-- DMZ-2 (extranet): SFTP-server voor Vitens-data-uitwisseling, sub-mailservers
   |
   +-- IT-Core (Level 4 enterprise):
   |   |
   |   +-- Server-VLAN (10.10.20.0/22) - 30 servers
   |   +-- Client-VLAN (10.10.30.0/22) - werkplekken
   |   +-- Management-VLAN (10.10.10.0/24) - admin-tooling
   |   +-- Voice-VLAN (10.10.40.0/24) - Teams Phone
   |
   +-- OT-Border firewall (separate Fortinet 200F)
       |
       +-- OT-DMZ / Level 3.5 (10.20.10.0/24): sprongserver Siemens-support
       |
       +-- OT-Core / Level 3 (10.20.20.0/22): WinCC OA servers, PI System
       |
       +-- per-installatie subnets:
           |
           +-- Zwolle-noord (10.20.110.0/24)
           +-- Kampen (10.20.120.0/24)
           +-- Hardenberg (10.20.130.0/24)
           +-- Wijhe (10.20.140.0/24)
           |
           +-- Per installatie:
               - Level 2 HMI-segment (.10/27)
               - Level 1 PLC-segment (.32/27)
               - Engineering workstation (.64/29)
```

## VLANs en segmentatie

| VLAN | Range | Doel | Firewall-regel |
|---|---|---|---|
| 10 | 10.10.10.0/24 | IT Management | Restrictive, alleen vanuit jumphost |
| 20 | 10.10.20.0/22 | Server-VLAN | Per-server flows |
| 30 | 10.10.30.0/22 | Client-VLAN | Default outbound, restricted inbound |
| 40 | 10.10.40.0/24 | Voice | Microsoft Teams flows |
| 50 | 10.10.50.0/24 | Wi-Fi gasten | Internet-only |
| 51 | 10.10.51.0/24 | Wi-Fi medewerkers | NAC-authentication, dan IT-Core |
| 80 | 10.20.10.0/24 | OT-DMZ | Strikt, deny-by-default |
| 90 | 10.20.20.0/22 | OT-Core | Strikt, deny-by-default |
| 110-140 | 10.20.110-140.0/24 | OT per installatie | Lokaal vrijer, naar Level 3 strikt |

## Specifieke flows (high-level)

### IT -> Azure
- Site-to-Site VPN naar Azure (IPsec, BGP)
- Express Route geplaand voor 2026-Q3

### IT -> OT
- Allowed:
  - PI System (Level 3) -> SQL replica in IT-Core (read-only, voor reporting)
  - Identity replication: Active Directory Domain Controller (legacy on-prem AD) -> 1 OT-domain controller (1-way trust)
- Niet allowed:
  - Direct werkplek -> SCADA / HMI / PLC
  - RDP / SSH van IT naar OT-systemen (alleen via OT-Jumphost)

### Remote support
- Siemens-support: Internet -> Fortinet SSL-VPN -> OT-DMZ Jumphost -> OT-Core
- AquaPolder OT-engineers: Internet -> Fortinet SSL-VPN (eigen profile) -> OT-Core

## Wi-Fi

- Aruba ClearPass NAC
- 4 SSIDs:
  - AquaPolder-Corporate (802.1X met Entra ID)
  - AquaPolder-Guests (captive portal, isolated)
  - AquaPolder-IoT (PSK, beperkte gebruikers)
  - AquaPolder-OT (PSK + MAC-whitelist, alleen op zuiveringsinstallaties)

## Logging

- Fortinet syslog -> Sentinel-workspace (via syslog forwarder in Azure)
- Cisco-switch SNMP -> Solarwinds NPM (intern beheerd)
- Geen flow-logging (NetFlow / sFlow) momenteel - planning 2026-Q2

## Bekende issues / open gaps

- **Single AD-domain-controller** op de OT-zijde (geen redundantie). Single point of failure voor OT-authenticatie.
- **Geen data diode** tussen Level 3 (OT) en Level 4 (IT). De PI System -> SQL replica gaat over een bidirectionele firewall-regel.
- **Wi-Fi-OT** PSK wordt gedeeld door OT-team, geen rotatie. Laatste wijziging 2024.
- **Geen netwerk-mirror op OT-switches** voor passieve monitoring (Claroty/Nozomi/Dragos voorbereiding mist).
- **Firewall change management**: wijzigingen worden gelogd in een SharePoint-list maar geen formeel approval-proces.

## Topologie-diagram

Een visuele weergave in Visio is beschikbaar op `\\fileserver-zwolle\IT\Netwerk\AquaPolder-toplevel-2026Q1.vsdx` (deze gap-analyse heeft alleen toegang tot dit markdown-document; het Visio-diagram is buiten scope).
