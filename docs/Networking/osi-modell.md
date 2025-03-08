# OSI-Modell

## Schicht 1
### Physical Layer (Bitübertragungsschicht)
- Übertragung von Rohdatenbits über ein physisches Medium
- legt elektrische (Ströme, Spannungen), mechanische (Kabel, Stecker), funktionale (Übertragungsrate) und prozedurale (Modulation) Parameter für die physische Verbindung zweier Einheiten fest

## Schicht 2
### Data Link Layer (Sicherungsschicht)
- Sicherstellen einer funktionierenden Verbindung zwischen zwei benachbarten Stationen (im selben LAN)
- Überprüfung auf Fehler
- Daten, die die Schicht 2 verlassen sind fehlerfrei

## Schicht 3
### Network Layer (Vermittlungsschicht)
- Wegfindung in andere Netze Routing
- geroutete Protokolle IPv4, IPv6
- Routingprotokolle RIP, OSPF, IS-IS, IRGP, EIRGP, BGP
- Gerät Router
- Dateneinheit Paket
	
## Schicht 4
### Transport Layer (Transportschicht)
- TCP: virtuelle Verbindung auf- und abbauen, fehlende Daten neu anfordern, Daten in die richtige Reihenfolge bringen, Flusskontrolle (Empfänger steuert wieviel er senden darf -> falls Empfänger überlastet ist), Ports
- UDP: Ports, (Best Effort), unzuverlässig  für sehr kurze Kommunikation (DHCP, DNS), Streams, Dateinheit ist Segment

## Schicht 5
### Session Layer (Kommunikationssteuerungsschicht)
- Auf-, Ab- und Wiederaufbau (nach Verbindungsabbruch) von virtuellen Verbindungen
- Verbindungen
- SyncPoints, Downloadmanager

## Schicht 6
### Presentationlayer (Darstellungsschicht)
- Codierung was bedeuten die Nullen und Einsen
- MP3, JPEG, MPEG, ASCII, PNG,…
- Verschlüsselung AES, RSA,…

## Schicht 7
### Application Layer (Anwendungsschicht)
- stellt der Anwendung Daten so zur Verfügung, wie die sie braucht
- DHCP, DNS, POP3, IMAP4, SMTP, SNMP,…