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
	1. Wenn die Prozesse wechselnd ausführen, werden beide Flags auf True gesetzt und beide Prozesse betreten den kritischen Bereich.

| t   | P0             | P1             | flag[0] | flag[1] |
| --- | -------------- | -------------- | ------- | ------- |
| 1   | flag[0]=FALSE  | -              | FALSE   | -       |
| 2   | -              | flag[1]=FALSE  | FALSE   | FALSE   |
| 3   | while(flag[1]) | -              | FALSE   | FALSE   |
| 4   | -              | while(flag[0]) | FALSE   | FALSE   |
| 5   | flag[0]=TRUE   | -              | TRUE    | FALSE   |
| 6   | -              |                |         |         |
| 7   |                |                |         |         |
| 8   |                |                |         |         |
|     |                |                |         |         |
