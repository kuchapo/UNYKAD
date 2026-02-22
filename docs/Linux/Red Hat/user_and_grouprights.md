# Benutzer- und Gruppenrechte (Discretionary Access Control)

```bash
sudo usermod -aG wheel <benutzername>		# '-G' wheel = setze die sekundären Gruppen auf wheel; '-a' = lasse alle anderen Gruppen, die er hat, unberührt

sudo chmod g+s dev/Projects
	# alle neuen Dateien und Unterordner erben die gleiche Gruppenzugehörigkeit
	# zeigt sich am 's' (also 'drwxrws---') und nennt sich SGID/SETGID
	# SUID wird zum Beispiel bei der '/usr/bin/passwd'-file benötigt, da passwd nur KURZZEITIG mit den root-Rechten bzw. des Dateibesitzers arbeiten muss, z.B. bei Passwortwechsel
	# Special Bits: setuid 4xxx, setgid 2xxx, sticky 1xxx
	# Sticky Bit protects deleting by not owner users (example: /tmp)

export PATH=$PATH:/home/admin			# alles was in diesem Pfad liegt kann man auch ohne Pfadangabe dann ausführen (z. B.: my_script statt /home/admin/my_script)
ps aux | grep pyhton >> procfile		# ps = process status, a = all with TTY, u = user friendly, x = processes with no TTY, schreibt Ausgabe in procfile
sudo useradd -m mlopez				# '-m' legt auch im home-Verzeichnis den Benutzerpfad an
sudo passwd mlopez				# legt Passwort für den Benutzer an
sudo passwd -l mlopez				# Sperrt den Benutzeraccount, indem der Passworthash in /etc/shadow mit einem ! vor dem Hash unbrauchbar gemacht wird
sudo chown -R acarter:developers /home/mlopez	# Überträgt den Owner an acarter und Gruppe an developers bei dem Pfad rekursiv
sudo chgrp developers dev/Projects		# Ändert nur den Gruppen-Besitzer des Pfades
sudo chmod 770 dev/Projects			# Owner und Gruppe haben alle Rechte und Other keine

/etc/login.defs					# Rahmenbedingungen der Benutzerverwaltung, falls man hier nicht das findet was man sucht, dann 'pwquality.conf'
/etc/security/pwquality.conf			# Regeln für Passwortstärke

sudo usermod -aG developers acarter
cat /etc/group | grep developers		# zeigt, ob die Gruppe developers existiert
```
