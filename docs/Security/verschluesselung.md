# Verschlüsselung

Texte werden durch das Verändern (ersetzen) und verschieben von Buchstaben verschlüsselt.

## Arten der Verschlüsselung

### Systematische Verschlüsselung

1 Schlüssel zum Verschlüsseln und Entschlüsseln

Problem: Schlüssel muss sicher zum Empfänger übertragen werden.

Vorteil: schnell und einfach

### Asymmetrische Verschlüsselung

Erzeuger erzeugt mit einem Programm ein Schlüsselpaar. Ein öffentlicher Schlüssel mit dem nur verschlüsselt werden kann. Diese Verschlüsselung wird zum Beispiel auf Servern jedem zur Verfügung gestellt der dem Empfänger etwas schicken will. Ein privater Schlüssel mit dem entschlüsselt werden kann, verlässt niemals den Empfänger.

Der private Schlüssel kann aus dem öffentlichen Schlüssel nicht berechnet werden.

Sender: holt sich den öffentlichen Schlüssel des Empfängers und verschlüsselt die Nachricht damit. Der Sender schickt die Nachricht an den Empfänger.

Empfänger: stellt öffentlichen Schlüssel auf Server. Entschlüsselt Nachricht mit dem privaten Schlüssel.

Vorteil: Problem der Schlüsselübergabe entfällt.

Nachteil: kompliziertes mathematisches Verfahren bis 10000 Mal langsamer als symmetrische Verschlüsselung.

### Hybride Verschlüsselung

Beispiele: SSL & TSL

Nur der symmetrische Schlüssel wird asymmetrisch verschlüsselt zum Empfänger übertragen. Die Nachricht wird symmetrisch verschlüsselt übertragen.

Vorteile: schnell, da symmetrische Schlüssel kurz sind und sichere Schlüsselübertragung
Nachteile: keine

### Einwegverschlüsselung

Zeichenfolge wird verschlüsselt, aus den verschlüsselten Zeichen kann das Original nicht mehr hergestellt werden.

Zur Verschlüsselung von Passwörtern, Hashes

## Caeser

| Wort  | Beispiel | Verschlüsselung                                                            |
| ----- | -------- | -------------------------------------------------------------------------- |
| hallo | kdoor    | Schlüssel C (3 Buchstaben nach hinten vom Alphabet her)                    |
| hallo | keopr    | Schlüssel CD (erster Buchstabe 3 nach hinten, zweiter 4 nach hinten, usw.) |
