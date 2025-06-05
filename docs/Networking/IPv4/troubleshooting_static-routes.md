# IPv4 Troubleshooting - Statische Routen

## Beispiel

![routing_troubleshooting](assets/routing_troubleshooting.drawio.svg)

**Situation**: `PC A` links kann `PC B` rechts nicht pingen.

Um den Fehler einzugrenzen, pingt man von `PC A` schrittweise alle erreichbaren IPs durch – beginnend beim lokalen Gateway bis zur letzten erreichbaren Instanz. So lässt sich bestimmen, ab welchem Hop der Ping fehlschlägt.

```cli
ping 10.0.0.1

ping 172.16.0.1

! Ping schlägt hier fehl:
ping 172.16.0.2
```

Wenn alle Interfaces erreichbar sind, prüft man von **Router A** aus, ob ein Ping zu `172.16.0.2` möglich ist. Dabei ist entscheidend, dass man beim `ping`-Befehl gezielt die **Quell-IP** (source) angibt – denn Router A hat mehrere Interfaces und somit mehrere mögliche Quelladressen.

```cli
! You can only use direct interfaces from the router as source IP

ping 192.168.10.8 source 172.16.0.1

ping 192.168.10.8 source 10.0.0.1
```

Wenn der erste Ping (über `172.16.0.1`) funktioniert, aber der zweite (über `10.0.0.1`) fehlschlägt, liegt der Fehler sehr wahrscheinlich an einer fehlenden statischen Route auf Router B zurück ins 10.0.0.0/24-Netz.

### Erwartete Routingtabellen

Mit dem folgenden Befehl kann man sich die Routing Tabelle anzeigen lassen:

```cli
show ip route
```

#### Router A

`S 192.168.10.0/24 via 172.16.0.2`  
`C 172.16.0.0 FastEthernet0/1`  
`C 10.0.0.0 FastEthernet0/0`

#### Router B

`S 10.0.0.0/24 via 172.16.0.1`  
`C 172.16.0.0 FastEthernet0/0`  
`C 192.168.10.0 FastEthernet0/1`

### Alternative Zugriffsmöglichkeit via SSH

Wenn du keinen direkten Zugriff auf Router B hast und **SSH von PC A aus nicht funktioniert**, kannst du vom Router A aus dennoch eine SSH-Verbindung zu Router B aufbauen:

```cli
ssh -l <Username> 172.16.0.2
! Enter your Password
```

!!! info
	Der Grund, warum du dich von Router A per SSH mit `172.16.0.2` verbinden kannst – obwohl du vom PC aus nur über das Interface `10.0.0.1` mit dem Router verbunden bist – liegt darin, dass Router A über sein Interface `172.16.0.1` eine **direkte Route** zu `172.16.0.2` hat. Die SSH-Verbindung läuft somit vollständig innerhalb des 172.16.0.0/30-Netzwerks und ist vom lokalen PC-Netz (10.0.0.0/24) unabhängig.

### Optional: Live-Überwachung durch Ping-Anfragen

Bevor man die fehlende Route in Router B nun ergänzt, hat man zusätzlich die Möglichkeit kontinuierliche Ping-Anfragen von PC A zu senden, um parallel zu beobachten, ob die gesendeten Pakete ankommen werden oder nicht:

```powershell
ping 192.168.10.8 -t
```

### Fehlende Routen auf Router B hinzufügen

Fehlende Route einrichten:

```cli
! Do Routing in the 10.0.0.0/24 Network via Router A
ip route 10.0.0.0 255.255.255.0 172.16.0.1
```

### Falsche Routen hinzugefügt

Beispiel für falsch konfigurierte Routen auf Router B:

```cli
ip route 10.0.0.0 255.255.255.0 172.15.0.1
ip route 10.0.0.0 255.255.255.0 172.18.0.5
ip route 10.0.0.0 255.255.255.0 172.118.0.1
ip route 10.0.0.0 255.255.255.0 10.0.0.1
ip route 10.0.0.0 255.255.255.0 192.168.10.8
ip route 10.0.0.0 255.255.255.0 172.16.0.1
```

Falsch konfigurierte Routen anzeigen lassen:

```cli
show run | i ip route
```

!!! info
	Die Next-Hop-Adresse kann nicht auf dem Router liegen!

Wenn man den Befehl `show ip route` ausführt, dann sieht man in der Routing-Tabelle alle Einträge, welche auch wirklich eine statische Route zur Next-Hop-Adresse haben.

Trotz dessen, dass die richtige Route mit der Next-Hop-Adresse `172.16.0.1` auch mit eingetragen wurde, funktioniert der Ping weiterhin nicht. Alle falsch konfigurierten Routen kann man mit dem `no`-Befehl löschen, so dass nur die eine richtige Route übrig bleibt.

Alternativ besteht die Möglichkeit statt `172.16.0.1` einzutragen das Interface `f0/0` zu benutzen:

```cli
ip route 10.0.0.0 255.255.255.0 f0/0
```

!!! info
	Beide Routen lassen sich nicht gleichzeitig eintragen und benutzen in der Routing-Tabelle!

### Traceroute

Mit Traceroute bzw. Tracert hat man die Möglichkeit den kompletten Pfad zur Ziel-IP anzeigen zu lassen:

=== "Windows"
	```powershell
	tracert -d 192.168.10.8
	```
	
=== "Linux"
	```bash
	traceroute -n 192.168.10.8
	```
