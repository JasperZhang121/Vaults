
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
1. Q(x) ∨ P(x) given 
2. ¬P(y) given 
3. Q(x) from 1, 2 unifier {y ← x} 
4. ¬Q(a) ∨ ¬Q(b) given 
5. ¬Q(b) from 3, 4 {x ← a} 
6. ⊥ from 3, 5 {x ← b}
