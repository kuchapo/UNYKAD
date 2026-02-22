# Logs

```bash
su root
cat /var/log/anaconda/lvm.log | grep vd			# man sieht hier z. B, wie LVM die Partitionen wie vda1 und vda2 erstellt hat
less /etc/logrotate.conf				# regelt die Rotation von klassischen Logdateien (z. B. /var/log/syslog, /var/log/auth.log, /var/log/nginx/*.log); journald ausgeschlossen
cd /etc/logrotate.d					# dort befinden sich die einzelnen Services für die Logs die man anpassen kann
less dnf
	# 'missingok' sagt, dass es keine Fehler-Benachrichtigung ausgeben sollte, falls genau dieser Service keinen einzigen Log ausgibt
	# 'notifempty' sagt, dass die Logdatei nicht rotiert (umbenannt/archiviert) wird, wenn sie leer ist
	# 'rotate 4' & 'weekly' sind die gleiche Art von Einstellung wie in '/etc/logrotate.conf'; diese Werte überschreiben logrotate.conf
	# 'create' erstellt eine neue leere Log-Datei, wenn die alte rotiert worden ist

journalctl						# zeigt alle System-Logs seit dem letzten System-Boot an; Suchen mit '/<Suchbegriff>' ('n'-Taste ist der nächste Treffer und 'N' vorheriger Treffer)
systemctl list-unit-files | grep enabled		# zeigt alle systemd enabled-Services, wie z. B. 'NetworkManager.service', an
journalctl -u NetworkManager				# sortiert nun die System-Logs nach der festgelegten Unit
journalctl -u NetworkManager --since "20 minutes ago"	# erlaubt es nach Eintragen zu suchen, die maximal 20 Minuten her sind
sudo nano /etc/systemd/journald.conf
	# Hash-Tag vor dem 'Storage=auto' entfernen und 'auto' in 'persistent' ändern -> Logs werden gespeichert und nicht mehr nach Reboot verworfen
	# hier drin befinden sich auch die Einstellungen, wie Backups verwaltet werden sollen
sudo systemctl restart systemd-journal			# macht die Änderungen aktiv
journalctl -u NetworkManager -b -2			# zeigt Einträge, welche genau 2 System-Boots her sind
```