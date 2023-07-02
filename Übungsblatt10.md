Omar Richi 434549
Niklas Block 434449
Max Dung 434798
Milo Tieu, 435019

H31:
Konstruktion:

Der PDA M1 hat die Zustandsmenge Q_1= {q_0, ... , q_n} und der DFA M2 hat die Zustandsmenge Q_2 = {p_0, ... , p_k} mit n,k aus N
M1 hat die Zustandstransitionen δ_1: Q × (Σ ∩ {ε}) x Γ x Q x Γ*  und Γ als Stackalphabet und Z als Kellersymbol. M2 besitzt die Zustandstransitionen δ_2: Q × Σ → Q .  q_f und p_f seien hier unsere akzeptierende Zustände.
 
Wir simulieren L(B) als PDA, dies ist insofern möglich da der PDA ein DFA mit Keller ist. Dementsprechen haben wir nun δ_2P: Q ×(Σ ∩ {ε}) x Γ x Q x Γ* .Um zu garantieren, wir alle Transition nur mit dem Zusatz des Kellers erhalten wollen,wir fügen wir immer in nur ε hinzu.
Und im unseren Stack befindet sich nur unser Kellersymbol. Somit ändern wir nicht die Funktion unseres DFAs M2.

Jetzt simulieren wir beide PDAs gleichzeitig als unseren PDA M3. Unsere Zustandsmenge für den PDA M3 sei Q_3 = Q_1 x Q_2 . Die Transitionen δ_3:  δ_1 x  δ_2  .Da unsere gegebene Sprache das Komplement der  Sprache L(M) von L(D) ist, heißt das beide akzeptierende 
Zustände mengenlogisch disjunkt sein müssen und akzeptierende Zustände nehmen die auch nur exklusiv von M1 verwendet werden, damit der PDA unsere Sprache repräsentiert. Für jedes Wort w das in M1 akzeptiert wird, d.h bei q_f ist, aber nicht in B akzeptiert wird ist für M3 ein gültiges Wort bzw in unserer Sprache drin.

Korrektheit:
Sei w ein Wort aus Σ* . So wird w nicht akzeptiert, wenn es sowohl nicht in A als auch in B liegt. Es wird weiterhin auch nicht akzeptiert, wenn w ∈ A ∩ B ist. Wenn w ∈ B ist und nicht in A ist, wird verworfen. Insofern aber w ∈ A und nicht in B liegt, besitzen wir einen akzeptierenden Lauf für w, um es ein gütliges Wort der Sprache zu machen.
Sei w ein Wort aus L(M)\L(D) so gibt es ein Lauf, wo wir in den akzeptierenden Zustand von A gelangen ohne gleichzeitig in den akzeptierenden Zustand von B zu erreichen.  


![[Pasted image 20230703011059.png]]
![[Pasted image 20230703011108.png]]

![[Pasted image 20230703011149.png]]
![[Pasted image 20230703011208.png]]
![[Pasted image 20230703011218.png]]
![[Pasted image 20230703011226.png]]
