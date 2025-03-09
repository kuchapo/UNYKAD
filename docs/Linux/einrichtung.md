# Arch einrichten

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

## Konfiguration (Grob)

Deutsches Tastaturlayout:
```bash
loadkeys de-latin1
```
WLAN verbinden:
```bash
iwctl --passphrase "YourPasswordHere" station
```
```bash
wlan0 connect <>
```
Überprüft, ob SSH ativiert ist:
```bash
systemctl status sshd
```
Aktiviert SSH:
```bash
systemctl start sshd
```
Root Passwort festlegen (sonst funktioniert SSH nicht):
```bash
passwd
```
Betriebssystem konfigurieren:
```bash
archinstall
```
Um Systemeinstellungen nachzuschlagen:
```bash
pacman -S fastfetch
```
