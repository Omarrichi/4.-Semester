21.06.2023 - Omar Richi (omar.richi@rwth-aachen.de)

Tut mir leid für die Verspätung, da aber das Thema etwas komplex sein kann, und viele wegen DSAL Einsicht nicht da seien konnten, habe ich mich entscheiden die Notizen ausführlicher zu machen.


Dieses Blatt ist Teil 2 des Kapitels "Speicherverwaltung"

Zunächst **Allgemeines:**

Wenn ein Prozess ausgeführt wird, werden entsprechend seine Daten geladen dies geschieht indem die Seiten in den Hauptspeicher geladen werden. Jedoch hat der Hauptspeicher nicht genug Platz für alle diese Seiten für alle ausgeführten Prozesse, deswegen werden nur die nötigen Seiten in den Hauptspeicher geladen. Wenn ein Prozess  ausgeführt wird dann müssen seine Seiten in die CPU geladen werden, dies geschieht über die MMU.

- Bei Zugriff auf einer Seite:
	- Die MMU prüft, ob diese Seite im Hauptspeicher schon exisitert.
	- Wenn ja, dann wird sie nun übersetzt (Physikalisch $\Leftrightarrow$ Logisch)
	- Wenn nein, dann:
		- 1. Die Seite existiert tatsächlich, muss aber von der Festplatte in den Hauptspeicher geladen werden (Page Fault / Seitenfehler)
		- 2. Die Seite existiert nicht bzw. gehört diesen Prozess nicht, dann wird der Zugriff verhindert (Segmentation Fault)
- Wenn ein Seitenfehler vortritt dann: 
	- Behandlungsroutine ausführen (Page Fault Handler)
	- Prozess ggfs. schlafen legen
	- Fehlende Seite wird lokalisiert und nachgeladen (eventuell werden andere Seiten verdrängt!)
- Seitenfehler verbringen einiges an Zeit deswegen: Seitenfehler Vermeiden!

### Aufgabe 7.1: Paging

```ad-note
title:Allgemein

Demand-Paging: beschreibt das System Speicher zu verwalten, sodass Speicher nur dann zugegriffen wird, wenn es gebraucht ist. Auf unserem System, holen wir uns also nur die Seiten die wir brauchen, und verdrängen die, die wir nicht mehr brauchen würden.
Das System ist nicht optimal implementierbar, da man nicht wissen kann, wann welche Seite angefragt werden wird, deswegen behandeln wir Allgemein, Strategien um eine Approximation zu erreichen.
```

Gegeben sei ein Demand-Paging-System, das aktuell wie folgt ausgelastet ist:

- CPU-Auslastung: 20%
- Auslastung des Zugris auf die Paging-Disk (d.h. die Festplatte, auf die der virtueller Speicher ausgelagert ist): 97,7%
- Auslastung der sonstigen I/O-Geräte: 5%

Ziel sollte aber eigentlich sein, die CPU so gut wie möglich auszulasten, um alle Programme schnell abzuarbeiten! Geben Sie für jeden der folgenden Vorschläge mit Begründung an, ob er sich eignen
würde, die Auslastung ihrer CPU zu verbessern (d.h. zu erhöhen):

i) Installation einer schnelleren CPU

ii) Installation einer größeren Festplatte

iii) Erhöhung der Anzahl laufender Prozesse

iv) Verringerung der Anzahl laufender Prozesse

v) Installation von mehr Hauptspeicher

vi) Installation einer schnelleren Festplatte

**Lösung:**

1. hilft nicht, unser CPU ist jetzt schon nicht ausgelastet, eine schnellere wird nun weniger ausgelastet sein, die Geschwindigkeit Seiten zu laden ändert sich hier nicht.
2.  Hilft auch nicht, Speicherplatz ist kein Problem, Problem ist die Geschwindigkeit.
	1. Ihr könnt euch vorstellen: Wenn unser Seiten Objekte in einem Lager sind, und die Tür ist so groß wie eine normale Tür, wenn wir den Lager größer machen, heißt nicht automatisch, dass wir eine größere Tür haben (Oder Schuhe auf Running Modus :) ) 
3. Mehr Prozesse, heißt mehr Seiten Anfragen, heißt mehr Seitenfehler, also hilft auch nicht
4. Analog wie drei jedoch umgekehrt, weniger Prozesse, heißt weniger Anfragen, also weniger Seitenfehler, dies wird also helfen.
5. Mehr Hauptspeicher, heißt mehr Platz für unsere Seiten, also wir werden weniger Seiten austauschen müssen $\Rightarrow$ weniger Seitenfehler, also das wird auch helfen
6. Das wird helfen, zwar haben wir immer noch die gleiche Anzahl an Seitenfehler, aber die CPU kann die Daten schneller bekommen
	1. Das gleiche wie bei 2. Lager bleibt diesmal gleiche große, jedoch die Tür wird größer, sodass man schneller hin und her laufen kann, (Running Modus ist hier auch gut treffend)


### Aufgabe 7.2: Demand Paging

```ad-abstract
title: Zusammenfassung
unten ist eine Wand Text, hier ist eine kleine Zusammenfassung:
Wir wollen Seiten laden, es gibt insgesamt 7 Seiten [0,1,2,3,4,5,6]
Wir haben eine begrenzte Anzahl an Plätze im Hauptspeicher (4 Rahmen)
Also wir können nicht alle Seiten gleichzeitig einfach laden, wir wissen nun welche als nächstes geladen werden muss, müssen jedoch entscheiden welche wir rausschmeißen wollen

Liest euch die Beschreibungen der Strategien aus der Frage, bei der Lösung findet ihr auch eine kurze Erklärung.
```


In der Vorlesung haben Sie das Prinzip des Pagings kennengelernt: der Adressraum eines Prozesses wird in Seiten (Pages) fester Größe eingeteilt, der Hauptspeicher in Rahmen der gleichen Gröÿe, welche jeweils eine Seite aufnehmen können. Die Zahl der Rahmen im Hauptspeicher ist im Allgemeinen kleiner als die Anzahl der tatsächlich im System existierenden Seiten. Seiten, die gerade nicht im Hauptspeicher gehalten werden können, werden auf ein externes Speichermedium ausgelagert. Befindet sich eine Seite nicht im Hauptspeicher, wenn auf sie zugegriffen werden soll, muss sie zuvor vom externen Speicher in den Hauptspeicher geladen werden (Seitenfehler, Page Fault). Sind noch nicht alle Rahmen im Hauptspeicher gefüllt, stellt dies kein Problem dar. Ist jedoch kein Platz mehr vorhanden, muss eine Seite im Hauptspeicher ausgewählt werden, die ausgelagert und durch die neue Seite ersetzt wird. Für die Auswahl der zu ersetzenden Seite gibt es verschiedene Strategien, unter anderem:

**FIFO (First In, First Out):** Die jeweils älteste Seite wird aus dem Hauptspeicher ausgelagert.

**LRU (Least Recently Used):** Es wird die Seite aus dem Hauptspeicher entfernt, auf die am längsten kein Zugriff mehr erfolgt ist.

**SC (Second Chance):** Diese Strategie ist eine Vereinfachung von LRU. Pro Seite wird ein ACCESSED Bit gespeichert, welches gesetzt wird, wenn auf eine bereits geladene Seite ein weiterer Zugriff erfolgt. Muss eine Seite ersetzt werden, wird die älteste Seite geprüft: ist ihr A-Bit nicht gesetzt, wird sie verdrängt; ist ihr A-Bit gesetzt, wird sie zur jüngsten Seite und ihr A-Bit wird zurückgesetzt - nun wird erneut die jetzt älteste Seite überprüft. Dies geschieht so lange, bis eine Seite verdrängt wurde.

**LFU (Least Frequently Used):** Es wird die Seite mit der geringsten Nutzungshäufigkeit ausgetauscht. Die Nutzungshäufgkeit bezieht sich dabei auf ein Zeitintervall der jüngsten Vergangenheit. Für diese Aufgabe wird angenommen, dass die Nutzungshäufigkeit jeweils ab dem Ladezeitpunkt einer Seite gezählt wird. Liegt für mehrere Seiten die gleiche Nutzungshäufigkeit vor, wird die älteste dieser Seiten zuerst ausgetauscht.

**CLIMB:** Wird eine Seite geladen, wird sie als 'älteste' Seite vermerkt. Wie bei FIFO erfolgt stets eine Verdrängung der ältesten Seite. Eine Seite muss sich zuerst bewähren, um länger im Speicher verbleiben zu können: erfolgt ein erneuter Zugriff auf eine Seite, während sie bereits im Speicher ist, wird sie um eine Position verjüngt, also bezüglich ihres Alters mit der nächstjüngeren Seite getauscht.

**OPT (Optimalstrategie):** Diese Strategie ersetzt die Seite, die am längsten nicht mehr benötigt wird. Wird eine Seite in Zukunft gar nicht mehr benötigt, wird sie auf die Stelle mit niedrigster Priorität gesetzt.
Seitenzugriffe werden hier als Referenzstring angegeben, welcher die zeitliche Reihenfolge der angeforderten Seiten enthält. Ein solcher String ist z.B. ω = 0 1 2 3 4 1 5 5 2 4 5 1 1 0 6 2 5 2 3 5 : Erst erfolgt ein Zugriff auf Seite 0, dann auf Seite 1, dann auf Seite 2 usw.. Seitennummern seien hier immer einstellig.

Gegeben sei ein System mit vier Rahmen, die zu Beginn leer sind. Geben Sie für jede der oben aufgeführten Strategien die Belegung dieser vier Rahmen nach jedem Seitenzugriff sowie die Anzahl der auftretenden Seitenfehler an. Legen Sie dabei den oben angegebenen Referenzstring zugrunde.

Sollten beim Laden einer Seite noch mehrere Rahmen frei sein, belegen Sie immer den Rahmen mit der niedrigsten Nummer. Ist kein Rahmen mehr frei, ersetzen Sie jeweils eine Seite entsprechend der verwendeten Strategie.

Dabei sollen die Felder "Priorität" der Unterstützung und Übersichtlichkeit bei der Erstellung der Rahmenbelegung dienen (füllen Sie sie auf jeden Fall auch aus). Diese Felder entsprechen der Liste, die das Betriebssystem verwaltet, um zu entscheiden, in welcher Reihenfolge bereits geladene Seiten ausgetauscht werden. Wird z.B. bei FIFO als erstes Seite 5 geladen, so ist "Priorität 1" 5 und die anderen Felder sind leer. Wird im zweiten Schritt 7 geladen, so ist "Priorität 1" 7, "Priorität 2" 5 und die anderen Felder sind leer. Der Wert in Priorität 4 gibt also die Seite an, die beim nächsten Seitenfehler ausgetauscht wird. Halten Sie in diesem Feld auch das A-Bit für SC und und den Häufigkeitszähler für LFU nach.

Die einzige Ausnahme bei der Sortierung der Prioritätenliste soll bei LFU gemacht werden: verwenden Sie wie bei FIFO nur das Alter der Seiten als Sortierkriterium. Dies führt zwar dazu, dass  je nach Ständen der Häufigkeitszähler - die zu verdrängende Seite nicht immer in "Priorität 4" steht sondern ggfs. über den zusätzlichen Zähler entschieden werden muss, macht aber das Beibehalten der korrekten Reihenfolge bei der Änderung von Zählerständen einfacher. 
Im Feld "Seitenfehler" markieren sie mit einem X, wenn in diesem Schritt ein Seitenfehler aufgetreten
ist und eine Seite ausgetauscht wurde.

**Lösung:**

Wichtig hier bei allen Strategien, wenn eine Seite verdrängt wird, werden die Rahmen nicht komplett neu zugeteilt, lediglich übernimmt die neue Seite den Platz der älteren Seiten. Aus diesem Grund führen wir eine zweite Tabelle "Priorität", sodass wir mithalten können welche jetzt als nächstes verdrängt werden soll. Prioritätstabelle, ist in der Regel keine Pflicht, jedoch ist sie extrem hilfreich, ich empfehle euch sie immer mitzuführen, da sie auch Folgefehler ermöglicht.

- FIFO:
	- Die einfachste Strategie, jede neue Seite wird als jüngste im Hauptspeicher eingelagert, wenn alles voll ist, dann nehmen wir die älteste aus dem Speicher.

![[Pasted image 20230622131130.png]]

- LRU (Least Recently Used): 
	- Identisch zur FIFO mit dem einzigen Unterschied:
	- Falls eine Seite im Hauptspeicher ist, bzw. Falls 