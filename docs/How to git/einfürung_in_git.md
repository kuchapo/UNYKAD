# Einführung in Git

**Git** ist ein verteiltes Versionskontrollsystem, das von Entwicklern verwendet wird, um den Verlauf von Änderungen an Dateien zu verfolgen und gemeinsam an Projekten zu arbeiten. Es ermöglicht die Verwaltung von Quellcode und erleichtert die Zusammenarbeit in Teams, indem es verschiedene Versionen eines Projekts verwaltet und es Benutzern ermöglicht, Änderungen rückgängig zu machen, Branches zu erstellen und zusammenzuführen.

## Wichtige Begriffe

- **Repository (Repo)**: Ein Ordner, der alle Dateien und den Verlauf eines Projekts enthält.
- **Commit**: Ein gespeicherter Schnappschuss des aktuellen Zustands der Projektdateien.
- **Branch**: Eine separate Entwicklungslinie innerhalb eines Repositories.
- **Merge**: Das Zusammenführen von Änderungen eines Branches in einen anderen.
- **Staging Area (Index)**: Ein Zwischenspeicher für Änderungen, die zum Commit bereit sind.
- **Remote Repository**: Ein Repository, das auf einem Server gehostet wird, z.B. auf GitHub.
- **Pull**: Das Abrufen und Zusammenführen von Änderungen aus einem Remote-Repository.
- **Push**: Das Hochladen von lokalen Commits in ein Remote-Repository.
- **Clone**: Das Kopieren eines Remote-Repositories auf den lokalen Computer.
- **Checkout**: Das Wechseln zu einem anderen Branch oder Commit.

## Wichtige Git-Befehle

- `git init`: Erstellt ein neues Git-Repository in einem Verzeichnis.
- `git clone [URL]`: Klont ein bestehendes Repository von einer URL in ein neues Verzeichnis.
- `git status`: Zeigt den aktuellen Status des Arbeitsverzeichnisses und der Staging-Area an.
- `git add [Datei/Verzeichnis]`: Fügt eine Datei oder ein Verzeichnis zur Staging-Area hinzu.
- `git commit -m "[Nachricht]"`: Erstellt einen Commit mit den Dateien in der Staging-Area und fügt eine Commit-Nachricht hinzu.
- `git commit --amend`: Ändert den letzten Commit (z.B. um die Commit-Nachricht zu bearbeiten).
- `git log`: Zeigt die Commit-Historie des aktuellen Branches an.
- `git log --oneline`: Zeigt die Commit-Historie im kompakten Format (eine Zeile pro Commit).
- `git diff`: Zeigt die Änderungen zwischen dem Arbeitsverzeichnis und der Staging-Area an.
- `git diff --staged`: Zeigt die Änderungen an, die zur Staging-Area hinzugefügt wurden, aber noch nicht committet sind.
- `git branch`: Zeigt alle lokalen Branches an.
- `git branch [Name]`: Erstellt einen neuen Branch mit dem angegebenen Namen.
- `git checkout [Branch]`: Wechselt zu einem anderen Branch.
- `git checkout -b [Name]`: Erstellt und wechselt zu einem neuen Branch mit dem angegebenen Namen.
- `git merge [Branch]`: Mergt den angegebenen Branch in den aktuellen Branch.
- `git rebase [Branch]`: Wendet die Commits des aktuellen Branches auf den angegebenen Branch neu an.
- `git stash`: Speichert uncommittete Änderungen vorübergehend, um mit einem sauberen Arbeitsverzeichnis zu arbeiten.
- `git stash pop`: Stellt die zuletzt gespeicherten Änderungen aus dem Stash wieder her und entfernt sie aus dem Stash.
- `git stash apply`: Stellt die zuletzt gespeicherten Änderungen aus dem Stash wieder her, ohne sie aus dem Stash zu entfernen.
- `git remote -v`: Zeigt die URLs aller Remote-Repositories an.
- `git remote add [Name] [URL]`: Fügt ein Remote-Repository mit dem angegebenen Namen hinzu.
- `git pull [Remote] [Branch]`: Holt die Änderungen von einem Remote-Repository und führt sie mit dem aktuellen Branch zusammen.
- `git fetch [Remote]`: Holt die Änderungen von einem Remote-Repository, ohne sie zu mergen.
- `git push [Remote] [Branch]`: Schiebt die lokalen Commits zum Remote-Repository.
- `git push -u [Remote] [Branch]`: Schiebt die lokalen Commits zum Remote-Repository und setzt den Remote-Branch als Tracking-Branch.
- `git reset --hard [Commit]`: Setzt das Arbeitsverzeichnis, die Staging-Area und den aktuellen Branch auf den angegebenen Commit zurück.
- `git reset --soft [Commit]`: Setzt den aktuellen Branch auf den angegebenen Commit zurück, behält jedoch die Änderungen in der Staging-Area bei.
- `git clean -f`: Entfernt nicht versionierte Dateien aus dem Arbeitsverzeichnis.
- `git tag [Name]`: Erstellt ein neues Tag für den aktuellen Commit.
- `git tag -d [Name]`: Löscht ein lokales Tag.
- `git push [Remote] --tags`: Schiebt alle lokalen Tags zum Remote-Repository.
