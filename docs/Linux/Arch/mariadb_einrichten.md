# MariaDB Installation & Einrichtung

MariaDB ist eine leistungsstarke Open-Source-Datenbank. Diese Anleitung hilft dir, sie auf Arch Linux zu installieren und sicher einzurichten.

---

## Installation von MariaDB

MariaDB installieren

   ```bash
   sudo pacman -S mariadb
   ```

Datenbank initialisieren

   ```bash
   sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
   ```

MariaDB als Service starten

   ```bash
   sudo systemctl start mariadb.service
   ```

MariaDB beim Booten automatisch starten

   ```bash
   sudo systemctl enable mariadb.service
   ```

---

## Sicherung & Grundkonfiguration

Nach der Installation solltest du das Sicherheitsskript ausführen:

```bash
sudo mariadb-secure-installation
```

Dies ermöglicht dir:
✅ Root-Passwort festzulegen  
✅ Anonyme Benutzer zu entfernen  
✅ Test-Datenbanken zu löschen  
✅ Remote-Root-Login zu deaktivieren  

**Nach Abschluss der Sicherung kannst du dich mit MariaDB verbinden:**

```bash
sudo mysql -u root
```

---

## Neue Datenbank & Benutzer erstellen

Neue Datenbank anlegen

   ```sql
   CREATE DATABASE <meine_datenbank>;
   ```

Neuen Benutzer erstellen & Passwort setzen

   ```sql
   CREATE USER '<mein_benutzer>'@'localhost' IDENTIFIED BY '<mein_passwort>';
   ```

Benutzer Berechtigungen für die Datenbank geben

   ```sql
   GRANT ALL PRIVILEGES ON <meine_datenbank>.* TO '<mein_benutzer>'@'localhost';
   ```

Änderungen übernehmen

   ```sql
   FLUSH PRIVILEGES;
   ```

---

## Verbindung zur Datenbank testen

Melde dich mit dem neu erstellten Benutzer an:

```bash
mysql -u <mein_benutzer> -p <meine_datenbank>
```

Gib dein Passwort ein und du solltest Zugriff auf die Datenbank haben! ✅

---

## MariaDB stoppen oder neu starten

Falls du MariaDB stoppen oder neustarten musst:

```bash
sudo systemctl stop mariadb.service
sudo systemctl restart mariadb.service
```
