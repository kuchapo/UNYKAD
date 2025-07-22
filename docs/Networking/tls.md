# Sichere Verbindung mit TLS

TLS Transport Layer Security (früher SSL, SSL 3.1 = TLS 1.0)

3 Kriterien für eine sichere Verbindung

- Authentizität: Kommunikationspartner ist wirklich der, der er behauptet zu sein (Passwörter, biometrisch, Zertifikate, ...)
- Integrität: Nutzdaten werden auf ihrem Weg nicht verändert (Hashwerte, Verschlüsselung)
- Vertraulichkeit: Daten können nur von Befugten eingesehen werden (Verschlüsselung)

WICHTIG: Kriterien für eine sichere Verbindung nicht verwechseln mit SCHUTZZIELEN!

## 1. Authentizität bei TLS durch Zertifikate

![CA](./assets/ca.drawio.svg)

Zertifikat beinhaltet unter anderem:

- für wen ausgestellt
- CA die es ausgestellt hat
- Gültigkeit ab und bis
- öffentlicher Schlüssel des Servers

3 Klassen von Zertifikaten:

- Domain Validated
- Organisation Validation
- Extended Validation

# 2. Integrität und Vertraulichkeit durch Verschlüsselung

![CA](./assets/ca2.svg)
