# Networking

- setzt die IP direkt im Kernel
- sofort aktiv
- nicht persistent (nach Reboot oder Network-Restart weg)

```bash
sudo ip a add 10.0.3.100/24 dev enp9s0
```

## Netzwerk einrichten über NetworkManager-Konfiguration

- ist persistent (bleibt nach Reboot)
- wird erst nach Neustart der Verbindung aktiv

```bash
sudo nmcli connection show							# zeigt alle NetworkManager-Verbindungen, egal ob aktiv oder inaktiv
sudo nmcli connection show "Profile 1"						# zeigt detailierte Einstellungen einer Verbindung
sudo nmcli con modify 'ens192' connection.permissions user:dbclinton		# erlaubt die Benutzung der Verbindung nur dem Benutzer dbclinton
nmcli dev wifi connect "MySSID" password "mypassword"				# erlaubt es sich mit WLAN zu verbinden

# DHCP
nmcli con mod ens192 ipv4.method auto
nmcli con mod ens192 ipv4.addresses "" ipv4.gateway ""
sudo nmcli con down ens192
sudo nmcli con up ens192

# Manuell IP einrichten
sudo nmcli con modify 'ens192' ipv4.addresses '172.30.73.65/26'
sudo nmcli con modify 'ens192' ipv4.gateway '172.30.73.65'
sudo nmcli con modify 'ens192' ipv4.dns '9.9.9.9 8.8.8.8'
sudo nmcli con modify 'ens192' ipv4.method manual
sudo nmcli con down ens192
sudo nmcli con up ens192
```

### GUI-Version

```bash
sudo nmtui			# erst "Edit a connection" und IP-Einstellungen sowie Haken bei "Automatically connect" setzen, dann "Activate a connection"
sudo shutdown -r now		# damit Änderungen wirksam werden
```

## Networking-Tools

```bash
route			# zeigt IP Routing table
netstat -r		# zeigt IP Routing table
netstat -at		# '-a' zeigt alle Netzwerk-Sockets an, also aktive Verbindungen und Listener; '-t' sagt, dass es nur TCP-Verbindungen ausgeben soll
```
