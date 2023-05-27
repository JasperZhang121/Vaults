
>The monkey-and-bananas domain features a monkey in a laboratory with some bananas hanging out of reach from the ceiling. A box is available that will enable the monkey to reach the bananas by pushing the box underneath them and climbing onto it. Initially, the monkey is at location A, the bananas are at location B, and the box is at location C. When the monkey climbs onto the box, the monkey’s height changes from floor to ceiling. The actions available are Go from one location to another, Push a pushable object from a location to another, ClimbUp onto or ClimbDown from a climbable object, and Grasping a graspable object. Grasping results in having the object if the monkey and object are at the same place and same height. Suppose that the goal of the monkey is to fool the scientists by getting the bananas but returning the box to its original place and then going back to its original location. Recall that STRIPS does not have negative preconditions nor negative goals.


1. Describe the problem using the STRIPS representation (operators, initial state, goal).

```
• Predicates:
  - climbable(o): o can be climbed on
  - pushable(o): o can be pushed
  - have(o): the monkey has o
  - graspable(o): o can be grasped
  - at(o,l,h): o is at location l and at height h

• Initial state s0:
  {climbable(box), pushable(box), graspable(bananas), at(monkey,A,floor), at(bananas,B,ceiling), at(box,C,floor)}

• Goal g:
  {have(bananas), at(box,C,floor), at(monkey,A,floor)}

• Operators:
  - go(l1,l2)
    * pre: {at(monkey,l1,floor)}
    * eff-: {at(monkey,l1,floor)}
    * eff+: {at(monkey,l2,floor)}

  - push(o,l1,l2)
    * pre: {pushable(o), at(monkey,l1,floor), at(o,l1,floor)}
    * eff-: {at(monkey,l1,floor), at(o,l1,floor)}
    * eff+: {at(monkey,l2,floor), at(o,l2,floor)}

  - climbUp(o,l)
    * pre: {climbable(o), at(o,l,floor), at(monkey,l,floor)}
    * eff-: {at(monkey,l,floor)}
    * eff+: {at(monkey,l,ceiling)}

  - climbDown(o,l)
    * pre: {climbable(o), at(o,l,floor), at(monkey,l,ceiling)}
    * eff-: {at(monkey,l,ceiling)}
    * eff+: {at(monkey,l,floor)}

  - grasp(o,l,h)
    * pre: {graspable(o), at(o,l,h), at(monkey,l,h)}
    * eff-: {at(o,l,h)}
    * eff+: {have(o)}

• Shortest plan:
  hgo(A,C), push(box,C,B), climbUp(box,B), grasp(bananas,B,ceiling), climbDown(box,B), push(box,B,C), go(C,A)
```

2. Give the shortest sequential plan for this problem

```
The STRIPS rule for a state s and an action a = (pre(a), eff-(a), eff+(a)) is:

s' = γ(s, a) = 
{
    (s - eff-(a)) ∪ eff+(a)   if pre(a) ⊆ s
    undefined                  otherwise (action not executable)
}

We want to compute the result s' of applying go(A,C) in s0:
- The precondition is verified: {at(monkey,A,floor)} ⊆ s0, hence the action is applicable.
- The resulting state is:
  {
    climbable(box),
    pushable(box),
    graspable(bananas),
    at(monkey,C,floor),
    at(bananas,B,ceiling),
    at(box,C,floor)
  }
```


> **ADL to STRIPS** 
> 
> *Consider the following action description, written in the ADL fragment of PDDL.* 
> 
> (:action A 
> :precondition (p)
> :effect (and (not (p)) 
> 					(when (q) (r))
> 					 (q)) 
> )

Write down a set of equivalent plain STRIPS actions (i.e., without conditional effects, and <mark style="background: #ABF7F7A6;">without disjunction or negation</mark> in the preconditions)

```
(
:action A1 
:precondition (and (p) (q)) 
:effect (and (not (p)) 
				(r)) )

(
:action A2 
:precondition (and (p) (not (q))) 
:effect (and (not (p)) 
				(q)) )

(
:action A2 
:precondition (and (p) (not-q)) 
:effect (and (not (p))
				(q) 
				(not (not-q))) )
```

2. Describe the changes that would need to be made to other actions in the domain description, the initial state and/or the goal in order to make the domain fall only within the STRIPS language.

```
Since we have introduced a new proposition in the domain we need to ensure that: 

• the initial state description contains either (q) or (not (q)), since one of them will need to be true given the closed-world assumption 

• any other action adding (q) also deletes (not (q)) 

• any other action deleting (q) also adds (not (q)) 

This assumes of course that the other actions were already STRIPS compliant, otherwise they need to be transformed in the same way as A. If the goal wasn’t STRIPS compliant and mentioned (not q), then this needs to be transformed into (not-q)
```

> ***Graph-Based Planning***
> 
> Consider the following planning problem. You want a sheep and a goat. Using your credit card, you can buy a sheep from the Automatic Sheep Machine (ASM). Waving your magic wand turns a sheep into a goat. The planning operators are as follows, where a box represents an operator whose pre-conditions are on the left-hand side and effects are on the right-hand side of the box.


1. Draw the planning graph <mark style="background: #D2B3FFA6;">until the first level leading to a plan</mark>. Include all mutex relations.

![[graph_plan.png]]

```
The mutexes are as follows:

• at level 2, W ave deletes haveS and is mutex with both ASM (inconsistence) and the maintenance action for haveS (interference + inconsistence) which have haveS as precondition (and effect for the maintenance action). 

• at level 2, haveS is mutex with haveG (inconsistent support caused by the action mutexes) 

• at level 3, W ave raemains mutex with ASM and the maintenance action for haveS for the same reasons (interference and inconsistence are static properties so these mutexes cannot disappear). 

• at level 3, the maintenance action for haveG is mutex with both W ave and the maintenance action for haveS because of competing needs (due to the mutex of haveS and haveG at level 2). 

• at level 3, the mutex between haveS and haveG has disappeared because haveG can now be produced by its maintenance action, which is not mutex with ASM.
```

2. State at which levels Graphplan would attempt extraction.

`Extraction would only be attempted at level 3 because the goal ({haveS, haveG}) is mutex at level 2. 3.`

3. Highlight in the graph the plan extracted by Graphplan (include the maintenance actions chosen).

`The actions, propositions and links that are part of the final plan are in bold face.`

4. Given the built planning graph structure, provide at least two heuristics that can be computed from the graph and then used in an informed state space heuristic search framework (e.g., A*). Report the estimated distance to the goal in the initial state for each heuristic.

`A first heuristic can be computed by taking the index of the smallest level in which all the goal propositions have all appeared (h1). Another, more informed heuristic is the index of the first level in which all the goal propositions appear and are not mutex (h2). Therefore h1(s0, G) = 2 and h2(s0, G) = 3.`

