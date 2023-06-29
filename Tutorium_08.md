26.06.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 8.1: Speicher vs. Speicher

![[Pasted image 20230629191450.png]]

*Allgemein;* 
Hauptspeicher:
- Arbeitet mit logischen und physikalischen Adressen
- benutzt eher Paging
- muss schnelle Zugriffe machen
	- deswegen benutzen wir Hardware (MMU)
- Jeder langsamer Zugriff lässt die CPU in Leerlauf
	- wenn dies häufig passiert, wird das ganze System langsamer

Festplatte:
- Arbeitet mit Segmente
- Positionen innerhalb einer Datei in Blöcke auf der Festplatte
	- Aus Blatt 1, die Verzeichnisse und Dateien in einer Datei / Verzeichnis, sind nun Adressen / Verweise, wo die wirklich Datei ist.


**Lösung: **

*a)*
Hauptspeicher:
- Viel in Hardware umsetzten um die Zugriffe zu beschleunigen (MMU)
- Muss schnell erfolgen, also wenig Zeit ist da
- Die Instruktionen (also z.B. angefragte Seiten in einem String) werden die Reihe nach bearbeitet
- Verzögerung führt zu einem CPU Leerlauf.

Festplatte:
- Viel in Verwaltungsaufwand
- Zeit ist nicht mehr kritisch
- Zugriffe können langsamer behandelt werden, die Zeit wird eher in Verwaltung verbracht
	- Auch wenn der Zugriff selber mal weniger Zeit bekommt
- Umsetzung in Software
- Instruktionen bzw. Reihenfolge kann beliebig geändert werden, wenn es schneller geht.

*b)*
- Defragmentierung, ist genau wie es sagt. Wir "lösen" die Fragmente bzw. Segmente unseres Speicher.
- Wie erwähnt finden Speicherbelegungen in Form von Segmente statt, das heißt jeder Prozess bekommt so viel wie er braucht.
- Angenommen, wir benutzen dies, und geben unsere Prozesse

### Aufgabe 8.2: I-Nodes

![[Pasted image 20230629191614.png]]

### Aufgabe 8.3: Disk-Scheduling

![[Pasted image 20230629191626.png]]

### Aufgabe 8.4: I/O-System

![[Pasted image 20230629191641.png]]

### Aufgabe 8.5: DNS

![[Pasted image 20230629191654.png]]