# SSH in Windows aktivieren (Client & Server)

SSH (Secure Shell) ist in Windows integriert und kann sowohl **clientseitig** (zum Verbinden mit anderen Servern) als auch **serverseitig** (zum Fernzugriff auf den eigenen Windows-PC) aktiviert werden.

---

## **1. SSH-Client aktivieren (Windows als SSH-Client)**

### **Überprüfen, ob der SSH-Client installiert ist**

Öffne eine **PowerShell** mit Administratorrechten und gib ein:

```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH.Client*'
```

Falls die Ausgabe **"State: Installed"** anzeigt, ist der SSH-Client bereits installiert.

Falls **nicht**, installiere ihn mit:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

➡ **Nach der Installation testen:**

```powershell
ssh
```

Falls eine Liste von Befehlen erscheint, ist der SSH-Client erfolgreich aktiviert.

➡ **Verbindung zu einem entfernten Server herstellen:**

```powershell

ssh benutzer@server-ip
```

Ersetze `benutzer` mit deinem Nutzernamen und `server-ip` mit der IP-Adresse des Servers.

---

## **2. SSH-Server aktivieren (Windows als SSH-Server)**

Damit dein Windows-PC per SSH erreichbar ist, musst du den **SSH-Server installieren**.

### **Überprüfen, ob der SSH-Server installiert ist**

```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH.Server*'
```

Falls nicht installiert, installiere ihn mit:

```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

### **SSH-Server als Dienst aktivieren**

Starte den SSH-Server und setze ihn auf "automatisch starten":

```powershell
Start-Service sshd
Set-Service -Name sshd -StartupType 'Automatic'
```

➡ **Prüfen, ob der SSH-Server läuft:**

```powershell
Get-Service sshd
```

---

## **3. Firewall für SSH anpassen**

Standardmäßig nutzt SSH **Port 22**. Um Verbindungen zuzulassen, musst du die Firewall konfigurieren.

### **Port 22 in der Windows-Firewall freigeben**

```powershell
New-NetFirewallRule -Name "SSH-Zugriff" -DisplayName "SSH-Zugriff" -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

➡ **Regeln überprüfen:**

```powershell
Get-NetFirewallRule | Where-Object DisplayName -like "*SSH*"
```

Falls du die Regel später löschen willst:

```powershell
Remove-NetFirewallRule -Name "SSH-Zugriff"
```

---

## **4. Verbindung zum Windows-SSH-Server testen**

Auf einem anderen Gerät (Linux, Mac oder Windows mit SSH-Client) kannst du dich nun verbinden:

```powershell
ssh benutzer@windows-ip
```

Ersetze `benutzer` mit deinen Windows-Login-Daten und `windows-ip` mit der lokalen oder öffentlichen IP des Rechners.

➡ **Lokale IP herausfinden:**

```powershell
ipconfig
```

Falls die Verbindung nicht klappt:

- Stelle sicher, dass der **SSH-Server (`sshd`) läuft**:

  ```powershell
  Get-Service sshd
  ```

- Überprüfe, ob der **Port 22 offen ist**:

  ```powershell
  Test-NetConnection -ComputerName localhost -Port 22
  ```

---

## **5. SSH-Server deaktivieren (Falls nicht mehr benötigt)**

Falls du SSH nicht mehr brauchst, kannst du den Dienst stoppen und deaktivieren:

### **SSH-Server beenden & deaktivieren**

```powershell
Stop-Service sshd
Set-Service -Name sshd -StartupType 'Disabled'
```

Falls du den SSH-Server komplett entfernen möchtest:

```powershell
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```
