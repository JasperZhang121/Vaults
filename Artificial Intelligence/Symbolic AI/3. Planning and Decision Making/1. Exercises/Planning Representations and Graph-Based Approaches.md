
> This is part of exercise 10.3 from the book. The monkey-and-bananas domain features a monkey in a laboratory with some bananas hanging out of reach from the ceiling. A box is available that will enable the monkey to reach the bananas by pushing the box underneath them and climbing onto it. Initially, the monkey is at location A, the bananas are at location B, and the box is at location C. When the monkey climbs onto the box, the monkey’s height changes from floor to ceiling. The actions available are Go from one location to another, Push a pushable object from a location to another, ClimbUp onto or ClimbDown from a climbable object, and Grasping a graspable object. Grasping results in having the object if the monkey and object are at the same place and same height. Suppose that the goal of the monkey is to fool the scientists by getting the bananas but returning the box to its original place and then going back to its original location. Recall that STRIPS does not have negative preconditions nor negative goals.


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
s' = γ(s, a) = {
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



> Consider the following action description, written in the ADL fragment of PDDL. 
> (:action A 
> :precondition (p)
> :effect (and (not (p)) 
> 					(when (q) (r))
> 					 (q)) 
> )



