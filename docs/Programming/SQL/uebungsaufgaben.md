# Übungsaufgaben

Diese Übungsaufgaben kommen von der Webseite [SQL Island](https://sql-island.informatik.uni-kl.de/).

## Übung 1

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Zeige alle Dörfer an.

=== "Lösung"
	
	```
	SELECT * FROM DORF;
	```
	
	|dorfnr|name|haeuptling|
	|---|---|---|
	|1|Affenstadt|1|
	|2|Gurkendorf|6|
	|3|Zwiebelhausen|13|

## Übung 2

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Zeige alle Bewohner an mit dem Bruf Metzger.

=== "Lösung"
	
	```
	SELECT * FROM BEWOHNER WHERE beruf = 'Metzger';
	```
	
	|bewohnernr|name|dorfnr|geschlecht|beruf|gold|status|
	|---|---|---|---|---|---|---|
	|6|Gerd Schlachter|2|m|Metzger|4850|boese|
	|7|Peter Schlachter|3|m|Metzger|3250|boese|
	|17|Erich Rasenkopf|3|m|Metzger|990|friedlich|
	|19|Anna Flysh|2|w|Metzger|2280|friedlich|

## Übung 3

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Zeige alle friedlichen Bewohner an.

=== "Lösung"
	
	```
	SELECT * FROM BEWOHNER WHERE status = 'friedlich';
	```
	
	|bewohnernr|name|dorfnr|geschlecht|beruf|gold|status|
	|---|---|---|---|---|---|---|
	|1|Paul Backmann|1|m|Baecker|850|friedlich|
	|2|Ernst Peng|3|m|Waffenschmied|280|friedlich|
	|3|Rita Ochse|1|w|Baecker|350|friedlich|
	|4|Carl Ochse|1|m|Kaufmann|250|friedlich|
	|10|Peter Trommler|1|m|Schmied|600|friedlich|
	|12|Otto Armleuchter|2|m|Haendler|680|friedlich|
	|13|Fritz Dichter|3|m|Hoerbuchautor|420|friedlich|
	|15|Helga Rasenkopf|2|w|Haendler|680|friedlich|
	|17|Erich Rasenkopf|3|m|Metzger|990|friedlich|
	|18|Rudolf Gaul|3|m|Hufschmied|390|friedlich|
	|19|Anna Flysh|2|w|Metzger|2280|friedlich|

## Übung 4

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Zeige alle friedlichen Bewohner an, welche als Beruf Waffenschmied haben.

=== "Lösung"

	```
	SELECT * FROM BEWOHNER WHERE status = 'friedlich' AND beruf = 'Waffenschmied';
	```
	
	|bewohnernr|name|dorfnr|geschlecht|beruf|gold|status|
	|---|---|---|---|---|---|---|
	|2|Ernst Peng|3|m|Waffenschmied|280|friedlich|

## Übung 5

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Zeige alle friedlichen Bewohner an, welche als Beruf Schmied sind. Wie z. B.: Waffenschmied, Schmied, Hufschmied,...

=== "Lösung"
	
	```
	SELECT * FROM BEWOHNER WHERE status = 'friedlich' AND beruf LIKE '%schmied';
	```
	
	|bewohnernr|name|dorfnr|geschlecht|beruf|gold|status|
	|---|---|---|---|---|---|---|
	|2|Ernst Peng|3|m|Waffenschmied|280|friedlich|
	|10|Peter Trommler|1|m|Schmied|600|friedlich|
	|18|Rudolf Gaul|3|m|Hufschmied|390|friedlich|

## Übung 6

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Trage einen neuen Bewohner in Affenstadt ein.

=== "Lösung"
	
	```
	INSERT INTO BEWOHNER (name, dorfnr, geschlecht, beruf, gold, status) VALUES ('Karl', 1, '?', '?', 0, '?');
	```

## Übung 7

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Frage die bewohnernr von Karl ab.

=== "Lösung"
	
	```
	SELECT bewohnernr FROM BEWOHNER WHERE name = 'Karl';
	```
	
	| bewohnernr |
	| ---------- |
	| 20         |

## Übung 8

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wieviel Gold hat Karl?

=== "Lösung"
	
	```
	SELECT gold FROM BEWOHNER WHERE name = 'Karl';
	```
	
	|gold|
	|---|
	|0|

## Übung 9

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Liste alle Gegenstände auf, die niemandem gehören. Tipp: Herrenlose Gegenstände erkennt man an WHERE besitzer IS NULL.

=== "Lösung"
	
	```
	SELECT * FROM GEGENSTAND WHERE besitzer IS NULL;
	```
	
	|gegenstand|besitzer|
	|---|---|
	|Teekanne|null|
	|Ring|null|
	|Kaffeetasse|null|
	|Eimer|null|
	|Pappkarton|null|
	|Gluehbirne|null|

## Übung 10

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Karl trägt sich als Besitzer einer Kaffeetasse ein.

=== "Lösung"
	
	```
	UPDATE GEGENSTAND SET besitzer = 20 WHERE gegenstand = 'Kaffeetasse';
	```

## Übung 11

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Karl sammelt alle herrenlose Gegenstände auf einmal ein.

=== "Lösung"
	
	```
	UPDATE GEGENSTAND SET besitzer = 20 WHERE besitzer IS NULL;
	```

## Übung 12

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Welche Gegenstände besitzt nun Karl?

=== "Lösung"
	
	```
	SELECT * FROM GEGENSTAND WHERE besitzer = 20;
	```
	
	|gegenstand|besitzer|
	|---|---|
	|Teekanne|20|
	|Ring|20|
	|Kaffeetasse|20|
	|Eimer|20|
	|Pappkarton|20|
	|Gluehbirne|20|

## Übung 13

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Finde friedliche Bewohner mit dem Beruf Haendler oder Kaufmann. (Hinweis: Achte bei AND- und OR-Verknüpfungen auf korrekte Klammerung)

=== "Lösung"
	
	```
	SELECT * FROM BEWOHNER WHERE status = 'friedlich' AND beruf = 'Haendler' OR beruf = 'Kaufmann';
	```
	
	|bewohnernr|name|dorfnr|geschlecht|beruf|gold|status|
	|---|---|---|---|---|---|---|
	|4|Carl Ochse|1|m|Kaufmann|250|friedlich|
	|12|Otto Armleuchter|2|m|Haendler|680|friedlich|
	|15|Helga Rasenkopf|2|w|Haendler|680|friedlich|

## Übung 14

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Karl überträgt seinen Ring und die Teekanne an den mit der Bewohnernummer 15

=== "Lösung"
	
	```
	UPDATE GEGENSTAND SET besitzer = 15 WHERE gegenstand IN ('Ring', 'Teekanne');
	```
	
=== "Lösung 2"
	
	```
	UPDATE GEGENSTAND SET besitzer = 15 WHERE gegenstand = 'Ring';
	UPDATE GEGENSTAND SET besitzer = 15 WHERE gegenstand = 'Teekanne';
	```
	
## Übung 15

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Karl bekommt 120 Gold.

=== "Lösung"
	
	```
	UPDATE BEWOHNER SET gold = gold + 120 WHERE bewohnernr = 20;
	```

## Übung 16

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Karl bekommt 120 Gold.

=== "Lösung"
	
	```
	UPDATE BEWOHNER SET gold = gold + 120 WHERE bewohnernr = 20;
	```

## Übung 17

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Angenommen, in der Datenbank wäre ein Fehler und Karls Name wäre falsch geschrieben. Wie könnte man seinen Namen dann korrekt wieder auf Karl setzen?

=== "Lösung"
	
	```
	UPDATE BEWOHNER SET name = 'Karl' WHERE bewohnernr = 20;
	```

## Übung 18

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Zeige alle Bäcker an und sortiere die Anzeige dann absteigend danach, wieviel gold sie jeweils besitzen. Der reichste kommt also als zuerst.

=== "Lösung"
	
	```
	SELECT * FROM BEWOHNER WHERE beruf = 'Baecker' ORDER BY gold DESC;
	```
	
	|bewohnernr|name|dorfnr|geschlecht|beruf|gold|status|
	|---|---|---|---|---|---|---|
	|1|Paul Backmann|1|m|Baecker|850|friedlich|
	|9|Tanja Trommler|1|w|Baecker|550|boese|
	|3|Rita Ochse|1|w|Baecker|350|friedlich|

## Übung 19

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Karl bekommt 100 Gold.

=== "Lösung"
	
	```
	UPDATE bewohner SET gold = gold + 100 WHERE bewohnernr = 20;
	```

## Übung 20

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Karl bekommt ein brandneues Schwert als Gegenstand.

=== "Lösung"
	
	```
	INSERT INTO gegenstand (gegenstand, besitzer) VALUES ('Schwert', 20);
	```

## Übung 21

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Gibt es jemanden, der den Beruf Pilot hat?

=== "Lösung"
	
	```
	SELECT * FROM BEWOHNER WHERE beruf = 'Pilot';
	```
	
	|bewohnernr|name|dorfnr|geschlecht|beruf|gold|status|
	|---|---|---|---|---|---|---|
	|8|Arthur Schneiderpaule|2|m|Pilot|490|gefangen|

## Übung 22

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Dirty Dieter hält den Piloten gefangen. Wie findest du heraus in welchem Dorf Dirty Dieter wohnt?

=== "Lösung"
	
	```
	SELECT DORF.name FROM DORF, BEWOHNER WHERE DORF.dorfnr = BEWOHNER.dorfnr AND BEWOHNER.name = 'Dirty Dieter';
	```
	
	|name|
	|---|
	|Zwiebelhausen|

## Übung 23

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Finde den Namen des Häuptlings von Zwiebelhausen mit einem SQL-Befehl heraus.

=== "Lösung"
	
	```
	SELECT BEWOHNER.name
	FROM DORF, BEWOHNER
	WHERE DORF.haeuptling = BEWOHNER.bewohnernr
	  AND DORF.name = 'Zwiebelhausen';
	```
	
	|name|
	|---|
	|Fritz Dichter|

## Übung 24

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wie viele Einwohner hat Zwiebelhausen?

=== "Lösung"
	
	```
	SELECT COUNT(*) FROM BEWOHNER, DORF WHERE DORF.dorfnr = BEWOHNER.dorfnr AND DORF.name = 'Zwiebelhausen';
	```
	
	|COUNT(*)|
	|---|
	|8|

## Übung 25

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wie viele weibliche Einwohner hat Zwiebelhausen? (Hinweis: Frauen erkennt man an geschlecht = 'w')

=== "Lösung"
	
	```
	SELECT COUNT(*) 
	FROM BEWOHNER, DORF 
	WHERE BEWOHNER.dorfnr = DORF.dorfnr AND BEWOHNER.geschlecht = 'w' AND DORF.name = 'Zwiebelhausen';
	```
	
	|COUNT(*)|
	|---|
	|1|

## Übung 26

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wie heißt diese weibliche Einwohnerin?

=== "Lösung"
	
	```
	SELECT BEWOHNER.name 
	FROM BEWOHNER, DORF 
	WHERE BEWOHNER.dorfnr = DORF.dorfnr AND BEWOHNER.geschlecht = 'w' AND DORF.name = 'Zwiebelhausen';
	```
	
	|name|
	|---|
	|Dirty Doerthe|

## Übung 27

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wie viel Gold hat Gurkendorf insgesamt?

=== "Lösung"
	
	```
	SELECT SUM(bewohner.gold) FROM BEWOHNER, DORF WHERE DORF.dorfnr = BEWOHNER.dorfnr AND DORF.name = 'Gurkendorf';
	```
	
	|SUM(bewohner.gold)|
	|---|
	|8980|

## Übung 28

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wie viel Gold haben alle Händler, Kaufmänner und Bäcker zusammen?

=== "Lösung"
	
	```
	SELECT SUM(gold) FROM BEWOHNER WHERE beruf IN ('Haendler', 'Kaufmann', 'Baecker');
	```
	
	|SUM(gold)|
	|---|
	|4130|

## Übung 29

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wie schaust du dir die gesamten sowie durchschnittlichen Goldvorräte der einzelnen Berufe an?

=== "Lösung"
	
	```
	SELECT beruf, SUM(bewohner.gold), AVG(bewohner.gold) FROM BEWOHNER GROUP BY beruf ORDER BY AVG(bewohner.gold);
	```
	
	|beruf|SUM(bewohner.gold)|AVG(bewohner.gold)|
	|---|---|---|
	|Erntehelfer|10|10.0|
	|?|70|70.0|
	|Kaufmann|250|250.0|
	|Hufschmied|390|390.0|
	|Waffenschmied|790|395.0|
	|Hoerbuchautor|420|420.0|
	|Pilot|490|490.0|
	|Baecker|1750|583.333333333333|
	|Schmied|1250|625.0|
	|Haendler|2130|710.0|
	|Metzger|11370|2842.5|

## Übung 30

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Wie viel Gold haben im Durchschnitt die einzelnen Bewohnergruppen je nach Status (friedlich, böse, gefangen)?

=== "Lösung"
	
	```
	SELECT status, AVG(BEWOHNER.gold)
	FROM BEWOHNER GROUP BY status;
	```
	
	|status|AVG(BEWOHNER.gold)|
	|---|---|
	|?|70.0|
	|boese|1512.85714285714|
	|friedlich|706.363636363636|
	|gefangen|490.0|

## Übung 31

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Lösche den Einwohner Dirty Dieter.

=== "Lösung"
	
	```
	DELETE FROM BEWOHNER WHERE name = 'Dirty Dieter';
	```
	
## Übung 32

=== "Aufgabe"
	DORF (dorfnr, name, haeuptling)  
	BEWOHNER (bewohnernr, name, dorfnr, geschlecht, beruf, gold, status)  
	GEGENSTAND (gegenstand, besitzer)
	
	Befreie den Piloten.

=== "Lösung"
	
	```
	UPDATE BEWOHNER SET status = 'friedlich' WHERE beruf = 'Pilot';
	```
