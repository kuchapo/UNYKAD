# Hardlinks und Softlinks

Ein Hardlink zeigt auf denselben Inode wie die Datei. Ein Softlink (auch Symlink oder Symbolic link genannt) zeigt auf den Pfad der Datei.
Das Risiko beim Softlink ist, dass wenn die Originaldatei gelöscht oder verschoben wird, der Softlink ins Leere verweist.

```bash
ln <Dateiname> <Dateiname der Verknüpfung>		# erstellt einen Hardlink -> Inodes-Nummer identisch bei beiden Dateien
ln -s <Dateiname> <Dateiname der Verknüpfung>		# erstellt einen Softlink -> Inodes-Nummer nicht identisch bei beiden Dateien
```
