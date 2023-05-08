08.05.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 3.1: Scheduling

 **a)** Was ist der Unterscheid zwischen prämtivem und nicht-präemtivem Scheduling? Welche Nachteile haben die Strategien der jeweiligen Kategorie?

**Lösung:**

Allgemeins: Was ist Scheduling?
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
|                     Ermöglicht Prozess zu unterbrechen                      |           Prozesse laufen bis sie terminieren            |
|               Andere Prozess können zwischengeschoben werden                |             (Oder der PC wird ausgeschaltet)             |
|              Unterbrochenen Prozesse können fortgesetzt werden              |                      Bsp: FIFO, SPT                      |
|           Bsp: Round Robin, SRPT (Shortest Remaining Time First)            | Nachteil: Prozesse können für eine lange Zeit blockeiren |
| Nachteil: höhe Anzahl an Kontextwechsel (Verlängert die Zeit bis Bedingung) |                                                          |


**b)** Betrachten Sie die Scheduling-Strategie Round Robin. Was wären Argumente zur Verwendung kurzer bzw. langer Zeitschlitze?

**Lösung:**

Kurz:

| +                                | -                              |
| -------------------------------- | ------------------------------ |
| parallele Ausführung von Prozessen | sehr oft Kontextwechsel        |
| Antwortzeiten verbessern sich    | verbliebende Zeit geht gegen 0 |

Lang:

| +                      | -                     |
| ---------------------- | --------------------- |
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
| ------- | ------------ | ---------- |
| $P_0$   | 0            | 5          |
| $P_1$   | 2            | 2          |
| $P_2$   | 3            | 6          |
| $P_3$   | 6            | 7          |
| $P_4$   | 9            | 2          |

```mermaid
gantt
    dateFormat  ss
    axisFormat  ss
    title CPU Process Schedule
    section Process A
    Process A : 0, 2s
    Process A : 4s, 7s

```
