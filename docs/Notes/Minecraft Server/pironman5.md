# Pironman 5

## Hilfe & Konfiguration ausgeben

```bash
pironman5 -h | --help            # Zeigt alle Optionen
```

```bash
pironman5 -c | --config          # Zeigt aktuelle Konfiguration (JSON-Format)
```

## RGB-LED-Steuerung

### Art der Leuchtung einstellen

=== "solid"
    ```bash
    sudo pironman5 -rs solid
    ```

=== "breathing"
    ```bash
    sudo pironman5 -rs breathing
    ```

=== "flow"
    ```bash
    sudo pironman5 -rs flow
    ```

=== "flow_reverse"
    ```bash
    sudo pironman5 -rs flow_reverse
    ```

=== "rainbow"
    ```bash
    sudo pironman5 -rs rainbow
    ```

=== "rainbow_reverse"
    ```bash
    sudo pironman5 -rs rainbow_reverse
    ```

=== "hue_cycle"
    ```bash
    sudo pironman5 -rs hue_cycle
    ```

### Farbe einstellen

=== "Rot"
    ```bash
    sudo pironman5 -rc ff0000
    ```

=== "Grün"
    ```bash
    sudo pironman5 -rc 00ff00
    ```

=== "Blau"
    ```bash
    sudo pironman5 -rc 0000ff
    ```

=== "Gelb"
    ```bash
    sudo pironman5 -rc ffe100
    ```

=== "Magenta"
    ```bash
    sudo pironman5 -rc ff00ff
    ```

=== "Cyan"
    ```bash
    sudo pironman5 -rc 00ffff
    ```

=== "Weiß"
    ```bash
    sudo pironman5 -rc ffffff
    ```

=== "Lila"
    ```bash
    sudo pironman5 -rc 800080
    ```

=== "Türkis"
    ```bash
    sudo pironman5 -rc 00ffaa
    ```

=== "Orange"
    ```bash
    sudo pironman5 -rc ff8800
    ```

### Sonstige Befehlsparameter

- -rb = Helligkeit (0–100)
- -rp = Geschwindigkeit (0–100)
- -re = LED ein/aus (true/false)
- -rl = Anzahl der LEDs (z. B. 4)

## Lüfter steuerung

=== "Always on"
    ```bash
    sudo pironman5 -gm 0
    ```

=== "Ab 50 °C"
    ```bash
    sudo pironman5 -gm 1
    ```

=== "Ab 60 °C"
    ```bash
    sudo pironman5 -gm 2
    ```

=== "Ab 67.5 °C"
    ```bash
    sudo pironman5 -gm 3
    ```

=== "Ab 70 °C"
    ```bash
    sudo pironman5 -gm 4
    ```

## Um Änderungen wirksam zu machen

```cli
sudo pironman5 restart
```
