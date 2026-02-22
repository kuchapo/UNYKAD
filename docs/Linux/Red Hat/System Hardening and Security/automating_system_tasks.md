# Automating System Tasks

```bash
at 2:00 AM tomorrow		# führt Befehl einmalig aus
	/home/script.sh		# das gewünschte Skript welches ausgeführt werden sollte; danach schließen mit STRG + D
atq				# zeigt aktiven at-Ausführungen

batch				# führt nur aus, wenn das System wenig ausgelastet ist

journalctl -u atd		# zeigt at history an

crontab -e			# verlässt man mit ':q', falls nano als Standard-Editor noch nicht ausgewählt wurde
nano .bashrc
	export EDITOR=nano	# ganz unten hinzufügen, damit nano der neue Standarteditor ist
crontab -e
	0 7 * * 1-5 /home/dbclinton/myscript.sh		# 0 7=7 Uhr morgens, *=jeden Tag des Monats, *=jeden Monat des Jahres, 1-5=Montag-Freitag; Rest ist der komplette Pfad zum Skript
ls /etc | grep cron		# man sieht hier unter anderem die Dateien, die bei anacrontab ausgeführt werden
less /etc/anacrontab
	# anacrontab ist für Ausführungen gedacht, welche auf jeden Fall ausgeführt werden sollen, auch wenn der Computer an genau dem Zeitpunkt es nicht ausführen kann (weil z. B. PC aus)
less /etc/cron.hourly/0anacron
	# das ist ein stündlicher Cron-Job, der anacron startet, aber nur wenn anacron heute noch nicht gelaufen ist (Prüfung durch '/var/spool/anacron/cron.daily') & Rechner am Strom hängt
	# das holt verpasste Jobs nach
less /etc/crontab		# Jobs hier drin werden als root ausgeführt; beim normalen 'crontab -e' als Benutzer, der es benutzt
systemctl list-timers		# zeigt alle aktuellen timer-units an; diese timer kann man über systemd aktivieren und deaktivieren

# Best practise
mkdir -p /usr/local/SCRIPTS/
nano /usr/local/SCRIPTS/test.sh
ll /usr/local/SCRIPTS/
	# ll=ls -l
chmod 700 /usr/local/SCRIPTS/test.sh
ll /etc/systemd/system/ | grep test			# test.service & test.timer sollen mit den Berechtigungen '-rw-r--r--' erscheinen

useradd -r -s /bin/false backupuser			# um Ownership einem custom user zu übergeben, muss dieser erst erstellt werden
							# '-s' legt /bin/false als default shell an -> verhindert Login in das System
							# '-r' erstellt einen Systemaccount
nano /etc/systemd/system/test.service
	[Unit]
	Description=Backup /var/www/html Directory
	After=network.target

	[Service]
	Type=oneshot
	ExecStart=/usr/local/SCRIPTS/test.sh
	User=Testuser
	ProtectSystem=strict				# verbietet das Schreiben in die meisten Filesystems
	PrivateTmp=true					# eigenes '/tmp', '/var',...; ist vom Rest des Systems isoliert
	NoNewPrivileges=true				# verhindert dass der main process und die child processes zusätzliche privileges nach dem Start bekommen
systemctl daemon-reload
system restart test.timer
chown testuser:testuser /usr/local/SCRIPTS/test.sh
ls -Z /usr/local/SCRIPTS/test.sh
chcon -t bin_t /usr/local/SCRIPTS/test.sh		# 'usr_t' ist mehr für normale Daten ohne Ausführung gedacht; 'bin_t' sagt, dass Dateien ausgeführt werden dürfen
ls -Z /var/log/test.log					# 'var_log_t' ist der korrekte type
restorecon -R /usr/local/SCRIPTS/			# macht Änderungen wirksam; '-R'=recursive
```
