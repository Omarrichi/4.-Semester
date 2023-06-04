Aufgabe H18

Die kontextfreie Grammatik $G_1$ für die Menge aller Korrekten Afi-Aufgaben ist folgermaßen:

a)
$G_1 = \{N,T,P,S\}$
$N = \{S,A,B,C\}$
$T = \{(,),+,-,0,1,=\}$ (Terminalalphabet $\Sigma$)
$S = S$
$P:$
$$S \rightarrow A=$$
$$A \rightarrow \space+B\space|\space-B\space|\space0C\space|\space1C\space|\space(A)$$
$$B \rightarrow \space0C\space|\space1C\space|\space(A)$$
$$C \rightarrow \space+B\space|\space-B\space|\space(A)\space |\space\varepsilon$$

b) Linksableitung für das Wort: $-1+1-(-(1+1))=$
$S \space\rightarrow A= \space\rightarrow -B= \space\rightarrow -1C= \space\rightarrow -1+B= \space\rightarrow -1+1C= \space\rightarrow -1+1-B=$ 
$\space\rightarrow -1+1-(A)= \space\rightarrow -1+1-(-B)= \space\rightarrow -1+1-(-(A))= \space\rightarrow -1+1-(-(1C))=$
$\space\rightarrow -1+1-(-(1+B))= \space\rightarrow -1+1-(-(1+1C))= \space\rightarrow -1+1-(-(1+1))=$

c)
Die eingeschränkte Grammatik lautet:


$G_2 = \{N,T,P,S\}$
$N = \{S,A,B\}$
$T = \{+,-,0,1,=\}$ 
$S = S$
$P:$
$$S \rightarrow A=$$
$$A \rightarrow \space-0A\space|\space+0A\space|\space+1A\space|\space-1AB\space | \space \epsilon$$
$$B \rightarrow \space+1A\space$$

d)
ep steht für $\varepsilon$

![[Pasted image 20230604230659.png]]

Erklärung der Grammatik:

Zunächst Teil a):
Das erste Grammatik beschreibt die Sprache der Afi-Aufgaben.
Zunächst sichern wir, dass wir das Endzeichen an der richtigen Stelle haben, danach gucken wir was wir links machen wollen:
Von S gehen wir in A über, dort können wir mit einem beliebigem Zeichen anfangen von dort aus, abhängig von dem Zeichen den wir genommen haben gehen wir entweder in B oder C über. Bei B und C gehen wir entsprechend der letzten Symbol, falls wir +/- genommen haben, gehen wir in B, dort können wir nur 0/1 schreiben. Analog für C aber umgekehrt. Zum Schluss kommen die Klammern, die können wir beliebig oft einsetzen sorgen aber jedesmal dafür, dass wenn eine aufgeht, dass auch zugeht. In die Klammern, ist eine "neue" Aufgabe, deswegen können wir dort mit einem beliebigen Zeichen anfangen. Enden können wir nud bei C, da dort $\epsilon$ ist. diese ist nicht bei B damit wir nicht mit einem + oder - minus enden, sondern auch wirklich mit einem Ziffer.

Zur Zeil c)
Hier ist die Sprache eingeschränkter. Zunächst haben wir den gleichen Schritt, wir sichern das = Zeichen.
Danach in A, können wir beliebig Zeichen hinzufügen mit der einzigen Vorraussetzung, dass bei jedem -1 wird ein +1 gezungen (der Übergang -1A*B*) natürlich muss dies aber nicht sofort passieren, deswegen haben wir ein A dazwischen damit die Zwischenzeichen auchgeschrieben werden können. Aber wir können nicht terminieren bis wir ein positives Ergebniss haben.