# Dynamic Host Configuration Protocol

DHCP dient zur auomatischen IP-Konfiguration.

- IP
- Subnetzmaske
- Standard Gateway
- DNS
- WINS, NTP

## Vorteile

Admin spart sehr viel Arbeit, wenn es einmal richtig eingerichtet ist, macht es keine Fehler. Mobile Geräte bekommen in jedem Netz eine gültige IP. Benutzer müssen keine Ahnung von IPs haben.

## Funktion

Client schickt ein DHCP Discover (Broadcast 255.255.255.255) an den DHCP Server. Der DHCP Server schickt als Antwort einen DHCP Offer (als Unicast oder Broadcast). Dann schickt der Client ein DHCP Request als Broadcast (immer!). Der DHCP Server schickt dann ein DHCP Acknowledge (als Unicast oder Broadcast).

### Leasetime

Zeit, für die die IP vergeben wird. Wenn sie zur Hälfte abgelaufen wird, dann wird der Client wieder einen Request schicken und der DHCP Server bestätigt sie mit einem Acknowledge und verlängert die Leasetime so immer wieder. Wenn der Server nicht antwortet bei 87,5%, dann schmeißt er die IP-Konfiguration weg.

### Adressvergabe

- Manuelle Zuordnung (statisches DHCP): Admin legt eine Liste mit MAC und IP an und der Client bekommt immer die gleiche IP z. B. für Drucker, Scanner, Server,...
- Automatische Zuordnung: wie manuell, nur liegt der DHCP Server die Liste an
- Dynamische Zuordnung: Client bekommt jedes Mal irgend eine freie IP aus dem Pool

#### Adressvergabe über mehrere Netze

Problem:
- Router leiten niemals Broadcasts weiter! DHCP Discover aus dem linken Netz kommt nicht beim Server an
Lösung:
- Relay Agent: Routerschnittstelle macht aus dem Broadcast einen Unicast gezielt an die IP des DHCP Servers. DHCP Server braucht für jedes Netz einen eigenen Pool

## Angriffe auf DHCP

DHCP Starvation: DHCP Server wird mit Anfragen überhäuft bis er keine IPs mehr hat (DoS Attack), dann springt ein Rogue Server ein und vergibt IPs mit z. B. falschem Standard Gateway oder DNS Server (DNS Spoofing). -> Man in the middle Attack
