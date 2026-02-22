# **Disk Partition**

```bash
lsblk											# zeigt alle erknannten Laufwerke; sr0 wurde benutzt um die rhel-iso Datei zu hosten; in "rhel-root" ist das Root-Dateisystem
df -h											# Dateisysteme; gemountete Dateisysteme
```

## Laufwerkbenutzung eines Benutzers einschränken

```bash
umount /mnt/mydata2									# erst unmounten
tune2fs -O quota /dev/mapper/myvg-shrinklv						# fügt das quota-feature dem Dateisystem hinzu
tune2fs -l /dev/mapper/myvg-shrinklv | grep features					# zeigt alle hinzugefügten Features
nano /etc/fstab										# hier muss man die hinzugefügten Features aktivieren
	/dev/myvg/shrinklv /mnt/mydata2 ext4 defaults,quota 0 0
quotaon -v /mnt/mydata2									# schaltet quota-Überwachung wirklich ein; im Pfad werden nun binäre Files erstellt
edquota -u user2									# hier werden die Limits für den Benutzer gesetzt
edquota -g group2									# hier werden die Limits für die Gruppe gesetzt
edquota -t										# legt fest wie lange jemand das Soft-Limit überschreiten darf, bevor die Nutzung eingefroren wird
```

## Neue Laufwerke erstellen

```bash
sudo dnf install qemu-img								# Laufwerk-Partitionierungs-Tool
sudo qemu-img create -f qcow2 /var/lib/libvirt/images/rhel9-server-disk2.qcow2 2G	# qcow = qemo copy on write; erstellt virtuelles Laufwerk durch dynamisches Ausleihen der Speicherressourcen bis 2 GB
ls -lh /var/lib/libvirt/images/rhel9-server-disk2.qcow2
sudo dnf install libvirt								# Paket für die Verwaltung von Virtualisierungstechnologien wie z. B. KVM
virsh list --all									# zeigt alle VMs an
virsh attach-disk rhel9-server /var/lib/libvirt/images/rhel9-server-disk2.qcow2 vdb --driver qemu -- subdriver qcow2 --targetbus virtio --persistent	# Name der VM 'rhel9-server'
virsh domblklist rhel9-server																# zeigt alle Blockgeräte der VM
lsblk																			# zeigt neues Laufwerk 'vdb' an
mkfs.ext4 /dev/vdb																	# formatiert das Laufwerk; ext4 = universell + gut für kleine Dateien
mkdir -p /mnt/newdrive																	# mit '-p' werden fehlende Unterordner erstellt
mount /dev/vdb /mnt/newdrive																# mountet das Dateisystem in den mnt-Pfad
nano /etc/fstab
	/dev/vdb /mnt/newdrive ext4 defaults 0 0	# sorgt dafür, dass das Laufwerk wärend dem Boot bereits geladen wird; erste 0 deaktiviert automatische Backups; zweite 0 deaktiviert Filechecking beim Boot
mount -a						# lässt durch's Remounten die Änderungen wirksam werden aus der fstab-Datei
ls -l /mnt/newdrive/					# 'lost-found' bedeutet, dass es das Root-Verzeichnis der Partition ist im ext4-Format
```

## Partition erstellen mit fdisk

```bash
fdisk /dev/vdc						# öffnet neue CLI
	n						# Lässt den Typ der Partition auswählen: Primary oder Extended (Extended sind Container, falls man 4 dieser Primariys erreicht hat)
	p						# man kann maximal 4 Mal Primary haben, also echte Partitionen haben pro Laufwerk
	1						# oder Leerzeichen drücken
	2048						# Das ist der First sector (wo die Partition beginnt)
	4194303						# Das ist der Last sector (wo die Partition endet)
	w						# write; speichert die Änderungen
mkfs.xfs /dev/vdc1					# erstellt die Formatierung; xfs ist gut für große Dateien und hohe IO-Last mit parallelen Prozessen
mkdr -p /mnt/newdrive2
mount /dev/vdc1 /mnt/newdrive2
nano /etc/fstab
	/dev/vdc1 /mnt/newdrive2 xfs defaults 0 0
mount -a
```

## Partition erstellen mit parted

```bash
parted /dev/vdc
mklabel gpt						# bestehende Partitionen werden gelöscht und es wird eine neue GPT („GUID Partition Table) erstellt
mkpart primary 1MiB 100%				# 1MiB und nicht 0MiB, damit man nicht mit der Disk-Metadaten überschneidet
mkfs.ext4 /dev/vdc1
mount /dev/vdc1 /mnt/newdrive2
nano /etc/fstab
	/dev/vdc1 /mnt/newdrive2 ext4 defaults 0 0
mount -a
```
