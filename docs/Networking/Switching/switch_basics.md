# Switch Basics

- Switches funktionieren auf Layer 2
- An jedem physischen Port besitzt ein Switch ein ASIC
- ASIC:
    - Hardwarebasierte Logikschaltung an jedem Port
    - Liest den Frame-Header (insbesondere die Quell-MAC-Adresse)
    - Lernt und speichert MAC-Adressen in der MAC Address Table
    - Nicht programmierbar wie ein Mikrocontroller
- Wenn zwei Geräte miteinander kommunizieren, dann bekommen es andere Geräte nicht mit
- Wenn ein Gerät für 300 Sekunden keine Daten in den Switch reinschickt, verwift der Switch die MAC-Adresse des Geräts
- Wenn ein Switch die MAC-Addresse des Zielgeräts nicht kennt, dann schickt er ein Broadcast (an alle Ports, außer dem Eingangsport) -> Flooding
    - Zielgerät schickt dann antwort an den Sender, dass dieser bereit ist zu kommunizieren
    - ASIC speichert MAC-Adresse des Zielgeräts
- L3-Switches funktionieren auf Layer 2 & 3
- L3-Switches werden häufig als Core-Switches verwendet

## Modi

- User EXEC Mode (`Router>`)
- Privileged EXEC Mode (`Router#`)
- Global Configuration Mode (`Router(config)#`)

## Speicherplätze

Wie beim Router, nur mit dem Unterschied, dass im Flash zusätzlich die Datei `config.text` existiert.  
Diese enthält die `startup-config`, die beim Booten in den NVRAM geladen wird.

`config.text` im Flash = gespeicherte Startup-Konfiguration

## Konfiguration

- Via Patchkabel zwischen Ethernet Port (PC) und Switch Port (Switch) verbinden
- Via Rollover-Kabel zwischen RS-232 Port (PC) und Console Port (Switch) verbinden
- PuTTY öffnen und verbinden
- 'no' eingeben

### Wie beim Router

- Hostnamen erstellen
- Banner erstellen
- SSH aktivieren

### IP-Adressen einrichten

IPs werden bei Switches nicht geroutet, deshalb erstellt man sogenannte VLANs, also virtuelle Interfaces.

- VLAN-Interfaces dienen nur der Verwaltung (Management)
- Ein Layer-2-Switch routet nicht zwischen VLANs – das geht nur mit einem L3-Switch oder Router

#### IPv4

```cli
interface vlan 1
ip address <IP-Address> <Subnet-Mask>
no shutdown
```

### Passwortwiederherstellung

- in den ROMMON-Modus booten
    - Switch ausschalten oder das Kabel raus ziehen
    - Mode-Taste gedrückt halten und Kabel wieder einstecken bzw. Switch wieder einschalten

```cli
flash_init
dir flash:
rename flash:config.txt flash:config.bak
boot

! Press Return
! Enter no
enable
show flash
rename flash:config.bak flash:config.text
! Press Return
show startup-config
copy startup-config running-config
configure terminal
enable secret <YourPassword>
copy running-config startup-config
! Press Enter
```

### IOS-Upgrade

```cli
show version
show flash
! The d in drwx stands for directory
! Look in FLASH for the directory that looks like an IOS
show flash:/<NameOfTheDirectory>
! Start TFTP Server in the right directory with the new IOS File in it
ping <TFTP Server IP>
copy tftp flash
! Enter TFTP Server IP
! Enter full name of the new IOS File with the extension
! Press Enter
show flash
configure terminal
boot system flash:/<New IOS File with Extension>
exit
copy running-config startup-config
! Überprüft auf Integrität, indem man diesen Wert und den vom Hersteller gegebenen Wert miteinander vergleicht
! Beide Werte müssen übereinstimmen
verify /md5 flash:/<New IOS File with Extension>
reload
show version
```

## MAC Address Table

![switch_beispiel](assets/switch_beispiel.drawio.svg)

Wenn man die MAC Address Table bzw. CAM Table von S1 anschaut, dann sieht man alle MAC-Adressen (sofern alle Geräte innerhalb von 300 Sekunden Traffic gesendet haben).

Zeigt die MAC Address Table an:

```cli
show mac address-table dynamic
```

Wenn ein physischer PC über einen Netzwerkadapter mit einem Switch verbunden ist, erscheint in der MAC-Adresstabelle des Switches in der Regel nur die MAC-Adresse dieses Adapters.

Wenn jedoch eine oder mehrere virtuelle Maschinen (VMs) auf dem PC laufen und deren virtuelle Netzwerkkarten über denselben physischen Adapter (z. B. im Bridged Mode) mit dem Netzwerk kommunizieren, können zusätzlich auch deren MAC-Adressen in der MAC-Tabelle erscheinen – nämlich die der virtuellen Netzwerkkarten.

Der Switch lernt dabei alle MAC-Adressen, von denen er aktiven Traffic empfängt – auch wenn dieser über denselben physikalischen Port kommt.

Die folgende MAC-Adressentabelle zeigt, welche Adressen vom S1-Switch gelernt wurden – sowohl von physischen Hosts mit USB-Netzwerkkarten als auch von virtuellen Maschinen.  
Letztere besitzen eigene virtuelle Netzwerkkarten, deren Datenverkehr über die physische Netzwerkkarte des Hosts weitergeleitet wird.

=== "Physische Hosts (über USB-Netzwerkkarten)"
    | MAC Address    | Type    | Port |
    | -------------- | ------- | ---- |
    | 110C.29FC.70A6 | DYNAMIC | F0/3 |
    | 110C.29FC.70A7 | DYNAMIC | F0/3 |
    | 000C.29FC.70A5 | DYNAMIC | F0/1 |
    | 000C.2982.4429 | DYNAMIC | F0/2 |
    | 000C.292D.9200 | DYNAMIC | F0/3 |
    | 000C.29CE.83BE | DYNAMIC | F0/3 |

=== "Virtuelle Maschinen (über virtuelle NICs via USB-NICs der Hosts)"
    | MAC Address    | Type    | Port |
    | -------------- | ------- | ---- |
    | 110C.29FC.70A6 | DYNAMIC | F0/3 |
    | 110C.29FC.70A7 | DYNAMIC | F0/3 |
    | 000C.29FC.70A5 | DYNAMIC | F0/1 |
    | 000C.2982.4429 | DYNAMIC | F0/2 |
    | 110C.29FC.70A1 | DYNAMIC | F0/1 |
    | 110C.29FC.70A2 | DYNAMIC | F0/2 |
    | 000C.292D.9200 | DYNAMIC | F0/3 |
    | 000C.29CE.83BE | DYNAMIC | F0/3 |
    | 110C.29FC.70A3 | DYNAMIC | F0/3 |
    | 110C.29FC.70A4 | DYNAMIC | F0/3 |


!!! info
    Da MAC-Adressen in der Switch-Tabelle nach einer gewissen Zeit (z. B. 300 Sekunden) automatisch gelöscht werden, kann es notwendig sein, erneut Netzwerktraffic (z. B. per `ping`) zu erzeugen, damit der Switch die MAC-Adresse wieder in die Tabelle aufnimmt.

## MAC-Trace

Mit den folgenden Befehlen kann man eine Layer-2-Pfadverfolgung durchführen, wenn man herausfinden möchte, an welchem Port sich eine MAC-Adresse an einem Switch befindet:

```cli
show mac address-table address <MAC-Adresse in US-Format>
```

Zeigt, welche Art von Gerät an welchem Port hängt, da der oben ausgegebene Port auch ein Switch sein könnte:

=== "Cisco-Geräte"
    ```cli
    show cdp neighbors
    ```

=== "Nicht Cisco-Geräte"
    ```cli
    show lldp neighbors
    ```
