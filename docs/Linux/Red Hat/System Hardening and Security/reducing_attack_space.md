# Reduzierung der attackierbaren Fläche

## Analyse

```bash
systemctl list-Units --type=service --state=running			# zeigt alle laufenden Services
nmap localhost								# zeigt offene Port nach draußen
ss -tuln								# zeigt NetworkSockets an
lsof -i :323								# zeigt wer diesen Port benutzt
Firewall-cmd --list-all							# listet alle Services und Ports welche von der Firewall erlaubt sind
dnf install lynis							# Audit-Tool
lynis audit system							# führt den Audit durch
dnf update								# muss regelmäßig durchgeführt werden damit das System gepatcht bleibt; kann auch automatisiert werden
```

## PAM-Konfigurationen

```bash
cd /etc/authselect
less password-auth							# für Netzwerk-/Remote-Logins -> nicht editierbar
less system-auth							# für lokale Logins -> nicht editierbar
authselect create-profile custom-profile -b sssd
	# erstellt eine Kopie des bestehenden sssd-Profils um es verändern zu können
	# SSSD = System Security Services Daemon
	# SSSD wird für zentrale Benutzerverwaltung genutzt (LDAP, AD,...)
cd /etc/authselect/custom/custom-profile
less password-auth
authselect select custom/custom/profile					# sagt, dass ab jetzt dieses Profil genutzt werden sollte -> system-auth und password-auth werden daraus generiert

cd /etc/security
less pwquality.conf							# Passwortqualität einstellen, wie z. B. Mindest-Länge
less faillock.conf							# Logout-Regeln
```

## Security Events Tracking with AIDE

```bash
dnf install aide -y							# installiert ein Tool zum Erkennen von Datei‑Manipulationen (Integrity Monitoring); z. B. wenn jemand eine wichtige File anpasst
aide --init								# Erstellt die erste Baseline-Datenbank mit allen Hashes/Checksummen, damit aide Änderungen wahrnehmen kann
mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz		# Die neue Datenbank wird zur aktiven Baseline gemacht
nano /etc/aide.conf							# Konfigurationsdatei: Welche Pfade / Dateitypen / Regeln überwacht werden sollen
aide --check								# Prüft das System gegen die Baseline und zeigt erkannte Änderungen an
less /var/log/aide/aide.log						# Vergangene Checks, Ergebnisse und Fehlermeldungen ansehen
aide --update								# Erstellt eine neue Baseline (aide.db.new.gz), wenn die Änderungen gewollt sind
mv /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz		# Die neue Datenbank wird zur aktiven Baseline gemacht
```

## Security Events Tracking with auditd

```bash
nano /etc/audit/rules.d/audit.rules					# Diese Datei enthält die Audit-Regeln. Jede Regel definiert, welche Dateien oder Aktionen überwacht werden.
									# -w  = watch (überwacht eine Datei oder ein Verzeichnis)
									# -p  = welche Aktionen überwacht werden: r=read, w=write, x=execute, a=attribute change
									# -k  = benutzerdefinierter Suchbegriff (Key), um Einträge in den Logs leichter zu finden
	## Monitor critical files
	-w /etc/passwd -p wa -k passwd_changes
	-w /etc/shadow -p wa -k shadow_changes
	-w /etc/sudoers -p wa -k sudoers_changes

	## Monitor authentication
	-w /var/log/faillog -p wa -k auth_fail
	-w /var/log/lastlog -p wa -k auth_log
augenrules --load							# macht die Änderungen aktiv
auditctl -l								# zeigt die aktuell aktiven Regeln an
aureport --auth								# zeigt Zusammenfassung der auditd Logs; zeigt alle Authentication-Versuche
aureport --failed							# zeigt, was fehlgeschlagen ist
ausearch --raw -m USER_LOGIN -sv no					# --raw = unformatted; -m = message type; -sv = success value
```
