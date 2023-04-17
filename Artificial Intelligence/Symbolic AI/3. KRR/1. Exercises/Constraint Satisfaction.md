
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

Consider a situation in which you want to bake two dishes d1 and d2 that cannot both be in the oven simultaneously. You will get home by 6:00pm on the day, and you have to prepare the food under certain constraints: 
1. Both dishes take 35-40 minutes to cook (i.e., each dish must be put in the oven and taken out between 35 and 40 minutes later), 
2. d1 requires 30 minutes preparation time, and must cool for at least 10 minutes before being served, 
3. you must serve dinner by 7:30pm, 
4. you cannot start any cooking or preparation until at least 6:00pm, with the exception of the preparation for d1, which you can choose to do either after 6:00pm or on the previous day, 
5. neither dish can go into the oven for the first 15 minutes after 6:00pm today while you wait for the oven to pre-heat. 

Formulate this problem as a constraint network γ = (V, D, C), defining variables, domains and constraints following the notation used in the lectures. Think carefully about what the variables should be, and what they represent. Constraints can be defined either explicitly— specifying the allowed pairs of values—or in some more compact notation that summarises the allowed pairs of values. Note: This can be designed as a very simple constraint network if you abstract away the times, but the idea of this course is to let the computer do the logic and reasoning, so try to construct a constraint network using the times listed.

```
The variables we will use for this problem are time points at which events occur, and the values they take are the times since 6:00pm. We can use a resolution of 5 minutes here, because all the time periods are divisible by 5, and no constraints require a gap more than 0 minutes and less than 5 minutes between events. For the variables, we have:

V = {prep1 , in-oven1, in-oven2, out-oven1, out-oven2,serve}




```