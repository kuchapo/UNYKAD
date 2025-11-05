# DNS – Domain Name System

- Dient der Namensauflösung
- Benutzer kennt Name der Webseite und bekommt vom DNS Server die zugehörige IP Adresse

Baumstruktur tabellarisch zusammengefasst:

| Ebene                               | Bezeichnung                                                 |
| ----------------------------------- | ----------------------------------------------------------- |
| `\`                                 | Root Level -> Weltweit nur 13 gespiegelte Root Level Server |
| `.net`, `.com`, `.de`, ...          | Top Level Domain                                            |
| `unykad`, `facebook`, `google`, ... | Second Level Domain                                         |
| `intern`, `docs`, ...               | Sub Level Domain (3. Level)                                 |
| `www`                               | Hostname oder Dienst, Sub Level Domain (3. Level und höher) |

## URLs

- Beispiel: http://www.docs.unykad.com
- steht für Uniforn Ressource Locator
- wird von rechts nach links gelesen
    - beginnt mit der `Top Level Domain` (organisatorische wie com, org, net, … oder Ländercodes wie de, at, fr, …)
    - `Second Level Domain` gehört einer Person oder Organisation, sie ist durch das Marken- und Namensrecht geschützt und muss (in Deutschland) bei der DENIC beantragt werden
    - es kann mehrere Sub Level Domains geben
    - für Subdomains wie "docs.unykad.com" gibts kein automatischen Subdomain-Präfix `www` wie oft bei den Second Level Domains, das heißt, dass "www.docs.unykad.com" zum Beispiel nicht zwangsweise funktionieren muss
- hinter jede URL gehört eigentlich noch ein Punkt für das Root Level, da das aber überall gleich ist und damit kein Unterscheidungsmerkmal ist, wird dieser weggelassen

## Ablauf

Client → rekursiver Resolver
Resolver (falls kein Cache) → Root (Startpunkt)
Root → Referral zu TLD an Resolver
TLD → Referral zu authoritativen NS der Zone an Resolver
Autoritativer NS → endgültige Antwort
Resolver cached, antwortet dem Client.
