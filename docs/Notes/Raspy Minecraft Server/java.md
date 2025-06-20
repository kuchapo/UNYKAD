# Java initialisieren

1. auf Webseite https://www.oracle.com/java/technologies/downloads/ gehen
2. Link von `ARM64 Compressed Archive` kopieren
3. im Terminal `sudo wget <Hier den Link einfügen>` ausführen

## Java dauerhaft verfügbar machen

`.bashrc`-Datei öffnen:

```bash
nano ~/.bashrc
```

Ganz unten einfügen:

```bash
export JAVA_HOME=~/jdk-24.0.1
export PATH=$JAVA_HOME/bin:$PATH
```

Dann aktivieren mit:

```bash
source ~/.bashrc
```