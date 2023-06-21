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
2. Hilft auch nicht, Speicherplatz ist kein Problem, Problem ist die Geschwindigkeit.
	1. Ihr könnt euch vorstellen: Wenn unser Seiten Objekte in einem Lager sind, und die Tür ist so groß wie eine normale Tür, wenn w 