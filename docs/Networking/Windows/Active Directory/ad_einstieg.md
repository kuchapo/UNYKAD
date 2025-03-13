# Einstieg in Active Directory (AD)

## Was ist AD?

AD ist ein Verzeichnisdienst, welcher auf Windows Server Betriebssystemen läuft und auf einem lokalen Netzwerk (on-premisis) betrieben wird.

### Protokolle

Folgende Protokolle werden für die Authentifizierung und Autorisierung in AD verwendet:

- Lightweight Directory Access Protocol (LDAP)
- Kerberos
- NTLM

## Hauptfunktionen von AD

- Verwaltung von Benutzern, Gruppen, Computern und anderen Ressourcen innerhalb eines Unternehmensnetzwerks
- ermöglicht die Implementierung von Gruppenrichtlinien zur Verwaltung von Sicherheitseinstellungen und Konfigurationen

## Verwaltung

Durch das Installieren von "Remote Server Administration Tools" (RSAT) auf dem Client kann man das Tool "Active Directory-Benutzer und -Computer" (ADUC) verwenden. Dort hat man eine grafische Oberfläche mit der man einzelne Objekte verwalten kann.

## Redundanz durch Domain Cotroller

In AD gibt es den sogenannten Active Directory Replication Service. Dieser funktioniert im Grunde genommen nach dem Data Replication Service (DRS) Prinzip. Es findet eine Synchronisation von Änderungen zwischen Domain Controllern statt, um sicherzustellen, dass alle Controller konsistente Daten haben. Dabei kommt das Multi-Master-Replikationsmodell zum Einsatz, bei dem jeder Domain Controller Änderungen vornehmen und diese an andere weitergeben kann.

Die Replikation erfolgt auf Attributbasis, was bedeutet, dass nur die geänderten Attribute eines Objekts repliziert werden, nicht das gesamte Objekt. Dies geschieht effizient durch die Verwendung von Update Sequence Numbers (USNs), die sicherstellen, dass nur die neuesten Änderungen repliziert werden.

---

Der Ressourcenzugriff besteht bei AD nur im lokalen Netzwerk (Ausnahme: Zugriff über VPN)!
