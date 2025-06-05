# Address Resolution Protocol (ARP)

ARP (Address Resolution Protocol) ist ein Netzwerkprotokoll, das verwendet wird, um die MAC-Adresse (Media Access Control) eines Geräts anhand seiner IP-Adresse zu ermitteln. Es ist ein entscheidender Bestandteil der Kommunikation in lokalen Netzwerken (LAN).

## Funktionsweise

1. **Anfrage:** Ein Gerät sendet eine ARP-Anfrage (als Broadcast), um die MAC-Adresse eines anderen Geräts mit einer bekannten IP-Adresse zu finden.
2. **Antwort:** Das Zielgerät antwortet mit seiner MAC-Adresse.
3. **Kommunikation:** Nach der Zuordnung können die Geräte miteinander kommunizieren.

## Wichtige Punkte

- ARP arbeitet nur innerhalb eines Netzwerks (also innerhalb eines Subnetzes) und funktioniert nicht über Router hinweg.
- Jedes Gerät besitzt eine ARP-Tabelle (ARP Cache), die die Zuordnung von IP-Adressen zu MAC-Adressen speichert.
- Wenn die MAC-Adresse dem sendenden Gerät nicht bekannt ist, wird eine ARP-Anfrage (Broadcast) gesendet – unabhängig davon, ob der Switch die MAC-Adresse kennt oder nicht.
- ARP ist ein Layer-2-Protokoll, aber es wird durch ein Layer-3-Bedürfnis ausgelöst:
    - „Ich habe eine IP – aber keine MAC.“
    - Deshalb nennt man es auch ein "Übersetzerprotokoll" zwischen Layer 3 (IP) und Layer 2 (MAC).
- Alle 90 Sekunden verschwindet ein ARP-Eintrag.
    - Grund: Eine IP-Adresse kann einer anderen MAC-Adresse zugewiesen werden.

Zusatz:

- Wenn das Ziel in einem anderen Netzwerk liegt, wird ARP nicht für das Ziel verwendet, sondern für das Standardgateway (Router-IP), da das Gerät den nächsten Hop erreichen muss. 

## Beispiel für eine ARP-Anfrage

Ein Computer möchte Daten an die IP-Adresse 192.168.1.10 senden und fragt:  
„Wer hat die IP-Adresse 192.168.1.10? Bitte melde deine MAC-Adresse.“

## Wichtige ARP Befehle

ARP Cache auf dem Rechner aufrufen:

=== "Windows"
    ```sh
    arp -a
    ```

=== "Linux"
    ```bash
    ip neigh show
    ```

Gesamten ARP Cache auf dem Rechner löschen:

=== "Windows"
    ```sh
    arp -d *
    ```

=== "Linux"
    ```bash
    ip -s -s neigh flush all
    ```
