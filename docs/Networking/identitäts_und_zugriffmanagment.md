# Identitäts und Zugriffsmanagment (IAM)

Identity and Access Managment (IAM) ist ein Sicherheits- und Verwaltungsprozess, der sicherstellt, dass die richtigen Personen (Identität) auf die richtigen Ressourcen (Zugriff) zur richtigen Zeit und unter den richtigen Bedingungen zugreifen können.

AD und Entra ID benutzen ebenso IAM. Hier ist ein entsprechendes Beispiel aus dem AD:

| Identifizierung                                      | Authentifizierung                                                       | Autorisierung |
|------------------------------------------------------|-------------------------------------------------------------------------|---------------|
| Feststellung der Identität einer Person/Objekts      | Akt des Nachweises, z.B. Abgleich des eingegebenen Passworts mit der DB | Gewähren von Zugangsrechten/Privilegien zu Ressourcen                  |
| -> Eindeutige Kennung, wird im Directory gespeichert | -> Identitätsnachweis durch etwas, was man besitzt, weiß oder ist (wenn mind. zwei Sachen erfüllt werden, spricht man von "MFA") | -> auf welche Ressourcen darf zugegriffen werden und wieviel Zugriff darf gewährt werden? |
