
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


