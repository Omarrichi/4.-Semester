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
$S \rightarrow A= \rightarrow -B= \rightarrow -1C= \rightarrow -1+B= \rightarrow -1+1C= \rightarrow -1+1-B= \rightarrow -1+1-(A)= \rightarrow -1+1-(-B)= \rightarrow -1+1-(-(A))= \rightarrow -1+1-(-(1C))=$