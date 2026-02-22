# Repositories

```bash
cd /etc/yum.repos.d/										# redhat.repo -> abhängig von Subscription freigeschalteten Red Hat Repositories
sudo subscription-manager repos --enable codeready-builder-for-rhel-9-$(arch)-rpms		# aktiviert das Repo CodeReady Builder, welches für EPEL notwendig ist
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm		# installiert EPEL-Repo
ls												# 3 neue Repos, epel.repo ist für upstream repository mirrors verantwortlich
sudo dnf install htop										# installiert htop

sudo nano mariadb.repo										# selbst Repo erstellen unter /etc/yum.repos.d/
 	[mariadb]
 	name = MariaDB
 	baseurl = https://downloads.mariadb.com/MariaDB/mariadb-10.5/yum/rhel/9/x86_64/
 	gpgkey = https://downloads.mariadb.com/MariaDB/mariadb-10.5/gpgkey
 	gpgcheck = 1
 	enabled = 1
sudo dnf makecache										# dnf holt sich das neue Repo damit
sudo dnf list available --repo=mariadb								# zeigt alle verfügbaren Packages vom neuen Repo an
sudo dnf list MariaDB-server --showduplicates --repo=mariadb					# zeigt alle verfügbaren Versionen für dieses Repo
sudo dnf install MariaDB-server-10.5.28-1.el9 --repo=mariadb					# nötigen Abhängigkeiten nicht verfügbar/aktiv -> kann fehlschlagen
```
