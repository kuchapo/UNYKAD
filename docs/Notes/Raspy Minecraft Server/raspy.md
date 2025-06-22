# Raspy

## Raspberry Pi OS aufsetzen

- Raspberry Pi Imager herunterladen
- entsprechendes OS und SD-Karte auswählen
- Drücke `Strg` + `Shift` + `X`
- Hostname, Benutzer, WLAN und SSH anpassen

## Via SSH verbinden

=== "via IP"
    ```powershell
    ssh <Username>@<IP-Adresse>
    ```

=== "via Hostname"
    ```powershell
    ssh <Username>@<Hostname>.local
    ```

### Debugging

Wenn sich das Verbinden nicht klappt und eine Fehlermeldung mit "Man in the Middle Attack" erscheint, dann:

```powershell
ssh-keygen -R <IP des Raspys>
```

## SSH: Secure Copy (scp)

Mit `scp` hat man die Möglichkeit Dateien auf den Raspberry Pi herunterzuladen und hochzuladen:

### Manuelles Backup vom Raspberry Pi

```powershell
scp -r "xozah@raspy.local:'/home/xozah/MC UNYKAD'" "C:\Users\Xozah\Documents\Minecraft Backup"
```

### Datapacks updaten

```powershell
scp -r "C:\Users\Xozah\Downloads\<Name of the datapack with extension>" xozah@raspy.local:"/home/xozah/MC UNYKAD/world/datapacks/"
```

## Dateien ausführbar machen

In Bash macht man mit dem folgenden Befehl eine Datei ausführbar:

```bash
chmod +x <Name of the file>
```
