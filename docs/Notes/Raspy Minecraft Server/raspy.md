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

```powershell
ssh-keygen -R <IP des Raspys>
```
