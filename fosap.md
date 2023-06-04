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
