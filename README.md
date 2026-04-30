## Motivation

Designing road infrastructure under financial constraints is one of the most consequential decisions in urban planning. A city planner must decide which roads to build given a fixed budget, with the goal of connecting as many people and localities as possible. Spending too little in a region leaves communities isolated; concentrating the entire budget in one area starves the rest of the network. Finding a principled, computationally efficient way to allocate a limited budget across a road network is therefore both practically urgent and algorithmically interesting.

This project approaches that question through the lens of combinatorial optimisation. Cities are modelled as graph nodes, possible roads as weighted edges where weight represents construction cost, and the question becomes:

> Given a budget $B$, which subset of edges maximises the number of pairs of nodes that can reach each other?

The broader motivation goes beyond road networks. The same structure appears whenever resources must be allocated to maximise reachability in a network, whether in telecommunications, supply chains, or last-mile connectivity. While the analysis here focuses on road infrastructure, the algorithmic insights extend to these domains as well.

---

## Problem Formulation

Let $G = (V, E)$ be an undirected graph where:
- $V$ is the set of nodes (intersections or localities)
- $E$ is the set of candidate edges (possible roads)

Each edge $e \in E$ has an associated cost $c(e) \in \mathbb{R}^+$, and there is a fixed budget $B > 0$.

### Objective

Select a subset of edges $X \subseteq E$ such that the total number of reachable node pairs is maximised:

$$
\max_{X \subseteq E} \; f(X) = \sum_{C \in \mathcal{C}(X)} \binom{|C|}{2}
$$

where:
- $\mathcal{C}(X)$ is the set of connected components of the subgraph $(V, X)$
- $\binom{|C|}{2} = \frac{|C|(|C|-1)}{2}$ counts the number of unordered reachable pairs within component $C$

### Constraint

$$
\sum_{e \in X} c(e) \leq B
$$

### Key Property

The objective function $f(X)$ is **monotone submodular**:
- Adding an edge never decreases connectivity (monotonicity)
- The marginal gain from adding an edge diminishes as the network grows (submodularity)

This structure makes greedy algorithms natural and theoretically justified candidates for approximation.
