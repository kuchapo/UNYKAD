# DNS

```bash
nano /etc/hosts						# DNS-Konfiguration
nmcli con show
nmcli dev show | grep DNS
nslookup pluralsight.com
nmcli con mod enp1s0 -ipv4.dns "192.168.122.1"		# entfernt den DNS Eintrag
nmcli con mod enp1s0 ipv4.dns "192.168.122.1"		# fügt den DNS Eintrag hinzu
nmcli con mod enp1s0 ipv4.ignore-auto-dns yes		# normalerweise bekommt NIC automatisch DNS-Server vom DHCP; Der Befehl schaltet das aus -> manuell konfigurierter DNS wird aktiv
nmcli con up enp1s0					# macht Änderungen aktiv

cat /etc/resolv.conf					# zeigt den aktuellen DNS-Server an
sudo dnf install unbound -y				# installiert DNS-Resolver, also den DNS-Server selbst
cd /etc/unbound/
sudo mv unbound.conf x.unbound.conf.backup		# Backup für alle Fälle
sudo nano unbound.conf
	server:
    		interface: 0.0.0.0			# Listen on all interfaces (localhost included)
    		do-ip4: yes				# Enabl IPv4
    		do-ip6: no				# Disable IPv6 (optional, enable if needed)
    		cache-max-ttl: 86400			# Cache entries for up to 24 hours
    		harden-dnssec-stripped: yes		# Enforce DNSSEC Validation for security	# Allow local queries
    		access-control: 127.0.0.0/8 allow	# Allowing DNS-requests only from this network
    		access-control: 172.30.73.64/26 allow	# Allowing DNS-requests only from this network
		chroot: ""				# chroot isoliert Programme in ein eigenes Mini-Dateisystem; Eintrag deaktiviert Funktion -> keine Fehler mehr (https://access.redhat.com/solutions/7124621)

	forward-zone:
    		name: "."				# Forward all queries (root zone)
    		forward-addr: 8.8.8.8			# Use Google DNS as upstream
    		forward-addr: 8.8.4.4			# Secondary upstream

	remote-control:
		control-enable: yes
		control-interface: 127.0.0.1
		control-port: 8953
		server-key-file: "/etc/unbound/unbound_server.key"
		server-cert-file: "/etc/unbound/unbound_server.pem"
		control-key-file: "/etc/unbound/unbound_control.key"
		control-cert-file: "/etc/unbound/unbound_control.pem"
unbound-checkconf					# Überprüft, ob es Fehler gibt
sudo systemctl enable --now unbound
nmcli con mod enp1s0 ipv4.dns "127.0.0.1"		# Network-Manager wird mitgeteilt, dass DNS über die Adresse 127.0.0.1 gezogen werden sollte und nicht vom DHCP-Server
nmcli con mod enp1s0 ipv4.ignore-auto-dns yes		# manuell konfigurierter DNS wird aktiv
nmcli con up enp1s0					# macht Änderungen aktiv
dig pluralsight.com					# zeigt Domain Profile an, wie z. B. alle Hosting-Records, Record-Typen, Query Time, …
dig pluralsight.com					# Query time sollte kleiner werden, da im Cache die Domain-IP-Zuweisung gespeichert ist
unbound-control-setup					# richtet die Zugriffsschlüssel (TLS‑Zertifikate & private Schlüssel) ein
systemctl restart unbound				# startet den Dienst neu
unbound-control dump_cache				# zeigt den kompletten DNS-Cache als Text
unbound-control flush tbh.eu				# löscht den Eintrag aus dem Cache
unbound-control reload					# lädt unbound neu (und löscht damit auch den Cache)
unbound-control flush_zone .				# löscht nur den Cache, ohne unbound neu zu laden
```
