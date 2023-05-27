
>Consider the following delivery problem. Two driverless trucks (T1 and T2) can autonomously deliver the products that customers have requested. The products are initially at the depot (location A), and our trucks can reach the customers identified by their locations (B,C,D,E,F) by following suitable paths connecting adjacent locations. Just two out of these five customers (F and D) have to be served. Assume for simplicity that the two trucks have always enough products on-board. Ignore any cost that could be associated to the actions. A typed STRIPS description of the problem is as follows:

Types:

-   Locations = {A, B, C, D, E, F}
-   Trucks = {T1, T2}

Propositions:

-   at(t, x), where x ∈ Locations and t ∈ Trucks
-   delivered(x), where x ∈ Locations
-   adjacent(x, y), where x, y ∈ Locations

Actions:

-   go(t, x, y), where t ∈ Trucks, x, y ∈ Locations
    
    -   Preconditions: {at(t, x), adjacent(x, y)}
    -   Add-list: {at(t, y)}
    -   Delete-list: {at(t, x)}
    
-   deliver(t, x), where t ∈ Trucks, x ∈ Locations
    
    -   Preconditions: {at(t, x)}
    -   Add-list: {delivered(x)}
    -   Delete-list: ∅

Initial state: {at(T1, A), at(T2, A)} ∪ {adjacent(x, y) | x, y ∈ Locations, there is an edge between x and y in the graph of Figure 1}

Goal: {delivered(F), delivered(D), at(T1, A), at(T2, A)}

1. Explain the concepts of the delete relaxation of a problem P and of a relaxed-plan for P.

`The delete relaxation P + of a problem the same problem as P where all actions have been transformed by removing their delete list. A relaxed plan is a (not necessarily optimal) solution to the delete-relaxation P +. As an aside: P + is simple problem than P hence an optimal relaxed plan cost underestimates the true cost and can be used as an admissible heuristic. One issue is that P + is NP-hard to solve optimally. Therefore, many approaches further relax the problem to get an admissible heuristics (e.g. the admissible hmax) or compute non-optimal relaxed plans and non-admissible heuristics (e.g. the FF heuristic).`

#### NP hard
"NP-hard" stands for <mark style="background: #FFB8EBA6;">nondeterministic polynomial-time hard</mark>. It is a complexity class in computer science that represents a set of problems that are at least as hard as the hardest problems in the class NP (nondeterministic polynomial-time).
A problem is classified as NP-hard if it has the property that every problem in the class NP can be reduced to it in polynomial time. In other words, if a problem X is NP-hard, then any problem Y in NP can be transformed into an instance of X in polynomial time, preserving the solution.

For example, consider the following boolean formula:
$$F = (x1 ∨ ¬x2 ∨ x3) ∧ (¬x1 ∨ x2 ∨ x4) ∧ (¬x1 ∨ ¬x3 ∨ x4)$$
The problem is to find an assignment of truth values to variables x1, x2, x3, and x4 such that the formula F evaluates to true.

<mark style="background: #FFB8EBA6;">SAT is known to be NP-hard because any problem in the class NP can be reduced to SAT in polynomial time</mark>. This means that if we can solve SAT efficiently, we can solve any problem in NP efficiently.


2. Write the optimal relaxed plan for this problem. What are the values of h + and h ∗ at the initial state?

```
The easiest way to answer the question is to recall that, in the delete-relaxation, once a proposition becomes true, then it remains true forever, and that an action that was once applicable remains applicable for ever. In the present case, a relaxed plan can assume that a truck is at a location that it already visited, in particular, location A. So that as soon as a truck has delivered the first item (for instance at location D), it can assume it is back in A.

• Optimal Relaxed Plan: h go(T1,A,B), go(T1,B,C), go(T1,C,D), deliver(T1,D), go(T2,A,E), go(T2,E,F), deliver(T2,F)

• h+ (s0) = 7 

• Note: The same plan with T2 replaced with T1 would still be a relaxed plan since when T1 is back in A, it can go on delivering to F.

• Optimal Sequential Plan: h go(T1,A,B), go(T1,B,C), go(T1,C,D),deliver(T1,D), go(T1,D,C), go(T1,C,B), go(T1,B,A), go(T2,A,E), go(T2,E,F), deliver(T2,F), go(T2,F,E), go(T2,E,A)i. 

• h ∗ (s0) = 12 (optimal plan)
```


3. Write a partially ordered plan and a parallel plan solving the problem.

```
Partial Order Plan: 

in this case, it corresponds to the two trucks operating asynchronously 

<
{ go(T1,A,B), go(T1,B,C), go(T1,C,D), deliver(T1,D), go(T1,D,C), go(T1,C,B), go(T1,B,A), go(T2,A,E), go(T2,E,F), deliver(T2,F), go(T2,F,E), go(T2,E,A)}, 

{go(T1,A,B) ≺ go(T1,B,C),
go(T1,B,C) ≺ go(T1,C,D),
go(T1,C,D) ≺ deliver(T1,D), 
deliver(T1,D) ≺ go(T1,D,C), 
go(T1,D,C) ≺ go(T1,C,B), 
go(T1,C,B) ≺ go(T1,B,A), 
go(T2,A,E) ≺ go(T2,E,F), 
go(T2,E,F) ≺ deliver(T2,F), 
deliver(T2,F) ≺ go(T2,F,E), 
go(T2,F,E) ≺ go(T2,E,A) }
>

Parallel Plan: there are many possibilities, for instance
<{ go(T1,A,B), go(T2,A,E)}, 
{go(T1,B,C), go(T2,E,F)}, 
{go(T1,C,D), deliver(T2,F)}, 
{deliver(T1,D)}, 
{go(T1,D,C), go(T2,F,E)}, 
{go(T1,C,B), go(T2,E,A)}, 
{go(T1,B,A)}>
```


> Consider a small propositional STRIPS planning problem with a set of propositions P = {p, q, r, s}, and actions a1 . . . a6. The preconditions and effects of the actions are described in the following table:

| Action | pre    | eff+  | eff- |
|--------|--------|-------|------|
| a1     | {p, q} | {r}   | {p}  |
| a2     | {q}    | {p}   | {q}  |
| a3     | {p, q} | {s}   | {q}  |
| a4     | {r}    | {s}   | {q}  |
| a5     | {r, s} | {q}   | {r}  |
| a6     | {s}    | {r}   | {s}  |

1. Let g be a goal in regression (backward search) planning, and a be a propositional STRIPS action such that a = <pre(a), eff+(a), eff−(a)>. State the condition under which a is relevant to g and explain how to compute the regression of g through a.

```
An action a is relevant for goal g iff: 

• g ∩ eff+(a) 6= ∅, i.e. if it achieves at least one of g’s propositions 

• g ∩ eff−(a) = ∅, i.e. it does not destroy one of g’s propositions

If a is relevant for g then the regression of g through a is defined and is: g 0 = γ −1 (g, a) = (g\eff+(a))∪pre(a).
```

2. For each of the 6 actions described above, state whether the goal {r, s} can be regressed through that action, and if yes, what the result is.

```
• a1: yes, g 0 = {p, q, s} 
• a2: no, the action does not achieve a proposition in g 
• a3: yes, g 0 = {p, q, r} 
• a4: yes, g 0 = {r} 
• a5: no, the action deletes a proposition in g 
• a6: no, the action deletes a proposition in g
```

(backward process)

> Examine the following partial plan. As in the previous tutorial, the goal is to have a sheep and a goat. The ASM (automated sheep machine) operator yields a sheep if you have an ASM card. The Wave operator waves your magic wand to turn a sheep into a goat.

1. Which conditions are open (indicate both the condition and the operator for which it is open).
`Have(S) for Wave and Have(C) for ASM`

2. List all threats (indicate both the causal link threatened and the threatening action). 
`The threatening action is Wave and the link threaten is that going from ASM to condition Have(S) of End.`

Tips:
`In the context of planning, "threats" refer to situations where a causal link between an action and a proposition (effect) is at risk of being invalidated or undone by another action`

3. State how those threats can be resolved.
`only one possibility: order Wave before ASM.`

4. Draw the final plan that the plan-space planning algorithm would produce. Include all new operators, causal links, and ordering constraints required (except those implied by the causal links).

![[plan_partial.png]]

5. Suppose you are executing the resulting plan. What conditions must be true just prior to executing the Wave operator in order for the plan to reach the goal?
`Have(C) and Have(S)`

