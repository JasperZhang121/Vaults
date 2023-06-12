
>1

**Consider the following set Γ of first order formulae:**

```
Γ = { 
		∀x(¬Q(x) → P(x)),
		¬∃y P(y),
		Q(a) → ∃x(R(x) ∧ ¬Q(x)) 
}
```

**(a) Use normal-forming moves to transform Γ into a set ∆ of first order clauses such that ∆ is satisfiable if and only if Γ is satisfiable.**

First get the quantifiers to the front ([[2. Prenex Normal form]]):

```
		∀x(¬Q(x) → P(x)), 
		∀y ¬P(y),
		∃x(Q(a) → (R(x) ∧ ¬Q(x)))
```

Next remove the existential quantifier and use a name (skolem constant) instead:

```
		∀x(¬Q(x) → P(x)),
		∀y ¬P(y),
		Q(a) → (R(b) ∧ ¬Q(b))
```


Delete the universal quantifiers, and put the propositional parts into clause form:

```
∆ = { 
		Q(x) ∨ P(x),
		¬P(y), 
		¬Q(a) ∨ R(b) ¬Q(a) ∨ ¬Q(b) 
	}
```


(b) Write out a resolution proof by which the empty clause is derived from ∆. For each resolution inference in the proof, make a note of  any unifier that is involved.

A resolution derivation could be written something like this:

```
1. Q(x) ∨ P(x) given 
2. ¬P(y) given 
3. Q(x) from 1, 2 unifier {y ← x} 
4. ¬Q(a) ∨ ¬Q(b) given 
5. ¬Q(b) from 3, 4 {x ← a} 
6. ⊥ from 3, 5 {x ← b}
```

> 2.

>Consider a situation in which you want to bake two dishes d1 and d2 that cannot both be in the oven simultaneously. You will get home by 6:00pm on the day, and you have to prepare the food under certain constraints: 
1. Both dishes take 35-40 minutes to cook (i.e., each dish must be put in the oven and taken out between 35 and 40 minutes later), 
2. d1 requires 30 minutes preparation time, and must cool for at least 10 minutes before being served, 
3. you must serve dinner by 7:30pm, 
4. you cannot start any cooking or preparation until at least 6:00pm, with the exception of the preparation for d1, which you can choose to do either after 6:00pm or on the previous day, 
5. neither dish can go into the oven for the first 15 minutes after 6:00pm today while you wait for the oven to pre-heat. 

>Formulate this problem as a constraint network γ = (V, D, C), defining <mark style="background: #FFB8EBA6;">variables, domains and constraints</mark> following the notation used in the lectures. Think carefully about what the variables should be, and what they represent. Constraints can be defined either explicitly— specifying the allowed pairs of values—or in some more compact notation that summarises the allowed pairs of values. Note: This can be designed as a very simple constraint network if you abstract away the times, but the idea of this course is to let the computer do the logic and reasoning, so try to construct a constraint network using the times listed.

The <mark style="background: #FF5582A6;">variables</mark> we will use for this problem are time points at which events occur, and the values they take are the times since 6:00pm. We can use a resolution of 5 minutes here, because all the time periods are divisible by 5, and no constraints require a gap more than 0 minutes and less than 5 minutes between events. For the variables, we have:

V = {prep1 , in-oven1, in-oven2, out-oven1, out-oven2,serve}

And the <mark style="background: #FF5582A6;">domains</mark> are the number of minutes since 6:00pm, and a extra value for the preparation happening the day before. Times can be between 6:00pm and 7:30pm, and we can apply the unary constraints in advance, so we have:

Dprep1 = {yesterday, 0, 5, . . . , 90}
Din-oven1 = Din-oven2 = {15, 20, . . . , 90} 
Dout-oven1 = Dout-oven2 = {0, 5, . . . , 90} 
Dserve = {0, 5, . . . , 90}

And finally the <mark style="background: #FFF3A3A6;">constraints</mark>:

Cprep1 ,in-oven1 ≡ prep1 = yesterday ∨ prep1 + 30 ≤ in-oven1
Cin-oven1,out-oven1 ≡ 35 ≤ out-oven1 − in-oven1 ≤ 40 
Cin-oven2,out-oven2 ≡ 35 ≤ out-oven2 − in-oven2 ≤ 40 
Cout-oven1,serve ≡ out-oven1 + 10 ≤ serve 
Cout-oven2,serve ≡ out-oven2 ≤ serve 
<mark style="background: #ADCCFFA6;">Cin-oven1,out-oven2 ≡ ¬(0 < out-oven2 − in-oven1 < 70) 
Cin-oven2,out-oven1 ≡ ¬(0 < out-oven1 − in-oven2 < 70)</mark>


> 3
	Consider the following logic puzzle: Alex, Beatrice and Catherine were attending a party at which the murder of the fourth guest took place. There were three potential weapons: a knife, a revolver and a wrench. Given the room layout below, and the following clues, who could the suspects be, and what weapons did they use? 

1 | 2 | 3
--|--|--

The murder took place in room 2. At the time: 
1. Alex, Beatrice and Catherine were all in separate rooms 
2. The weapons were also in separate rooms. 
3. Alex was in a room adjacent to the revolver. 
4. Beatrice was not in the room with the wrench. 
5. Catherine was in a room adjacent to the knife. 
6. Neither revolver or Beatrice were in room 1

>(a) This problem can be formulated as a constraint network with the variables 
	$V = {A, B, C, k, r, w}$ 
	which each have a domain of $D = {1, 2, 3}$ 
	Describe another choice of variables and domains for this problem

Here the variables describe what room each item is in, but because every person is in a different room, and each weapon is in a different room, variables could go in the other direction. We could have a variable for which person is in each room, and which weapon is in each room:

$V = {P1, P2, P3, W1, W2, W3}$

Where the $P_i$ represents the person in room i and similarly $W_i$ is the weapon in room i. Given that, the <mark style="background: #FFB8EBA6;">domains</mark> are:

$DP 1 = DP 2 = DP 3 = {A, B, C}$

and

$DW1 = DW2 = DW3 = {k, r, w}$


> (b) Using the variables and domains provided in part a, find all the solutions to the constraint network by first applying the unary constraints, then using <mark style="background: #FFF3A3A6;">backtracking search</mark> employing <mark style="background: #ABF7F7A6;">forward checking</mark> and most constrained <mark style="background: #D2B3FFA6;">variable ordering</mark>. In the case of ties, choose the variable listed first. Values should be chosen lexicographically (in the order given). You will need to keep track of the call-stack in some way, so that you can backtrack and undo your inference. You may find the left hand table below helpful for tracking assignments and inferences, and the right hand table for keeping track of the call-stack.








