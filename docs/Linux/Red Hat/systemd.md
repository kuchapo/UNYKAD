# systemd

## Dienste starten

```bash
sudo systemctl enable httpd		# aktiviert den Dienst automatisch beim Booten
sudo systemctl start httpd		# startet den Dienst jetzt auch ohne Booten
sudo sytemctl enable --now httpd	# aktiviert fürs Booten UND startet den Dienst
```

## Erstellen eines eigenen systemd-Dienstes

Beispiel:

```bash
sudo dnf install inotify-tools
sudo touch /var/log/file_tracker.log
sudo chown nobody:nobody /var/log/file_tracker.log	# nobody hat die geringsten Berechtigungen (Security-Prinzip „Least Privilege“), weil Prozess kann nur das tun, was der Benutzer tun kann
sudo chmod 644 /var/log/file_tracker.log
nano /usr/local/bin/file-monitor.sh
	#!/bin/bash
	# Directory to monitor
	WATCH_DIR="/var/log/uploads"
	# Log file for tracking
	TRACKER_LOG="/var/log/file_tracker.log"

	# Ensure the watched Directory exists
	mkdir -p "$WATCH_DIR"

	# Monitor the Directory for new files
	while true; do
		inotifywait -e create "$WATCH_DIR" | while read -r path Action file; do
			echo "$(date '+%Y-%m-%d %H:%M:%S') - New file detected: $file" >> "$TRACKER_LOG"
		done
	done
sudo chmod +x /usr/local/bin/file-monitor.sh
sudo touch /etc/systemd/system/file-monitor.service
sudo nano /etc/systemd/system/file-monitor.service
	[Unit]
	Description=Monitor /var/log/Uploads for new files
	After=network.target

	[Service]
	Type=simple
	ExecStart=/usr/local/bin/file-monitor.sh
	Restart=always
	User=nobody
	Group=nobody
	StandardOutput=Journal
	StandardError=Journal

	[Install]
	WantedBy=multi-user.target
sudo chmod 644 /etc/systemd/system/file-monitor.service
sudo systemctl daemon-reload
sudo systemctl start file-monitor
sudo touch /var/log/uploads/newtestfile.txt
cat /var/log/file_tracker.log
```

## Units und Unit-Files

Units sind Ressourcen bzw. Objekte, die systemd aktiv verwaltet. Sie existieren zur Laufzeit im RAM und sind keine eigenständigen Dateien.
Unit-Files sind Konfigurationsdateien von systemd, welche beschreiben wie die Unit funktionieren soll. Dazu gehören u. a.:

- Services (*.service)		# läuft ständig und verbraucht RAM und bisschen CPU, auch wenn niemand per z. B. per SSH kommt
- Sockets (*.socket)		# hört auf Verbindungen (z. B. ssh); läuft nicht immer, aber meistens durch ein Netzwerk
- Mounts (*.mount)
- Timer (*.timer)
- Targets (*.target)		# Unit-Files, welche andere Units über Abhängigkeiten bündeln
- Devices (*.device)
- Paths (*.path)

```bash
cd /etc/systemd/system/		# wird als erstes geladen; hier sind Unit-Files für benutzerdefinierte/angepasste Units (haben Vorrang den Standard-Unit-Files gegenüber)
cd /run/systemd/system/		# temporärer Speicher für systemd-Units zur Laufzeit
cd /usr/lib/systemd/system	# wird als letztes geladen; vom System/Paketmanager bereitgestellte Standard-Unit-Files (werden bei Updates überschrieben)

less sshd.service
	# 'Documentation' verweist auf Handbuchseiten/URLs zur Doku, wie z. B. man:sshd(8), man:sshd_config(5)
	# 'After' sagt, dass der Dienst erst starten soll, wenn Netzwerk funktioniert und die SSH-Keys generiert worden sind -> garantiert es aber nicht, sorgt bloß für die Reihenfolge
	# 'ExecStart' startet den eigentlichen Dienstprozess (den 'daemon')
	# 'WantedBy=multi-user.target' bedeutet, dass beim Systemstart dieser Dienst automatisch gestartet werden soll, sobald das System den Zustand multi-user.target erreicht
		# 'multi-user.target' ist der normale Betriebsmodus ohne grafische Oberfläche (wäre 'graphical.target')
less sshd.socket
	# sshd.socket kann nicht laufen, wenn sshd.service läuft, da Port 22 bereits belegt ist; wenn ein definierter Port belegt ist -> Socket-Unit schlägt fehl
	# 'Conflicts' überprüft ob sshd.service läuft und verhindert unnötige Fehlermeldungen, falls der Socket bereits belegt ist
less tmp.mount
	# mountet /tmp als tmpfs (flüchtiger Speicher im RAM, ggf. mit Swap)
	# 'ConditionPathIsSymbolicLink=!/tmp' sorgt dafür, dass die Unit erst dann aktiviert wird, wenn '/tmp' kein Softlink ist
```
