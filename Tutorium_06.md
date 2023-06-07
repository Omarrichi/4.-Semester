05.06.2023 - Omar Richi (omar.richi@rwth-aachen.de)

#### Aufgabe 6.1 Speicherverwaltung:

a) Welche Aufgaben hat die Speicherverwaltung in Betriebssystemen?

b) Welche Aufgaben hat die MMU? Welchen Sinn hat dabei die Zweiteilung einer logischen Adresse in Segement- bzw. Seitennummer und Offset?

c)  der Aufruf einer logischen Adresse, zu der keine physikalische Adresse existiert, vorkommen? Wenn ja, wie sollte die MMU damit umgehen?

d) Warum ist es sinnvoll, die MMU als Hardwarekomponente umzusetzen, anstatt die notwendigen Berechnungen in Software durchzuführen?

**Lösung:**

a) 

- Übersetzten von logischen in physikalischen Adressen
- Zuweisung von Speicher an Prozesse, Freigabe von Speicher
- Verhinderung von unerlaubten Speicherzugriffen
- Auslagerung von Speicher auf bzw. Laden vom Hintergrundspeicher
- Ermöglichung von Shared Memory

b) 

- Die MMU rechnet logische Adressen in physikalische um
- befindet sich zwischen CPU und Speicher
- Durch die Zweiteilung wird der Umfang der Abbildungstabelle der MMU reduziert
	- Muss nicht zu jeder möglichen Adresse ein Eintrag in der Tabelle existieren, sondern nur jeder möglichen Adresse eines Segments / Seite.

c)

- darf vorkommen, es ist auch ein Regelfall also kommt regelmäßig vor.
- Es heißt, ein Prozess will auf eine Speicherstelle zugreifen, die aber nicht in den Hauptspeicher geladen ist
- Page Fault: Die MMU übersetzt und sucht die angefragte Adresse, und sie wird dann in den Hauptspeicher geladen.

d) 
- Geschwindigkeit, übersetzten passiert sehr häufig, deswegen extra Komponente, sonst wird das System Allgemein verlangsamt

#### Aufgabe 6.2 Adressen:

Für die Speicherverwaltung nach dem Segmentierungsverfahren sei für einen Prozess die folgende Segmenttabelle gegeben:

| Segment | Basisadresse | Länge |
|:-------:|:------------:|:-----:|
|    0    |     1320     |  320  |
|    1    |     562      |  58   |
|    2    |     310      |  120  |
|  3750   |              |       |
