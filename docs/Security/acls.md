# ACLs – Access Control Lists

## ACLs im Vergleich zu Firewalls

| Merkmal             | **ACL (Access Control List)**                                                       | **Firewall**                                                                            |
| ------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Grundprinzip**    | Liste von Regeln: erlaubt oder blockiert Traffic nach Quelle, Ziel, Port, Protokoll | Nutzt ACLs als Basis, erweitert um viele zusätzliche Sicherheitsfunktionen              |
| **Ebene (OSI)**     | Meist **Layer 3/4** (IP, TCP/UDP)                                                   | Bis **Layer 7** (Anwendungsebene: HTTP, DNS, VoIP etc.)                                 |
| **Stateful?**       | Nein (stateless, prüft jedes Paket einzeln)                                         | Ja (merkt sich Sitzungen, erlaubt Antwortpakete automatisch)                            |
| **Funktionsumfang** | Nur Filterung von Paketen                                                           | Filterung + NAT, VPN, IDS/IPS, Application Control, Webfilter, Antivirus usw.           |
| **Komplexität**     | Einfach (nur Bedingungen)                                                           | Komplex (Policies, Profile, Benutzerrechte, Protokollanalysen)                          |
| **Beispiel**        | `permit tcp 192.168.1.0/24 any eq 80`                                               | „Erlaube HTTP von internen Clients ins Internet, scanne Traffic auf Viren, logge alles“ |

## Regelwerke

Ein **Regelwerk** ist die **Gesamtheit von Regeln**, die den Datenverkehr für eine bestimmte Richtung/Schnittstelle/Protokoll betreffen.

- Ein **Regelwerk** ist also nicht nur eine einzelne Regel, sondern eine **Liste von Bedingungen** (Access Control List / ACL, Firewall-Regeln).
- Darin wird beschrieben: **welcher Datenverkehr erlaubt oder blockiert wird**.
- ACE (Access List Entry) bezeichnet eine einzige Regel in einer ACL

Pro Schnittstelle hat man 4 mögliche Regelwerke (ACLs):

- IPv4 rein (z. B. „allow Port 80, deny alles andere“)
- IPv4 raus
- IPv6 rein
- IPv6 raus

→ 4 **Regelwerke**, weil für jede Richtung und Protokollfamilie eine **eigene Liste** gilt.

## Whitelist

```cli
permit tcp 192.168.1.0/24 any eq 80
deny ip any any
```
→ Nur HTTP-Traffic aus dem Netz 192.168.1.0/24 ist erlaubt.
→ Professionelle Firewalls sind meistens Whitelists (Default-Deny), Heimrouter hingegen eher Blacklists.

## Blacklist

```cli
deny tcp any 10.0.0.5 eq 23
permit ip any any
```
→ Alles ist erlaubt, außer Telnet zu 10.0.0.5.

## Geräte mit ACL-Unterstützung

- Router
- Layer-3-Switches
- Firewalls
- Wireless-Controller / Access Points
- Endgeräte / Betriebssysteme
