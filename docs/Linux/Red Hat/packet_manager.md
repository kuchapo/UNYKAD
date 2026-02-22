# Packetmanager

- 'rpm' ist für die Installation, updating und entfernen von Software (.rpm-Format) verantwortlich
- 'dnf' funktioniert mit rpm unter der Haube
- 'yum' war bis 2015 das Standard-Packetmanagement-Tool
- Beim Herunterladen gibt es ein GPG-Key, um den Traffic beim Herunterladen zu verschlüsseln.

```bash
which rpm				# zeigt die Pfade, gilt auch für dnf und yum, mit rpm/dnf/yum zeigt wie man es benutzt
sudo dnf remove httpd			# entfernt Packet selbst und alle Abhängigkeiten, wenn diese nicht bei anderen Packets verwendet werden
sudo dnf update				# updated alle Packages
which nano				# zeigt den Speicherort an, wo nano sich befindet
rpm -qf /usr/bin/nano			# -qf = query file, Ausgabe ist Repository-Package durch das nano installiert worden ist
rpm -q openssh-server			# zeigt ebenso das Repository-Package durch das ssh installiert worden ist; diesmal ist es aber kein Pfad
rpm -qi nano-5.6.1-7.el9.x86_64		# -qi = query info, zeigt unter Anderem die Lizenz und die Key ID an (Key ID kann man im Internet abgleichen)
rpm -qR nano-5.6.1-7.el9.x86_64		# -qR = query requires, ist gut fürs Troubleshooting
rpm -Vf /usr/bin/nano			# überprüft, ob die Binary von Nano keine Fehlenden Abhängingkeiten hat, keine Ausgabe = gut
```
