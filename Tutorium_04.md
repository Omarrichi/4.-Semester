15.05.2023 - Omar Richi (omar.richi@rwth-aachen.de)

### Aufgabe 4.1: 

Die drei Kriterien des Wechselseiteiges Ausschlusses:
- Mutual Exclusion:
	- nur maximal ein Prozess zur Zeit darf im kritischen Bereich sein.
- Progress:
	- Wenn Prozesse warten, muss auch einer in den kritischen Bereich dürfen.
- Bounded waiting:
	- Es gibt eine Obergrenze, wie oft ein Prozess überholt werden darf.

1. Mutual Exclusion ist verletzt: 
	- Wenn die Prozesse wechselnd ausführen, werden beide Flags auf True gesetzt und beide Prozesse betreten den kritischen Bereich.

| t   | P0             | P1             | flag[0] | flag[1] |
| --- | -------------- | -------------- | ------- | ------- |
| 1   | flag[0]=FALSE  | -              | FALSE   | -       |
| 2   | -              | flag[1]=FALSE  | FALSE   | FALSE   |
| 3   | while(flag[1]) | -              | FALSE   | FALSE   |
| 4   | -              | while(flag[0]) | FALSE   | FALSE   |
| 5   | flag[0]=TRUE   | -              | TRUE    | FALSE   |
| 6   | -              | flag[1]= TRUE  | TRUE    | TRUE    |
| 7   | critical       | -              | TRUE    | TRUE    |
| 8   | -              | critical       | TRUE    | TRUE    |

2.  Progress ist verletzt:
	-  beide Prozesse setzten abwechseldn ihre Flags auf TRUE, beide betreten die Schleife und warten dass der andere sein Flag auf FALSE setzt.


| t   | P0             | P1             | flag[0] | flag[1] |
| --- | -------------- | -------------- | ------- | ------- |
| 1   | flag[0]=FALSE  | -              | FALSE   | -       |
| 2   | -              | flag[1]=FALSE  | FALSE   | FALSE   |
| 3   | flag[0]=TRUE   | -              | TRUE    | FALSE   |
| 4   | -              | flag[1]=TRUE   | TRUE    | TRUE    |
| 5   | while(flag[1]) | -              | TRUE    | TRUE    |
| 6   | -              | while(flag[0]) | TRUE    | TRUE    |

3. Progress ist verletzt: 
	- Zuerst führt $P_0$ aus, turn wird auf 1 gesetzt aber $P_0$ stirbt, jetzt darf $P_1$ einmal ausführen und setzt turn wieder auf 0. Turn bleibt bei 0 da $P_0$ nicht mehr lebt, also $P_1$ darf nicht mehr ausführen

| t   | P0        | P1    | turn |
| --- | --------- | ----- | ---- |
| 1   | crit.     | -     | 0    |
| 2   | turn = 1  | -     | 1    |
| 3   | remain... |       | 1    |
| 4   | stirbt    | crit. | 1    |
| 5   | -         | turn = 0      | 0     |
| 6   | -         |    remain.   |   0   |
| 7   | -         |    while   |      0|
