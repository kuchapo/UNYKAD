# DDNS

## Cronjob öffnen

```bash
crontab -e
```

## Eintrag einsehen

```bash
10 5 * * * /home/xozah/cloudflare-ddns.sh >> /home/xozah/cloudflare-ddns.log 2>&1
```

Dieser Eintrag sagt aus, dass das Skript `cloudflare-ddns.sh` jeden Morgen um 5:10 ausgeführt wird.

| Feld | Bedeutung          |
| ---- | ------------------ |
| `10` | Minute (10)        |
| `5`  | Stunde (5 Uhr)     |
| `*`  | Jeder Tag im Monat |
| `*`  | Jeder Monat        |
| `*`  | Jeder Wochentag    |

Der letzte Teil des Skripts sorgt dafür dass die Terminalausgabe beim Ausführen des Skripts in die log-Datei landet, also unabhängig davon ob die Ausführung geklappt hat oder nicht.

## Skript

Dieses Skript funktioniert in Kombination mit Aufgabenplanung/Cronjob via Cloudflare.
Die Variablen `$zoneId`/`ZONE_ID`, `$recordId`/`RECORD_ID`, `$apiToken`/`API_TOKEN` und `$recordName`/`RECORD_NAME` müssen entsprechend angepasst werden.
Das letztere ist die Domain.

=== "Windows"
    ```powershell
    # Konfiguration
    $zoneId = "<zoneId>"
    $recordId = "<recordID>"
    $apiToken = "<apiToken>"
    $recordName = "<recordName>"

    # Aktuelle öffentliche IPv4-Adresse abrufen
    $ip = Invoke-RestMethod -Uri "https://api.ipify.org?format=text"

    # Cloudflare-API Header vorbereiten
    $headers = @{
        "Authorization" = "Bearer $apiToken"
        "Content-Type"  = "application/json"
    }

    # DNS-Record aktualisieren (PUT-Anfrage)
    $body = @{
        type    = "A"
        name    = $recordName
        content = $ip
        ttl     = 120
        proxied = $false  # Wichtig: Minecraft mag keine Proxied-Wolke
    } | ConvertTo-Json

    Invoke-RestMethod -Method PUT 
        -Uri "https://api.cloudflare.com/client/v4/zones/$zoneId/dns_records/$recordId" 
        -Headers $headers 
        -Body $body
    ```

=== "Linux"
    ```bash
    #!/bin/bash

    # Deine Zugangsdaten
    ZONE_ID="<ZONE_ID>"
    RECORD_ID="<RECORD_ID>"
    API_TOKEN="<API_TOKEN>"
    RECORD_NAME="<RECORD_NAME>"

    # Aktuelle öffentliche IP abrufen
    IP=$(curl -s https://api.ipify.org)

    # Cloudflare API-Aufruf
    UPDATE=$(curl -s -X PUT "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$RECORD_ID" \
        -H "Authorization: Bearer $API_TOKEN" \
        -H "Content-Type: application/json" \
        --data "{\"type\":\"A\",\"name\":\"$RECORD_NAME\",\"content\":\"$IP\",\"ttl\":120,\"proxied\":false}")

    # Erfolg prüfen
    if echo "$UPDATE" | grep -q '"success":true'; then
    echo "[OK] DNS-Eintrag aktualisiert auf $IP"
    else
    echo "[FEHLER] Antwort von Cloudflare:"
    echo "$UPDATE"
    fi
    ```
