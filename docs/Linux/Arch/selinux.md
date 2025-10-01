# SELinux – Security Enhanced Linux

- SELinux ist ein Linux Security Module (LSM) im Kernel (LSM-Framework) und wurde ursprünglich von der NSA entwickelt (heute ist Red Hat der Hauptentwickler)
- Es ist keine Firewall, sondern eine Sicherheitsframework auf Kernel-Ebene, welche systemweite Zugriffskontrollen durchsetzt. Sie bestimmt also, welcher Prozess auf welche Ressource zugreifen darf, basierend auf Labels und Regeln.
- ist im Kernel seit der Version 2.6 (~ 2003) und ist bei den Red Hat basierten Distributionen (z. B. Red Hat, Fedora, CentOS, AlmaLinux,...) oft standardmäßig eingeschaltet
- Andere Distributionen, wie Debian und Ubuntu, benutzen AppArmor (auch LSM)!
- Normalerweise hat man nur seine "normalen" UNIX-Rechte (DAC)
- DAC (Discretionary Access Control) stellt die „normalen“ UNIX-Rechte (UID/GID/Mode, ggf. ACLs) dar
	- **UID**: die User-ID – sagt, **welcher Benutzer** Eigentümer einer Datei ist (z. B. `root`, `alice`)
	- **GID**: die Group-ID – sagt, **welche Gruppe** Eigentümer ist (z. B. `wheel`, `devs`).
	- **Mode/Permissions**: `rwx` für Owner | Group | Others
		- Beispiel: `-rwxr-x---` = numerisch 750 (bzw. 0750 vierstellig)  
		- Zahlenwerte: r=4, w=2, x=1
		- Erste Ziffer bei vierstelliger Schreibweise sind Spezialbits (bei `0` keine gesetzt):
			- **setuid (4):** Prozess läuft mit Owner-Rechten der Datei (z. B. `passwd`).
			- **setgid (2):** Prozess/Ordner nutzt Gruppen-ID der Datei; bei Ordnern **erben** neue Dateien die Gruppe.
			- **sticky (1):** In Ordnern dürfen nur Eigentümer eigene Dateien löschen (z. B. `/tmp`).
- SELinux bietet zum DAC eine zusätzliche Sicherheitsschicht auf dem Linux-Kernel mit MAC (Mandatory Access Control)
- Diese zusätzliche Sicherheitsschicht schaut, ob ein Prozess oder ein Benutzer auf eine Datei, Port o. Ä. zugreifen darf

## In einem Satz

"May {subject} do {action} to {object}? For example: May a web server access files in users' home directories?" - Red Hat

Ein lokal gehosteter Webserver, der direkt auf einem Linux-System läuft (also nicht in einem Container), hat standardmäßig Zugriff auf die Datei `/etc/passwd`, sofern die Dateiberechtigungen dies zulassen. Und das tun sie in der Regel (standardmäßig Permission gesetzt auf `r`).

Bei einem kaputten PHP-Skript, welches kompromittiert wurde, könnte man zum Beispiel dann `/etc/passwd` eventuell auslesen, was eine Sicherheitslücke darstellt.

## Was macht SELinux?

Systemressourcen (Dateien, Sockets, Ports, etc.) und Prozesse werden gelabelt und bekommen dadurch einen Kontext zugewiesen. Dazu gibt es dann ein Regelwerk (=Sammlung von Regeln), welches Aktionen erlaubt.

## Praxis

Umgebung:
- Root-Rechte
- AlmaLinux 9.5

Umgebung vorbereiten:

```cli
# Installiert Web-Server
dnf install httpd -y
# Erlaubt der Firewall auf Port 80 zuzugreifen
firewall-cmd --add-service=http
# Startet Web-Server
systemctl enable --now httpd
```

### Status überprüfen und setzen

Insgesamt gibt es 3 verschiedene States, welche SELinux haben kann.
- Enforcing: Läuft. Wenn ein System auf eine Ressource zugreifen will, dann kann er dieses blockieren.
- Permissive: Hört im Hintergrund mit, aber blockiert nichts.
- Disabled: Hört im Hintergrund nicht mit und blockiert auch nichts.

SELinux Status überprüfen:

```cli
getenforce
```

SELinux Status setzen (wird bis zum Reboot beibehalten, dabei steht `0` für 'Permissive' und
`1` für 'Enforcing'):

```cli
setenforce <0 or 1>
```

SELinux Status setzen, damit die Daten auch nach einem Reboot erhalten bleiben:

```cli
# Zeile mit SELINUX=<Statusname> suchen und zum gewünschten Status ändern
vim /etc/selinux/config
# Nach einem Reboot wird dann die Einstellung aktiv
```

### Kontexte

Es gibt den Befehl `ls -l`, welcher alle Dateien mit detaillierten Informationen anzeigt. Um jetzt Kontexte mit anzuzeigen benutzt man den Befehl `ls -lZ`. Der Kontext selbst besteht aus 4 Elementen, welche jeweils durch ein `:` abgetrennt sind.

- Erstes Element: User
- Zweites Element: Rolle
- Drittes Element: Type
- Viertes Element: MLS (Multi-Level Security)

Mit `ps -ef` kann man sich alle Prozesse detailliert anzeigen lassen. Das `-e` zeigt alle Prozesse (auch anderer Benutzer) und `-f` liefert eine erweiterte Ausgabe mit zusätzlichen Infos. Mit `ps -efZ` werden zusätzlich noch die Sicherheitskontexte mit angezeigt.

Mit `ss -lntp` können Socket Statistics, also offene Netzwerkverbindungen wie IP-Adressen, Ports, usw. angezeigt werden. Es ist der moderne Nachfolger von `netstat`, wobei `-l` dabei nur die lauschenden Ports anzeigt, `-n` nur die numerische Ausgabe ausgibt (keine DNS-Auflösung), `-t` nur TCP-Verbindungen anzeigt und `-p` zeigt Prozessinfos (PID + Name). Mit `ss -lntpZ` werden auch hier zusätzlich noch die Sicherheitskontexte mit angezeigt.

### Debugging: Beispiel 1

Beim Webserver wird DocumentRoot auf den Pfad `/srv/test` verlegt. Das kann man in dieser Datei anpassen:

```cli
vim /etc/httpd/conf/httpd.conf
```

Unter dem Pfad befindet sich eine Index.html-Datei mit dem Inhalt "Hallo Welt". Mit einem Server-Neustart des Webservers kann man dann überprüfen ob sich die Seite dann auch wirklich noch anzeigen lässt:

```cli
systemctl restart httpd
```

Die neue Index-Seite wird allerdings nicht angezeigt. Aus diesem Grund schaut man dann ins Log mit dem Befehl `tail -f /var/log/httpd/error_log`. Dort sieht man dann die Fehlermeldung 'Permission denied'. Oft wird deshalb auch einfach empfohlen, SELinux mit `setenforce 0` zu deaktivieren, was allerdings nicht die beste Option ist. Selbst wenn man mit dem Befehl `stat /srv/test/index.html` oder `stat /srv/test` sich die Rechte ansieht, dann wird dort trotzdem angezeigt dass beide auf lesen gesetzt sind. Auch, wenn man nun aus Verzweiflung `chmod -R 0777 /srv/test/` versuchen würde (`-R` steht für recursive), was man niemals machen sollte, würde es nicht gehen.

SELinux blockiert diesen Zugriff also. Um diesen Zugriff allerdings nun freizugeben, kann man sich zunächst mit dem Befehl anschauen, was genau blockiert wird:

```cli
ls -lZ /srv/test/
```

Als Ergebnis wird beim Kontext-Typen `var_t` angezeigt und Apache kann offensichtlich mit `httpd` darauf nicht zugreifen. Man kann sich das mit dem Tool `ausearch` angucken. Mit ihm kann man den Audit-Log (Pfad: `/var/log/audit/audit.log`) durchsuchen. Das geht folgendermaßen:

```cli
ausearch -m avc -ts recent
```

`-m` beschreibt den Message-Type und "avc" steht hier für "Access Vector Cache" (Komponente von SELinux, welche die Regeln bearbeitet). `-ts` ist der Time-Start und der liegt bei "recent" bei 10 Minuten.

#### Nach bestimmten Prozess suchen

Angenommen, es bereitet nun ein bestimmter Prozess Probleme, also z. B. in diesem Fall `httpd`. Dann kann man mit dem folgenden Befehl herausfinden wo genau dieser Prozess genau liegt:

```cli
which httpd
```

Das kann man dann in den ausearch-Befehl mit einbauen und dann sieht man auch nur die Fehler für diesen Prozess:

```cli
ausearch -m avc -x /sbin/httpd -ts recent
```

Mit dem nachfolgenden Befehl kann man dann die Kontexte im Standardverzeichnis  ansehen:

```cli
ls -lZa /var/www/html/
```

Das Ergebnis dieses Befehls bei dem Kontext an dritter Stelle ist nun bei beiden Einträgen `httpd_sys_content_t`. Damit wir bei dem Verzeichnis `/sbin/httpd` das auch auf diesen Wert setzen können, müssen wir folgendes ausführen:

```cli
# -R steht hier für Rekursiv und -t für den SELinux-Typ
chcon -R -t httpd_sys_content_t /srv/test/
```

Temporär funkioniert es jetzt so. Wenn man allerdings Probleme mit dem Filesystem bekommt, dann kann es zu einem Relabeling führen und dann setzt SELinux die Einstellungen auch wieder zurück. Aus diesem Grund muss man die Eistellungen auch der SELinux-Datenbank auch klar machen. Die Datenbank kann man so einsehen:

```cli
semanage fcontext -l
```

Man kann mit dem folgenden Befehl alle `httpd`-Einträge einsehen in dieser SELinux-Datenbank:

```cli
semanage fcontext -l | grep httpd
```

Um nun es permanent auch nach einem Relabeling verfügbar zu machen, macht man folgendes:

```cli
semanage fcontext -a -t httpd_sys_content_t "/srv/test(/.*)?"
```

#### Relabeling

Hätte man jetzt mit `semanage fcontext -a -t httpd_sys_content_t "/srv/test(/.*)?"` den Eintrag nicht in die SELinux Datenbank gemacht, und man hätte ein Relabeling des Filesystems durchgeführt:

```cli
restorecon -Rv *
```

...dann würde es die ganzen Konfigurationen auf die Standard-Kontextwerte aus der SELinux-Datenbank zurücksetzen.

### Debugging: Beispiel 2

Normalerweise, wenn man einen Port ändern möchte für z. B. SSH, dann macht man es über:

```cli
vim /etc/ssh/sshd_config
# Port auf z. B. 1337 setzen im Editor
systemctl restart sshd
firewall-cmd --add-port=1337/tcp
firewall-cmd --runtime-to-permanent
```

Wenn SELinux an ist, dann kommt nach dem Systemneustart allerdings die Fehlermeldung, dass der Prozess Error bringt.

Man kann sich die Ereignisse dann mit dem folgenden Befehl anzeigen lassen:

```cli
journalctl -u sshd
```

SELinux-Ports kann man sich mit folgendem Befehl anzeigen lassen:

```cli
semanage port -l
```

Man ändert nun in SELinux den Port mit:

```cli
semanage port -a -t ssh_port_t -p tcp 1337
systemctl restart sshd
```

Mit diesem Befehl sieht man dann auch ob die Einstellungen dann auch wirklich gesetzt worden sind:

```cli
ss -lntpZ
```
