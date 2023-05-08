08.05.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 1.1: Betriebssystemgrundlagen

 **a)** Was ist der Unterscheid zwischen prämtivem und nicht-präemtivem Scheduling? Welche Nachteile haben die Strategien der jeweiligen Kategorie?
 
**Lösung:**

**b)** Betrachten Sie die Scheduling-Strategie Round Robin. Was wären Argumente zur Verwendung kurzer bzw. langer Zeitschlitze?

**Lösung:**

**c)** Auf Multiprozessor-Plattformen verwaltet Linux eine eigene runqueue für jede CPU. Ist dies ein gutes Konzept? Erläutern Sie Ihre Antwort, indem Sie zwei Gründe anführen.

|                                  Präemtiv                                   |            Non-Präemptiv            |
|:---------------------------------------------------------------------------:|:-----------------------------------:|
|                     Ermöglicht Prozess zu unterbrechen                      | Prozesse laufen bis sie terminieren |
|               Andere Prozess können zwischengeschoben werden                |  (Oder der PC wird ausgeschaltet)   |
|              Unterbrochenen Prozesse können fortgesetzt werden              |           Bsp: FIFO, SPT            |
|           Bsp: Round Robin, SRPT (Shortest Remaining Time First)            |           Nachteil:                           |
| Nachteil: höhe Anzahl an Kontextwechsel (Verlängert die Zeit bis Bedingung) |                                     |
|                                                                             |                                     |
**Lösung:**