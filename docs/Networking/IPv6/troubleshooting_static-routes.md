# IPv6 Troubleshooting - Statische Routen

IPv6 Troubleshooting funktioniert größtenteils genauso wie IPv4 Troubleshooting. Die Konzepte lassen sich also übertragen. Die folgenden Fälle betreffen allerdings explizit nur das IPv6 Routing.

## Beispiel

![routing_troubleshooting](ipv4_ospf.drawio.svg)

| Router A<br>Routing Tabelle                                      | Router B<br>Routing Tabelle                                          |
| ---------------------------------------------------------------- | -------------------------------------------------------------------- |
| C F0/0 2001:DB8:10:A::/64                                        | C F0/0 2001:DB8:10:B::/64                                            |
| C F0/1 2001:DB8:10:B::/64                                        | C F0/1 2001:DB8:10:C::/64                                            |
| S 2001:DB8:10:C::/64<br>                    via 2001:DB8:10:B::2 | S 2001:DB8:10:A::/64<br>                    via 2001:DB8:10:B::1<br> |

### IPv6 Router Interface Fehlkonfiguration

Wenn man IPv6-Adressen auf einem Interface konfiguriert und versehentlich mehrere IPv6 Adressen eingetragen hat, dann kann das unter Umständen zu Problemen führen.

Während man bei IPv4 mit `ìp address <IPv4-Address>` auf dem Interface die vorhandene IP-Adresse überschreibt, fügt man bei IPv6 mit `ipv6 address <IPv6-Address` eine neue IPv6 Adresse zum Interface hinzu.

Aus diesem Grund sollte man mit dem folgenden Befehl alle unerwünschten IPv6-Adressen auch löschen:

```cli
no ipv6 address <IPv6-Address>
```

Um die IPv6-Adressen auch vom Client weg zu kriegen, sollte man das Interface unter Netzwerkverbindungen (unter Windows) ausschalten und wieder einschalten.

### IPv6 Unicast-Routing Command

Situation: `ipv6 unicast-routing` wurde nicht aktiviert

Wenn man ohne Probleme IPv6-Adressen konfigurieren kann und der Ping zum `PC B` auch funktioniert, der Traffic aber trotzdem nicht durchkommt, dann sollte man schauen, ob beim pingen die Fehlermeldung `PING: transmit failed. General failure.` angezeigt wird.

Diese Fehlermeldung ist ein Indiz dafür, dass etwas mit dem Default Gateway nicht stimmt. Ob das auch tatsächlich der Fall ist, kann man mit `ìpconfig /all` in der PowerShell überprüfen, indem man schaut, ob das Default Gateway eingetragen ist.

Sobald man bei sich lokal auf dem Rechner wieder das richtige Default Gateway setzt, welches auch auf dem Router Interface eingetragen ist, wird beim PING diesmal ein `Request timed out.` kommen. Erst wenn man IPv6-Routing mit dem Befehl `ipv6 unicast-routing` wieder einschaltet, wird es wie gewohnt funktionieren.

### Link-Local vs. Global Unicast Next Hop Adresse

Es gibt verschiedene Meinungen zu dem Thema welche Next Hop Adresse man benutzen sollte. In den meisten Fällen gibt es keinen signifikaten Unterschied zwischen der Link-Local vs. Global Unicast Next Hop Adresse beim Einsatz. Man sollte bei der Erstellung der Link-Local-Adresse nur darauf achten, dass das Exit-Interface auch mit angegeben wird.

Ebenso besteht die Möglichkeit beide Konzepte zu gleichzeitig zu benutzen. Also wenn man zum Beispiel auf Router A das Ziel-Netz `2001:db8:10:c::/64` einträgt und von Router B die Link-Local-Adresse.
## Wichtige Befehle

Statische Route auf dem Router hinzufügen:

```cli
ipv6 route <Destination Network with Prefix> <Next Hop ID>
```

---

Link-Local-Adresse auf dem Router hinzufügen:

```cli
ipv6 route <Destination IPv6> <Exit-Interface> <Next-Hop-Link-Local-Adresse>
```

z. B. wenn auf Router A konfiguriert wird und Router B die Link-Local-Adrese `FE80:21D:E6FF:FE15:CB00`:

```cli
ipv6 route 2001:db8:10:c::/64 f0/1 fe80:21d:e6ff:fe15:cb00
```

!!! info
	Bei Link-Local-Adressen ist immer die Angabe des Exit-Interfaces erforderlich, da diese Adressen nur innerhalb des lokalen Netzsegments gültig sind. Ohne Interface wüsste der Router nicht, über welche Schnittstelle der nächste Hop erreichbar ist.

---

IPv6-Routing-Tabelle überprüfen:

```cli
show ipv6 route
```

---

IPv6-Adresse hinzufügen:

```cli
ipv6 address <IPv6 Address with Prefix>
```

---

Zeigt alle IPv6-Adressen auf einem Interface an:

```cli
show run int <Name of Interface>
```

---

Spezifisch Einträge für IPv6 einsehen:

```cli
show run | i ipv6 route
```
