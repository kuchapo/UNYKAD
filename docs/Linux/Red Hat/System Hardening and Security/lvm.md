# LVM

PV -> VG -> LV

```bash
pvs							# zeigt Physical Volumes an
pvcreate /dev/sdb1					# erzeugt eine neue Physical Volume; klappt nur wenn es nicht benutzt wird bzw. nicht gemounted ist ('sudo umount /dev/sdb1')
pvdisplay /dev/sdb1
pvs --segments						# zeigt unter anderem die Volume Gruppen
vgs							# zeigt Übersicht aller Volume Gruppen
vgcreate myvg /dev/sdb1					# erstellt neue Volume Gruppe; hier kann man auch mehrere PV's einer VG zuweisen; man kann auch mehrere VG's haben
lvs							# zeigt alle Logical Volumes an
lvdisplay						# zeigt mehr Details an
lvcreate -L 1G -n mylv myvg				# erstellt eine neue LV; VG wird der LV zugewiesen
mkfs.ext4 /dev/myvg/mylv
mkdir /mnt/mydata
mount /dev/myvg/mylv /mnt/mydata/
nano /etc/fstab						# macht Änderungen persistent; man muss die neue Festplatte einbinden

lvcreate -L 900M -n shrinklv myvg			# ändert die Größe eines LVM
mkfs.ext4 /dev/myvg/shrinklv
tune2fs -m 1 -L shrinktest /dev/myvg/shrinklv		# reserviert 1% der gesamten 900M, falls Speicherplatz voll ist und man nichts mehr benutzen kann ('lsblk -f' zeigt Labels an)
systemctl daemon-reload
```

## LVM-Größe ändern

```bash
df -h
umount /mnt/mydata2
fsck -f /dev/myvg/shrinklv				# prüft, ob es irgendwelche Fehler wirft oder nicht
resize2fs /dev/myvg/shrinklv 500M			# ändert die Größe des Dateisystems
lvreduce -L 500M /dev/myvg/shrinklv			# verkleinert die Größe der eigentlichen Logical Volume
lvextend -L +400M /dev/myvg/shrinklv			# vergrößert die Größe der eigentlichen Logical Volume; ohne 'resize'
mount /dev/myvg/shrinklv /mnt/mydata2
```

## LVM löschen

```bash
unmount /mnt/mydata2
lvremove /dev/myvg/shrinklv
vgremove myvg
pvremove /dev/sdb1
```
