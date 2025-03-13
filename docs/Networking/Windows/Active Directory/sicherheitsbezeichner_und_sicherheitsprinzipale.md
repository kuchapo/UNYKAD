# Sicherheitsbezeichner und Sicherheitsprinzipale

## Sicherheitsbezeichner (SID)

Ein **Sicherheitsbezeichner (SID)** (Englisch: Security Identifier) ist ein eindeutiger Wert, der verwendet wird, um Sicherheitsprinzipale oder Sicherheitsgruppen in Windows zu identifizieren. Jeder Benutzer, jede Gruppe und jedes Objekt erhält eine SID, die während der gesamten Lebensdauer unverändert bleibt.

### Beispiel

Wenn sich ein Benutzer anmeldet, dann erhält dieser im Hintergrund vom Domain Controller (also vom AD) ein Token, welcher unter anderem den Security Identifier (SID) beinhaltet mithilfe dessen der User dann identifiziert und in verschiedene Gruppen zugewiesen wird.

## Sicherheitsprinzipale

Ein **Sicherheitsprinzipal** (Englisch: Security Principal) ist eine Entität, die vom Betriebssystem authentifiziert werden kann, wie z. B. ein Benutzerkonto, ein Computerkonto oder eine Sicherheitsgruppe. Sicherheitsprinzipale werden verwendet, um den Zugriff auf Ressourcen zu steuern und Berechtigungen zu verwalten.

---

Wichtig: Ein Sicherheitsprinzipal, wie z. B. ein Benutzerkonto, wird nur dann als solcher betrachtet, wenn ihm eine SID (Sicherheitsbezeichner) zugewiesen ist.
