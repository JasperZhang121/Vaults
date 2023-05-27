> Consider the following constraint network γ = (V, D, C): 

- Variables: V = {a, b, c}. 
- Domains: For all v ∈ V : Dv = {1, 2, 3, 4, 5, 6, 7, 8, 9}. 
- Constraints: a = 2b; a + 1 = c; b + c ≤ 6. 

(a) Draw a <mark style="background: #FFF3A3A6;">constraint graph</mark> for this constraint network. It may help to also label each constraint with its definition when completing the next exercise.

![[ac-3 constraint.png]]


(b) Run the AC-3(γ) algorithm, as specified in the lecture. Precisely, for each iteration of the while-loop, give the content of M at the start of the iteration, give the pair (u, v) removed from M, give the domain of u after the call to Revise(γ, u, v), and give the pairs (w, u) added into M.

```
(a) M content: {(a, b),(a, c),(b, a),(b, c),(c, a),(c, b)}; pair selected: (a, b); Da = {2, 4, 6, 8}; Candidate (c, a) already present in M.

(b) M content: {(a, c),(b, a),(b, c),(c, a),(c, b)}; pair selected: (a, c); Da = {2, 4, 6, 8}; No change, so nothing added into M.

(c) M content: {(b, a),(b, c),(c, a),(c, b)}; pair selected: (b, a); Db = {1, 2, 3, 4}; Candidate (c, b) already present in M.

(d) M content: {(b, c),(c, a),(c, b)}; pair selected: (b, c); Db = {1, 2, 3, 4}; No change, so nothing added into M.

(e) M content: {(c, a),(c, b)}; pair selected: (c, a); Dc = {3, 5, 7, 9}; Candidate (b, c) added into M.

(f) M content: {(c, b),(b, c)}; pair selected: (c, b); Dc = {3, 5}; Candidate (a, c) added into M.

(g) M content: {(b, c),(a, c)}; pair selected: (b, c); Db = {1, 2, 3}; Candidate (a, b) added into M.

(h) M content: {(a, c),(a, b)}; pair selected: (a, c); Da = {2, 4}; Candidate (b, a) added into M.

(i) M content: {(a, b),(b, a)}; pair selected: (a, b); Da = {2, 4}; No change, so nothing added into M.

(j) M content: {(b, a)}; pair selected: (b, a); Db = {1, 2}; Candidate (c, b) added into M.

(k) M content: {(c, b)}; pair selected: (c, b); Dc = {3, 5}; No change, so nothing added into M.

(l) M empty; return modified γ with reduced domains.
```

> The Queens Problem is usually stated in terms of a standard 8 × 8 chessboard, probably because a board of that size looks familiar. However, it can be formulated similarly for boards of other sizes, including smaller ones. To the right, one solution to the 5 queens problem is shown (that is, 5 queens are placed on a <mark style="background: #FFB8EBA6;">5×5 chessboard</mark> without any attacking each other). How many other solutions to the 5 queens problem can you find just by <mark style="background: #ADCCFFA6;">exploiting symmetries</mark> in the problem? Identify at least one additional constraint which would <mark style="background: #D2B3FFA6;">allow the solution to the right, but would remove some of the symmetries</mark>. Make sure they only remove symmetrical solutions. Specifically, if some solution violates the new constraints, there must be another solution symmetrical to it which does not.

Use here the standard encoding that there are 5 variables {q1, q2, ..., q5} where the domain of each is {1, ..., 5}, and qi represents the queen in column (file) i, and the value of qi is which row (rank) that queen is in.

Some example constraints that break some of the symmetry are:

1. q1 < q5
    
    - This constraint breaks symmetry both vertically and horizontally. Given a solution a and its reflection a', a' breaks constraint 1 if and only if a does not break the constraint.
2. q1 ≤ 3 ∧ q1 = 3 → q2 < 3
    
    - This constraint breaks horizontal symmetry. It specifies that if q1 is less than or equal to 3 and q1 equals 3, then q2 must be less than 3.
3. q3 ≤ 3 ∧ q3 = 3 → q2 < 3
    
    - This constraint is similar to constraint 2 but also breaks 180° symmetry when q3 is not equal to 3.


>Constraint problems involving time are often formalized as Temporal Constraint Networks. To define a constraint network, we need a set of time points H and intervals between them. The time points we will use are:

$$H = {start, prep-d1, d1-in, d1-out, d2-in, d2-out, serve}$$

We will start by considering a simple version, using the following constraints:

1. Both dishes take 35-40 minutes to cook, meaning each dish must be put in the oven and taken out between 35 and 40 minutes later.
    
2. d1 must cool for at least 10 minutes before being served.
    
3. d1 requires 30 minutes of preparation time.
    
4. You must serve dinner by 7:30pm.
    
5. You cannot start preparation until 6:00pm.
    
6. Neither dish can go into the oven for the first 15 minutes after 6:00pm today, while you wait for the oven to pre-heat.
    
7. d1 must come out of the oven before d2 goes into the oven, meaning d2 must come out of the oven after d1 goes into the oven.

> Considering only constraint 2 with the serve and d1-out time points, and assuming that serve is at 7:30pm, what is the latest that d1-out could occur?

`Based on the constraint, d1-out can happen anywhere between 10 and ∞ minutes before serve. The latest it can be served is then 10 minutes before. Note that this does not depend on the lower bound at all, so if the constraint was, between 10 and 20 minutes before, the latest would still be 10 minutes before. Hence, the answer is 7:20pm.`


> What if you take into account all the constraints between the time points d1-in, d1-out, d2-in, d2-out, serve? (i.e., <mark style="background: #BBFABBA6;">leaving out start and prep-d1</mark>). You will need to work iteratively.

`6:55pm, because the constraint that it has to come out before d2 goes in supersedes constraint 2. Adding up the time differences in the upper bounds gives us a time difference of −35.`

> Construct the distance graph for the simplified temporal constraint network in part b. In this graph, find the shortest path (which might be negative) from serve to d1-out using only the time points from b. As you’re doing this, can you convince yourself that this is exactly the same as part b?

![[simple_distance_graph.png]]

First, you find the path of length -10 (i.e., d1-out must occur 10 minutes before serve or earlier), which is the same as finding that it must happen at or before 7:20pm. Upon finding the shortest paths to d2-out and d2-in, the shortest path is updated to serve → d2-out → d2-in → d1-out with a length of -35, corresponding to the solution of 6:55pm in part b.

Given two times a [l, u] → b, the latest a can happen with respect to b is -l minutes earlier, and the latest that b can happen with respect to a is u minutes later.

> Is the full simple temporal network consistent (i.e., with all the time points and constraints above)? Explain why/why not.

No, this can be seen by just <mark style="background: #FFF3A3A6;">adding up the times</mark> that happen in order. If you start at 6:00pm and have to put d1 in first, you have to do 30 minutes of preparation (ending 6:30), then put d1 in for 35 to 40 minutes (ending 7:05 to 7:10), leaving only 25-20 minutes to cook d2 before 7:30pm, but it takes 35-40 minutes. Therefore there isn’t enough time to do things in this order.

> What is the shortest path in the full distance graph from start to serve. Or equivalently, what is the latest you can serve assuming you start at 6:00pm.

![[full_d_graph.png]]








