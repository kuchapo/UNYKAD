# Address Resolution Protocol (ARP)

ARP (Address Resolution Protocol) ist ein Netzwerkprotokoll, das verwendet wird, um die MAC-Adresse (Media Access Control) eines Geräts anhand seiner IP-Adresse zu ermitteln. Es ist ein entscheidender Bestandteil der Kommunikation in lokalen Netzwerken (LAN).

## Funktionsweise

1. **Anfrage:** Ein Gerät sendet eine ARP-Anfrage, um die MAC-Adresse eines anderen Geräts mit einer bekannten IP-Adresse zu finden.
2. **Antwort:** Das Zielgerät antwortet mit seiner MAC-Adresse.
3. **Kommunikation:** Nach der Zuordnung können die Geräte miteinander kommunizieren.

## Wichtige Punkte

- ARP arbeitet nur innerhalb eines Netzwerks und ist nicht zwischen verschiedenen Netzwerken aktiv.
- Es existiert eine **ARP-Tabelle** auf jedem Gerät, die die Zuordnungen von IP- zu MAC-Adressen speichert.

## Beispiel für eine ARP-Anfrage

Ein Computer möchte Daten an die IP-Adresse 192.168.1.10 senden und fragt:  
„Wer hat die IP-Adresse 192.168.1.10? Bitte melde deine MAC-Adresse.“

---

Mit ARP wird sichergestellt, dass Geräte in einem lokalen Netzwerk Daten effizient und korrekt übertragen können.
