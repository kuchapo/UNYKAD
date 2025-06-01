# Switch Basics

- Switches funktionieren auf Layer 2
- An jedem physischen Port besitzt ein Switch ein ASIC
- ASIC:
    - ist eine integrierte Schaltung, welche den Frame Header ließt
    - fügt die MAC-Adresse in die MAC Address Table hinzu
    - nicht programmierbar wie beim Mikrocontroller
- Wenn zwei Geräte miteinander kommunizieren, dann bekommen es andere Geräte nicht mit
- Wenn ein Gerät für 300 Sekunden keine Daten in den Switch reinschickt, verwift der Switch die MAC-Adresse des Geräts
- Wenn ein Switch die MAC-Addresse des Zielgeräts nicht kennt, dann schickt er ein Broadcast (an alle Ports, außer dem Eingangsport) -> Flooding
    - Zielgerät schickt dann antwort an den Sender, dass dieser bereit ist zu kommunizieren
    - ASIC speichert MAC-Adresse des Zielgeräts
- L3-Switches funktionieren auf Layer 2 & 3
- L3-Switches werden häufig als Core-Switches verwendet

## Modi

Wie beim Router

## Speicherplätze

Wie beim Router, nur mit dem Unterschied, dass im Flash noch die config.txt existiert, welche den NVRAM darstellt.

`config.txt` = NVRAM, also Startup-Config

## Konfiguration

- Via Patchkabel zwischen Ethernet Port (PC) und Switch Port (Switch) verbinden
- Via Rollover-Kabel zwischen RS-232 Port (PC) und Console Port (Switch) verbinden
- PuTTY öffnen und verbinden
- 'no' eingeben

### Hostname vergeben

Wie beim Router

### Banner erstellen

Wie beim Router

### SSH aktivieren

Wie beim Router

### IP-Adressen einrichten

IPs werden bei Switches nicht geroutet, deshalb erstellt man sogenannte VLANs, also virtuelle Interfaces.

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

## Beispiel Lab

![switch_beispiel](assets/switch_beispiel.drawio.svg)

## Wichtige Switch Befehle

Zeigt die MAC Address Table an:

```cli
show mac address-table
```
