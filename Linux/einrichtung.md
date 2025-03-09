# Arch einrichten
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
