# Arch Linux einrichten

```
                  -`  
                 .o+`  
                `ooo/  
               `+oooo:  
              `+oooooo:  
              -+oooooo+:  
            `/:-:++oooo+:  
           `/++++/+++++++:  
          `/++++++++++++++:  
         `/+++ooooooooooooo/`  
        ./ooosssso++osssssso+`  
       .oossssso-````/ossssss+`  
      -osssssso.      :ssssssso.  
     :osssssss/        osssso+++.  
    /ossssssss/        +ssssooo/-  
  `/ossssso+/:-        -:/+osssso+-  
 `+sso+:-'                 '.-/+oso:  
`++:.                           '-/+/  
.`                                 '/  
```

## Konfiguration

Diese Anleitung beschreibt die grundlegende Konfiguration des Betriebssystems Arch Linux.  
Sie zeigt die wichtigsten Befehle, jedoch ohne detaillierte Zwischenschritte  
(z. B. nach dem Ausführen von archinstall).

### Tastaturlayout einstellen

Für ein deutsches Tastaturlayout:

```bash
loadkeys de-latin1
```

### Mit WLAN verbinden

Öffne iwctl:

```bash
iwctl
```

Scanne nach verfügbaren Netzwerken:

```bash
station wlan0 get-networks
```

Verbinde dich mit einem Netzwerk (ersetze <YourPasswordHere> und <YourSSID> entsprechend):

```bash
iwctl --passphrase "<YourPasswordHere>" station wlan0 connect <YourSSID>
```

Beende iwctl:

```bash
exit
```

### SSH überprüfen und aktivieren

Prüfe, ob der SSH-Dienst läuft:

```bash
systemctl status sshd
```

Falls nicht aktiv, starte ihn:

```bash
systemctl start sshd
```

Setze ein Root-Passwort (erforderlich für SSH-Zugriff):

```bash
passwd
```

### Arch Linux konfigurieren

Starte den Installationsassistenten:

```bash
archinstall
```
### Systemeinstellungen anzeigen lassen

Installiere das Tool fastfetch (ähnlich wie Neofetch, um Systeminformationen anzuzeigen):

```bash
pacman -S fastfetch
```
