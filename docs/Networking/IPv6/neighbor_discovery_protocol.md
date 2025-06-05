# IPv6 Neighbor Discovery Protocol

## Multicast Funktionsweise

Während bei Broadcast auf Layer 2 die MAC-Adresse nur `F`'s beinhaltet, hat ein Multicast eine sehr spezifische MAC-Adresse. Wenn ein Gerät, welcher den Multicast erhalten hat, sich bei der Multicast-Gruppe beteiligen möchte, dann wird diese Nachricht zuerst oft an den Switch geschickt, wo dieser dann sich die MAC-Adressen der Geräte merkt. Der Switch, sofern dieser IGMP unterstützt, wird dann in einer IGMP-Tabelle die Ports, welche sich an der Multicast-Gruppe beteiligen möchten, speichern. Dabei kann der Switch mehrere Multicast-Gruppen parallel am laufen halten.

## Router Advertisement (RA)

Wird vom Router an die Endgeräte verschickt und enthält:

- Router MAC Address
- Network Prefix/mask
- Client IPv6 address options

## Neighbor Advertisement (NA)

Wird von jedem Gerät, welches das RA erhalten hat, jeweils an alle anderen IPv6 Geräte im Netzwerk verschickt (Router eingeschlossen) und enthält:

- Client MAC Address
- Client IPv6 Address
- Options

Diese Neighbor Advertisements werden dann auf den Geräten in den IPv6 Neighbor Tables gespeichert. Diese laufen allerdings wie bei ARP nach einer gewissen Zeit ab.

## Neighbor Solicitation (NS)

Da die Einträge in Neighbor Table ablaufen, werden über Multicast mit Neighbor Solicitation neue NAs angefragt.