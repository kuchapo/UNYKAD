# Neighbor Discovery Protocol

## Multicast-Funktionsweise bei IPv6

Im Gegensatz zu einem Layer-2-Broadcast (Ziel-MAC: nur `F`s) verwendet IPv6 spezielle **Multicast-MAC-Adressen**, die aus der IPv6-Multicast-Adresse abgeleitet werden und mit `33:33:` beginnen.

Wenn ein Gerät an einer Multicast-Gruppe teilnehmen möchte, meldet es das über das **MLD-Protokoll** (Multicast Listener Discovery). Ein Switch, der **MLD-Snooping** unterstützt, speichert daraufhin in einer internen Tabelle, über welchen Port welches Gerät zu welcher Multicast-Gruppe gehört.

So kann der Switch Multicast-Verkehr gezielt an die richtigen Ports weiterleiten – ohne Flooding. Ein Switch kann gleichzeitig mehrere Multicast-Gruppen verwalten.

---

## Router Solicitation (RS)

Wird von den Endgeräten an den Router verschickt und enthält:

- Die eigene Link-Local-Adresse (Absender)
	- falls noch keine vorhanden ist, wird stattdessen die **unspezifizierte Adresse** `::` verwendet
		- Router antwortet dann per Multicast statt gezieltem Unicast
- Eine Anfrage nach einem Router Advertisement (RA)

---

## Router Advertisement (RA)

Wird vom Router an die Endgeräte verschickt und enthält:

- Router MAC Address
- Network Prefix/mask
- Client IPv6 address options

!!! info
	Ein RA kann auch ohne RS erfolgen. Der Router schickt RA-Nachrichten an alle Geräte periodisch (z. B. alle 200 Sekunden), nach dem Hochfahren und nach einem Interface-Status-Wechsel.

---

## Neighbor Advertisement (NA)

Wird von jedem Gerät, welches das RA erhalten hat, jeweils an alle anderen IPv6 Geräte im Netzwerk verschickt (Router eingeschlossen) und enthält:

- Client MAC Address
- Client IPv6 Address
- Options

Diese Neighbor Advertisements werden dann auf den Geräten in den IPv6 Neighbor Tables gespeichert. Diese laufen allerdings wie bei ARP nach einer gewissen Zeit ab.

---

## Neighbor Solicitation (NS)

Da die Einträge in Neighbor Table ablaufen, werden über Multicast mit Neighbor Solicitation neue `NA`s angefragt.

---

## Wichtige Befehle

### NDP-Tabelle anzeigen lassen

```cli
netsh int ipv6 show neighbor
```

### NDP-Tabelle löschen

```cli
netsh int ipv6 delete neighbors interface="<YourInterface>"
```
