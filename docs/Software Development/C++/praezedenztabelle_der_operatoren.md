# Präzedenztabelle der Operatoren

| **Bezeichnung**          | Operatorsymbol                     | Priorität | Bewertungsreihenfolge |
| ------------------------ | ---------------------------------- | --------- | --------------------- |
| Klammern                 | () []                              | 14        | Von links nach rechts |
| Komponentenauswahl       | . ->                               | 14        | Von links nach rechts |
| Arithmetische Negation   | -                                  | 13        | Von rechts nach links |
| Logische Negation        | !                                  | 13        | Von rechts nach links |
| Bitlogische Negation     | ~                                  | 13        | Von rechts nach links |
| Inkrement                | ++                                 | 13        | Von rechts nach links |
| Dekrement                | --                                 | 13        | Von rechts nach links |
| Arithmetische Operatoren | * / %                              | 12        | Von links nach rechts |
|                          | + -                                | 11        | Von links nach rechts |
| Shift-Operatoren         | << >>                              | 10        | Von links nach rechts |
| Vergleichsoperatoren     | > >= < <=                          | 9         | Von links nach rechts |
|                          | == !=                              | 8         | Von links nach rechts |
| Bit-Operatoren           | &                                  | 7         | Von links nach rechts |
|                          | ^                                  | 6         | Von links nach rechts |
|                          | \|                                 | 5         | Von links nach rechts |
| Logische Operatoren      | &&                                 | 4         | Von links nach rechts |
|                          | \|\|                               | 3         | Von links nach rechts |
| Zuweisungsoperatoren     | = += -= *= /= %= >>= <<= &= ^= \|= | 2         | Von rechts nach links |
| Sequenzoperator          | ,                                  | 1         | Von rechts nach links |
