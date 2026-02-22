# Firewalld

Eine Zone definiert die Firewall‑Regeln. Ein Network Interface wird genau einer Zone zugewiesen und erhält dadurch diese Regeln.
Es können also mehrere NICs in derselben Zone sein, aber eine NIC nicht in mehreren Zonen.

```bash
sudo iptables -L									# Vorgänger von nftables; kaum noch benutzt
nft list ruleset									# Low-Level Firewall-System; das wirkliche System, das entscheidet; firewalld ist High-Level (komfortables Bedienpanel)

sudo firewall-cmd --list-all								# zeigt detailiert die Default-Zone an
firewall-cmd --zone=public --list-all							# zeigt detailiert eine ausgewählte Zone an (auch inaktive Zonen)
sudo firewall-cmd --get-active-zones							# zeigt alle aktiven Zonen an
sudo firewall-cmd --get-default-zone							# zeigt die Default-Zone an
firewall-cmd --get-zones								# zeigt alle aktiven und inaktiven Zonen an
firewall-cmd --zone=public --list-ports							# zeigt alle freigeschalteten Ports einer Zone an; falls hier etwas nicht drin steht, dann vielleicht unter Services
firewall-cmd --zone=public --list-service						# zeigt alle freigeschalteten Services einer Zone an
firewall-cmd --zone=public --list-protocols						# zeigt alle freigeschalteten Protokolle; "esp" (=encapsulation security payload) agiert auf Layer 3 und hat keine Portnummer
firewall-cmd --zone=public --list-sources						# zeigt der Zone zugewiesenen IP-Adressen an

sudo firewall-cmd --permanent --new-zone=restricted_zone				# Erstellt eine neue Zone
sudo firewall-cmd --permanent --zone=public --add-port=80				# falls Error: sudo systemctl enable httpd; geht auch z. B. "--add-port=8080/tcp" oder "--add-port=8000-8080/tcp"
sudo firewall-cmd --permanent --zone=public --remove-port=80
sudo firewall-cmd --permanent --zone=public --add-service=http				# falls Error: sudo systemctl enable httpd
sudo firewall-cmd --permanent --zone=public --remove-service=http
sudo firewall-cmd --permanent --zone=public --add-protocol=esp
sudo firewall-cmd --permanent --zone=public --remove-protocol=esp
sudo firewall-cmd --permanent --zone=public --add-source=192.168.1.100			# fügt eine IP zur Zone hinzu; nur diese Zone greift auf die IP
sudo firewall-cmd --permanent --zone=public --remove-source=192.168.1.100
	# Traffic von genau dieser IP-Adresse wird dieser Zone zugewiesen
	# andere Zonen spielen für diese IP keine Rolle mehr
	# "192.168.1.100/24" geht auch

sudo firewall-cmd --permanent --zone=restricted_zone --add-icmp-block=echo-request	# blockiert das Pingen
sudo firewall-cmd --permanent --zone=public --remove-interface=enp9s0			# entfernt das Interface aus der Zone
sudo firewall-cmd --permanent --zone=restricted_zone --add-interface=enp9s0		# fügt dem Interface eine Zone hinzu
sudo firewall-cmd --reload								# macht die Änderungen sofort aktiv

systemctl restart firewalld								# falls irgendwas fehlschlägt
```
