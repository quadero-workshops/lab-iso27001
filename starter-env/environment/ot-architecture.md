# AquaPolder N.V. - OT-architectuur

**Versie**: 2026-Q1
**Bron**: Robert Koster (Manager OT-engineering) + technische documentatie Siemens
**Markering**: VERTROUWELIJK

## Purdue Model toepassing

AquaPolder's OT-architectuur volgt het Purdue-referentiemodel met enkele afwijkingen.

```
Level 5 - Enterprise Zone (M365, kantoor)
    |
    |  -- Geen DMZ tussen 4 en 3.5 in de strikte zin --
    |  (Fortinet OT-Border firewall doet de scheiding)
    |
Level 3.5 - DMZ / OT-grens
    |
    +-- OT-Jumphost (Windows Server 2019, 1 stuks, gedeeld voor alle installaties)
    +-- Reverse proxy voor Siemens-remote-support
    +-- AntiVirus update relay (richting AV-clients op Level 3)
    |
Level 3 - Site operations (OT-Core, in Zwolle DC)
    |
    +-- 2x PI System server (HA-pair) (per Q1 2026)
    +-- 1x OT Domain Controller (single, geen replica)
    +-- 1x WSUS-server voor Windows OT-systemen
    +-- Backup-server (Veeam OT, separate van IT-Veeam)
    +-- Engineering admin-server (TIA Portal central library)
    |
Level 2 - Supervisory (per installatie)
    |
    +-- 2x WinCC OA server (active-passive, fysieke Siemens IPC)
    +-- 1-2x Engineering workstation (Windows 10 LTSC 2019, niet Intune-managed)
    +-- 1-2x HMI-display (Siemens TP1500 Comfort)
    |
Level 1 - Control (per installatie)
    |
    +-- 2-6x PLC per installatie (Siemens S7-1500, sommige S7-300)
    +-- Diverse Profinet remote IO-modules
    |
Level 0 - Process (per installatie)
    |
    +-- Sensors: druk, debiet, troebelheid, pH, vrij-chloor, leidingvermogen
    +-- Actuators: pompen, kleppen, doseerunits
```

## Per zuiveringsinstallatie

### Zwolle-noord (hoofdinstallatie)
- 4x S7-1500 PLC (firmware V2.9.x)
- 2x S7-300 PLC (firmware V3.3, einde levensduur 2027)
- 12 HMI's incl. control-room
- WinCC OA versie 3.18 P010
- 2 engineering workstations

### Kampen
- 3x S7-1500 PLC
- 1x S7-300 PLC (uitfasering 2026)
- 8 HMI's
- WinCC OA versie 3.18 P010
- 1 engineering workstation

### Hardenberg
- 3x S7-1500 PLC
- 0x S7-300 (uitgefaseerd 2025)
- 6 HMI's
- WinCC OA versie 3.18 P011
- 1 engineering workstation

### Wijhe
- 2x S7-1500 PLC
- 2x S7-300 PLC (uitfasering 2026)
- 4 HMI's
- WinCC OA versie 3.18 P010
- 1 engineering workstation

## ICS-protocollen

| Protocol | Waar gebruikt | Encryptie | Authentication |
|---|---|---|---|
| PROFINET-RT | Level 0 <-> Level 1 (sensor/actuator <-> PLC) | Nee | Nee (network-only) |
| S7comm | TIA Portal <-> PLC, WinCC OA <-> PLC | Optioneel (S7comm-plus, niet altijd ingeschakeld) | Wachtwoord per access-level op PLC |
| OPC UA | WinCC OA <-> PI System, WinCC OA <-> Reporting | Ja (TLS, server-cert verplicht) | Client certificate |
| HTTPS | Engineering naar WinCC OA web-UI | Ja | Forms-auth, lokale WinCC-users |
| RDP | OT-jumphost naar engineering workstation | Ja (NLA) | AD-account in OT-domain |

## Authenticatie en toegang in OT

### OT-Domain (Active Directory)
- Eigen domain (`ot.aquapolder.local`), geen trust met IT-domain
- ~40 user-accounts (12 OT-engineers + Siemens-support + service-accounts)
- Geen MFA op het OT-domain (intern), alleen op de Fortinet-VPN ervoor

### WinCC OA users
- Lokale user-store binnen WinCC OA, niet gefedereerd
- 3 roles per installatie: Operator (read-only), Engineer (write), Admin
- Gedeelde Operator-accounts per shift (legacy reden: shift-overdracht praktijk)

### PLC access-levels (S7-1500 met TIA Portal)
- Level 1 - Full access: wachtwoord uniek per installatie (4 wachtwoorden totaal)
- Level 2 - Read access: wachtwoord uniek per installatie
- Level 3 - HMI access: hard-coded in WinCC OA config

### PLC access-levels (S7-300 legacy)
- Geen niveau-bescherming. Iedereen met TIA Portal en netwerk-toegang kan herprogrammeren.

## Backups en herstel

### PLC-projecten
- Lokale backup per installatie op de engineering workstation
- Centrale backup: handmatig, na elke deploy, naar `\\ot-fileserver\plc-projects\<installatie>\<datum>`
- Geen versie-beheer (geen Git of vergelijkbaar)
- Siemens neemt halfjaarlijks remote backup (in Siemens-infrastructuur)

### WinCC OA-configuraties
- Veeam OT backup, dagelijks, incremental
- Bewaar-termijn: 4 weken local, 6 maanden offsite (Apeldoorn)

### Historian (PI System)
- Veeam OT backup, dagelijks
- Process-data zelf: PI-archive houdt 10 jaar online + 30 jaar offline storage

### Recovery-tests
- Volume-level restore: getest 2024 + 2025
- Full PLC-restore-end-to-end: nooit getest in productieomgeving
- BCM-plan beschrijft een 4-uurs herstel-target voor SCADA, een 24-uurs RTO voor productie-impact

## Patch management OT

| Component | Cyclus | Vendor-betrokken | Notities |
|---|---|---|---|
| WinCC OA | 2x/jaar | Ja (Siemens) | Patch-window 's nachts, downtime ~2 uur |
| S7-1500 firmware | 1x/2-3 jaar | Ja (Siemens) | Coordinated, per installatie |
| S7-300 firmware | Niet meer | n.v.t. | Einde levensduur |
| HMI-firmware | 1x/2 jaar | Ja (Siemens) | Per installatie |
| Engineering workstation OS | Kwartaal | Nee (intern OT-team) | Handmatig, Windows Update Catalog |
| OT-domain controller | Maandelijks | Nee | Microsoft cumulative updates |
| PI System | 1x/jaar | Ja (PI-partner) | |

## Bekende kwetsbaarheden / gaps

- **PLC firmware** is niet altijd up-to-date met Siemens-CVE-fixes (laatste sweep was 2024). Geen volwassen vulnerability management proces.
- **Engineering workstations** zijn Windows 10 LTSC 2019, supported tot oktober 2029. Niet onder Intune.
- **OT-domain** heeft single DC zonder replica.
- **S7-300 PLC's** zonder toegangs-bescherming, op netwerk-laag wel beperkt maar niet bullet-proof.
- **Gedeeld Siemens-support-account** op OT-jumphost (besproken in interview-05).
- **Geen passieve OT-monitoring** (geen Claroty / Nozomi / Dragos / vergelijkbaar).
- **Geen application whitelisting** op engineering workstations.
- **Geen formele change control op PLC-programmering** (peer review ontbreekt).
- **Backup van PLC-configuratie** is handmatig en niet versie-beheerd.
- **Historian** (PI System) is een single point of failure voor security-logs - geen replicatie van PI naar IT-side voor analyse.
