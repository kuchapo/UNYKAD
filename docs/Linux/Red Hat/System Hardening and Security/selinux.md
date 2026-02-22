# SELinux

- Access Control Architektur, welche Regeln für Prozesse setzt (welcher Prozess darf auf was zugreifen)

```bash
dnf install setools-console				# bietet zusätzliche Tools für SELinux
seinfo -u
	# zeigt die SELinux-Benutzer -> nicht die Linux-Benutzer aus '/etc/passwd'
	# jeder Linux‑Benutzer ist genau einem SELinux‑Benutzer zugeordnet (z. B. unconfined_u, staff_u, user_u)
seinfo -r
	# zeigt die SELinux‑Rollen
	# Rollen bestimmen, in welche SELinux‑Domains (Prozess‑Typen = Sicherheitslabels für laufende Programme) ein SELinux‑Benutzer wechseln darf
seinfo -t | grep httpd
	# zeigt alle Types an, also Prozesse & Objekte (Dateien, Ports, usw.)
ls -Z /etc/passwd					# gibt den Context einer Datei aus, man sieht welchem type die Datei zugeordnet ist
	system_u:object_r:passwd_file_t:s0 /etc/passwd	# user:role:type:level
sudo semanage login -l					# zeigt welcher Benutzer welchem SELinux-Benutzer zugewiesen ist + Levels (MLS/MCS Range)
less /etc/selinux/targeted/setrans.conf			# setrans.conf übersetzt SELinux‑Level (s0, s0:c1,c2) in lesbare Namen; Für Zugriff braucht Prozess und Datei dasselbe Level

getenforce						# zeigt aktuellen SELinux-Mode an
setenforce 0						# Permissive; SELinux inaktiv, aber loggt alles

less /etc/selinux/config				# Allgemeine Config-File für SELinux; fürs vollständige deaktivieren und reaktivieren von SELinux zum Beispiel
ps auxZ | grep httpd					# 'ps -eZ | grep httpd' zeigt fast dasselbe an
	# 'S' steht für 'Stat', also Interruptible Sleep (auf Ereignis wartend, kann durch Signal geweckt werden)
	# 's' steht 'Session Leader' und ist der parent-process (managed andere worker Prozesse); hat eine eigene Prozessgruppe
	# 'l' steht für 'multi-threaded' worker Prozess; sie nehmen Anfragen an und benutzen Threads um mehrere Verbindungen zu managen
getsebool -a | grep httpd				# zeigt über SELinux aktivierte Regeln für Domains
setsebool -P httpd_can_network_connect off		# on/off oder 1/0; '-P' setzt es auf permanent, also über den Reboot hinweg
semanage fcontext -l | grep '/var/www/html'
	# zeigt die Regeln, nicht die echten Dateien
	# alles was links im Pfad landet bekommt den Context/Label der rechts steht
	# auch wenn die Packete noch gar nicht installiert sind, welche die Pfade benötigen, die Regeln sind trotzdem bereits vordefiniert
id -Z							# zeigt welche Rolle man mit dem Login bekommen hat; unconfined bedeutet uneingeschränkter Zugriff auf alles

semanage port -a -t http_port_t -p tcp 8888		# erlaubt es einer App auf tcp Port 8888 zu hören

sesearch --allow -s httpd_t -t httpd_sys_content_t -c file		# prüft, ob Domain 'httpd_t' Zugriff auf Objekt 'httpd_sys_content_t' hat; '-s'=source; '-t'=target; '-c'=Objektklasse

# Context ändern
mkdir -p /custom/data
nano /custom/data/index.html
ls -Z /custom/data							# Type = default_t; das bedeutet, dass keine Domain zugriff darauf hat
semanage fcontext -a -t httpd_sys_content_t "/custom/data/(/.*)"	# '-a'=add; '-t'=type
restorecon -Rv /custom/data/						# '-R'=recursive; '-v'=verbose Output nach dem Ausführen des Befehls; aktualisiert den Context

# Troubleshooting Beispiel
ls -Z /root
touch /root/secret.txt							# als root ausführen; sollte selben type entsprechend erben
runcon -u system_u -r system_r -t  httpd_t cat /root/secret.txt		# '-u'=selinux-user; '-r'=role; '-t'=type; Falscher SELinux-Context -> wirft Fehler in die Logs
cd /var/log/audit/							# Log-File
less audit.log | grep AVC						# AVC=Access Vector Cache (zeigt denials); 'scontext' zeigt source context und 'tcontext' target context
ausearch -m AVC								# zeigt ebenfalls logs; wichtig ist, dass SELinux immer nur den Zugriff blockt, der zuerst verboten wurde
```

Linux-User -> SELinux-User -> Rolle1 / Rolle2 / Rolle3
Es wird pro Benutzerlogin genau eine Rolle aktiv. Diese Rolle entscheidet dann in welche SELinux-Domains, also Sicherheitslabels für laufende Programme, der Benutzer wechseln darf
„Darf ein Prozess mit Domain X auf ein Objekt (Datei/Verzeichnis/Socket etc.) mit Type Y zugreifen?“ -> nie umgekehrt!
