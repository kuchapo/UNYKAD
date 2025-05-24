# Dynamic Host Configuration Protocol (DHCP)

DHCP dient zur automatischen IP-Konfiguration. Betroffen sind:

- IP
- Subnetzmaske
- Standard Gateway
- DNS-Server
- WINS-Server, NTP-Server

## Vorteile

Der Admin spart sehr viel Arbeit und wenn es einmal richtig eingerichtet ist, macht es keine Fehler. Mobile Geräte bekommen in jedem Netz eine gültige IP und Benutzer müssen keine Ahnung von IPs haben.

## Funktion

Client schickt ein DHCP Discover (Broadcast 255.255.255.255) an den DHCP Server. Der DHCP-Server schickt als Antwort einen DHCP Offer (als Unicast oder Broadcast). Dann schickt der Client ein DHCP Request als Broadcast (immer!). Der DHCP-Server schickt dann ein DHCP Acknowledge (als Unicast oder Broadcast).

### Leasetime

Die Zeitspanne, für die die IP-Adresse vergeben wird. Sobald die Hälfte der Lease-Zeit verstrichen ist, sendet der Client eine erneute Anfrage. Der DHCP-Server bestätigt diese mit einem Acknowledge und verlängert dadurch die Leasetime kontinuierlich. Sollte der Server bei 87,5 % der Lease-Zeit nicht antworten, wird letztmalig eine Broadcast-Anfrage gesendet. Bleibt auch diese unbeantwortet, verwirft der Client die aktuelle IP-Konfiguration.

### Adressvergabe

- Manuelle Zuordnung (statisches DHCP): Admin legt eine Liste mit MAC und IP an und der Client bekommt immer die gleiche IP z. B. für Drucker, Scanner, Server,...
- Automatische Zuordnung: wie manuell, nur liegt der DHCP Server die Liste an
- Dynamische Zuordnung: Client bekommt jedes Mal irgend eine freie IP aus dem Pool

#### Adressvergabe über mehrere Netze

![DHCP](assets/dhcp.drawio.svg)

Problem:  
- Router leiten niemals Broadcasts weiter! DHCP Discover aus dem linken Netz kommt nicht beim Server an

Lösung:  
- Relay Agent: Routerschnittstelle macht aus dem Broadcast einen Unicast gezielt an die IP des DHCP Servers. DHCP Server braucht für jedes Netz einen eigenen Pool

## Angriffe auf DHCP

DHCP Starvation: DHCP Server wird mit Anfragen überhäuft bis er keine IPs mehr hat (DoS Attack), dann springt ein Rogue Server ein und vergibt IPs mit z. B. falschem Standard Gateway oder DNS Server (DNS Spoofing). -> Man in the middle Attack

Abhilfe: DHCP Snooping: Switch lässt DHCP Offers und Acknowledge nur an bestimmten Ports zu (Port Security)
