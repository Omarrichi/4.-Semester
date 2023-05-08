08.05.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 3.1: Scheduling

 **a)** Was ist der Unterscheid zwischen prämtivem und nicht-präemtivem Scheduling? Welche Nachteile haben die Strategien der jeweiligen Kategorie?

**Lösung:**

Allgemein: Was ist Scheduling?
- Verteilung und Zuweisung von begrenzten Ressourcen an Prozessen
	- Wenn z.B. zwei Prozesse existieren, dann entscheidet der Scheduler welcher Prozess als nächstes ausgeführt werden soll.
- Es gibt mehrere Schedulingstrategien, die versuchen folgende Ziele zu erreichen:
	1. Fairness: Jeder Prozess soll fair behandelt werden, D.h jeder Prozess soll nach einer gewissen Zeit seine CPU-Zuteilung bekommen
	2. Auslastung der CPU: Die CPU darf keine Pause haben, sie soll möglichst ausgelastet werden
	3. Durchsatz maximieren: Die Anzahl der zubearbeiteten Prozesse soll pro Zeiteinheit möglichst hoch sein.
	4. Minimierung der Ausführungszeit: Diese beginnt wenn der Prozess an kommt, und endet wenn der Prozess terminiert, Wir können nicht ändern wie lange ein Prozess braucht, jedoch können wir dafür sorgen, dass er nicht viel warten muss zum Beispiel.
	5. Antwortzeit minimieren: sorgen für schnelle Reaktionen des Systems auf die Eingaben, also die Zeitspanne zwischen ankommen des Prozesses und die erste Ausfühung sollte minimiert sein.


|                                  Präemtiv                                   |                      Non-Präemptiv                       |
|:---------------------------------------------------------------------------:|:--------------------------------------------------------:|
|                     Ermöglicht Prozesse zu unterbrechen                      |           Prozesse laufen bis sie terminieren            |
|               Andere Prozess können zwischengeschoben werden                |             (Oder der PC wird ausgeschaltet)             |
|              Unterbrochenen Prozesse können fortgesetzt werden              |                      Bsp: FIFO, SPT                      |
|           Bsp: Round Robin, SRPT (Shortest Remaining Time First)            | Nachteil: Prozesse können für eine lange Zeit blockeiren |
| Nachteil: höhe Anzahl an Kontextwechsel (Verlängert die Zeit bis Bedingung) |                                                          |


**b)** Betrachten Sie die Scheduling-Strategie Round Robin. Was wären Argumente zur Verwendung kurzer bzw. langer Zeitschlitze?

**Lösung:**

Kurz:

| +                                | -                              |
| :--------------------------------: | :------------------------------: |
| parallele Ausführung von Prozessen | sehr oft Kontextwechsel        |
| Antwortzeiten verbessern sich    | verbliebende Zeit geht gegen 0 |

Lang:

| +                      | -                     |
| :----------------------: | :---------------------: |
| Weniger Kontextwechsel | Langläufer blockieren |


**c)** Auf Multiprozessor-Plattformen verwaltet Linux eine eigene runqueue für jede CPU. Ist dies ein gutes Konzept? Erläutern Sie Ihre Antwort, indem Sie zwei Gründe anführen.


**Lösung:**
- Jede CPU hat ihre eigene Queue
	- Vermeidung von cache Thrashing, wenn wir CPU wechseln
	- Schedulingstrategien und Suchaufwand steigert sich
	- Scheduling-Entscheidungen können dann lokal folgen:
		- keine Synchronisation notwendig, da die CPUs nicht auf die gleiche Queue zugreifen müssen
		- Scheduling von einem CPU blockiert das Scheduling von anderen CPUs nicht

### Aufgabe 3.2: Scheduling-Strategien

| Prozess | Ankunftszeit | Bedienzeit |
| :-------: | :------------: | :----------: |
| $P_0$   | 0            | 5          |
| $P_1$   | 2            | 2          |
| $P_2$   | 3            | 6          |
| $P_3$   | 6            | 7          |
| $P_4$   | 9            | 2          |

**Lösung:**

LIFO:

![[Pasted image 20230508203209.png]]

Mittlere Wartzeit: $\frac{0+18+2+7+2}{5} = \frac{29}{5} = 5,8$

SRPT:

![[Pasted image 20230508203403.png]]

Mittlere Wartzeit: $\frac{2+0+6+9+0}{5} = \frac{17}{5} = 3,4$

### Aufgabe 3.3: Echtzeitsysteme und Earliest Deadline First

| Prozess | Ankunftszeit | Bedienzeit | Deadline |
| :-------: | :------------: | :----------: | :--------: |
| $P_1$   | 4            | 8          | 19       |
| $P_2$   | 10           | 3          | 28       |
| $P_3$   | 0            | 7          | 10       |
| $P_4$   | 1            | 2          | 5        |
| $P_5$   | 5            | 5          | 11       |
| $P_5$   | 16           | 1          | 17       |

**Lösung:**
a)

![[Pasted image 20230508203705.png]]
 
b)
Es werden zwei Deadlines verpasst, $p_1$ und $p_5$
die Verspätungen sind entsprechend 4 und 3
Wir könnten $p_5$ nicht bearbeiten und stattdessen $p_1$ die Zeit geben, dann hätten wir nur eine Verletzung gehabt, trotzdem falls eine spätere Antwort besser ist als gar keine, würde man trotzdem zwei Verletzungen vorziehen, damit auch $p_5$ eine geringe Antwortzeit hat, und entsprechend eventuell bestimmte Betriebsmitteln nicht mehr blockiert.


### Aufgabe 3.4: "Multilevel Feedback Queueing"

| Prozess | Ankunftszeit | Bedienzeit | Priorität |
| :-------: | :------------: | :----------: | :---------: |
| $P_1$   | 0            | 5          | 1         |
| $P_2$   | 0            | 6          | 3         |
| $P_3$   | 2            | 2          | 2         |
| $P_4$   | 4            | 10         | 4         |
| $P_5$   | 6            | 7          | 2         |
| $P_5$   | 8            | 2          | 1         | 

**Lösung:**
a) ohne Feedback:

![[Pasted image 20230508204716.png]]

b) Mit Feedback:

![[Pasted image 20230508204751.png]]

c)
Interaktive Prozesse:
- Interaktive Prozesse benötigen geringes Quantum
- hohes Quantum bei solchen Prozessen führt zur Steigerung der Antwortzeiten
- ein großer Nachteil, da die Prozesse interaktiv sind, also das häufiger wechsel von Prozessen ist erwünscht

Rechenintensiveprozesse:
- leiden unter kleinen Quanten
- sehr viele Kontextwechsel für einen Prozess
- effizienter: CPU Zeit für den Prozess zu verlängern

Ziel: Eine Kombination, Antwortzeiten sollen gering bleiben, aber wenn nicht viele Prozesse sind, dann vermeide Kontextwechsel.