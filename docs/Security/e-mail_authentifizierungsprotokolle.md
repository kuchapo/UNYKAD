# E-Mail-Authentifizierungsprotokolle

- SPF prüft, ob eine E-Mail von einem autorisierten Server gesendet wurde.

    Beispiel für einen SPF-Eintrag:  
    `v=spf1 ip4:192.168.0.1 -all`

- DKIM stellt sicher, dass eine E-Mail nicht verändert wurde und vom angegebenen Absender stammt.

    Beispiel für einen DKIM-Eintrag:  
    `default._domainkey.example.com IN TXT "v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GN..."`

- DMARC gibt an, wie E-Mails behandelt werden sollen, die SPF oder DKIM nicht bestehen, und ermöglicht Berichterstattung.

    Beispiel für einen DMARC-Eintrag:  
    `_dmarc.example.com IN TXT "v=DMARC1; p=reject; rua=mailto:dmarc-reports@example.com"`

## Gründe, warum SPF alleine nicht ausreicht

1. Forwarding:

    - Wenn eine E-Mail weitergeleitet wird, ändert sich der Absender-Server, aber der SPF-Eintrag bleibt auf die ursprüngliche Domain bezogen. Der empfangende Server kann die SPF-Prüfung nicht bestehen, weil der Forwarding-Server nicht im SPF-Eintrag der ursprünglichen Domain steht.

2. SPF-Pass sagt nichts über den Inhalt der E-Mail aus:

    - SPF bestätigt nur, dass die E-Mail von einem autorisierten Server gesendet wurde. Es sagt nichts darüber aus, ob der Inhalt der E-Mail authentisch oder unverändert ist.

3. Fehlende Verschlüsselung und Integrität:

    - SPF prüft nur die IP-Adresse des sendenden Servers. Es stellt nicht sicher, dass der Inhalt der E-Mail während der Übertragung nicht verändert wurde.

4. Absenderadresse (Envelope From) und From-Header können unterschiedlich sein:

    - SPF überprüft die "Envelope From" Adresse, die in der SMTP-Kommunikation verwendet wird. Diese Adresse kann sich von der "From"-Adresse, die der Benutzer im E-Mail-Client sieht, unterscheiden. Daher kann eine E-Mail SPF bestehen, obwohl die sichtbare Absenderadresse gefälscht ist.
