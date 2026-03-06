# Adaptive Branch & Bound for Graph Coloring

A density-based adaptive solver for the Graph Coloring Problem (GCP).
The solver automatically classifies each instance by its density and
applies the most suitable Branch & Bound variant among three specialized
DSATUR algorithms. A comparison with Lawler's Dynamic Programming
approach completes the study.

---

## Authors

SID02 - Team 02

- Merabtine Aya Malek
- Kebbal Malek
- Koroghli Fadhila
- Diaf Lina
- Malek Yasmine


---

## Problem Description

The Graph Coloring Problem consists of assigning a color to each vertex
of a graph G = (V, E) such that no two adjacent vertices share the same
color, while minimizing the total number of colors used. This minimum
is called the chromatic number, denoted chi(G). The problem is
NP-hard: no polynomial exact algorithm is known.

---

## Adaptive Strategy

The solver classifies each instance by its density d(G) = 2|E| / |V|(|V|-1)
and selects the most appropriate algorithm automatically.

| Class   | Density          | Algorithm          | Reference                      |
|---------|------------------|--------------------|--------------------------------|
| Sparse  | d < 0.3          | DSATUR VB1         | Brelaz (1979)                  |
| Medium  | 0.3 <= d < 0.7   | DSATUR VB2 + O2    | Mendez-Diaz & Zabala (2006)    |
| Dense   | d >= 0.7         | DSATUR chi_GA      | Furini, Gabrel & Ternier (2016)|

---

## Algorithms

### Algorithm 1 - DSATUR VB1 (sparse graphs, d < 0.3)

- **Lower bound**: greedy clique heuristic in O(n^2)
- **Upper bound**: single-start DSATUR heuristic + graph reduction
- **Node selection**: maximum DSAT, tie-break by total degree
- **Exploration**: ascending color order 1, 2, ..., k+1

### Algorithm 2 - DSATUR VB2+O2 (medium density, 0.3 <= d < 0.7)

- **Lower bound**: greedy clique heuristic in O(n^2)
- **Upper bound**: multi-start DSATUR heuristic
- **Node selection**: maximum DSAT, tie-break by residual degree
- **Exploration**: O2 order — new color first, then existing colors
- **Enhanced pruning**: residual clique bound omega_R

### Algorithm 3 - DSATUR chi_GA (dense graphs, d >= 0.7)

- **Lower bound**: Bron-Kerbosch exact clique with timeout
- **Upper bound**: DSATUR heuristic with clique-priority ordering
- **Strong bound**: chi_GA via Cornaz & Jost theorem (2008)
  - alpha(G_A) + chi(G_C) = |V_C|  =>  chi_GA = |V_C| - alpha(G_A)
  - alpha(G_A) computed by MIP integer programming
- **Node selection**: maximum DSAT, tie-break by initial clique ordering
- **Cost control**: phi_{u,g} strategy with optimal parameters u=2, g=1

### Dynamic Programming - Lawler (1976)

Exact algorithm based on the recurrence:

  chi(G[S]) = 1 + min over I in IS(G[S]) of chi(G[S \ I]),   chi({}) = 0

- Time complexity: O(2.4423^n), space: O(2^n)
- Practical limit: n <= 25 vertices
- Compared with B&B on three criteria: predictability (CV%),
  worst-case ratio (t_max / t_mean), and structural independence

---

## Benchmark Instances

All instances are from the DIMACS benchmark suite.

| Instance      | n   | Density | chi(G) | Class  |
|---------------|-----|---------|--------|--------|
| DSJC125.1.col | 125 | 0.095   | 5      | Sparse |
| myciel3.col   | 11  | 0.364   | 4      | Medium |
| school1.col   | 385 | 0.258   | 14     | Sparse |
| DSJC125.9.col | 125 | 0.898   | 44     | Dense  |

DIMACS instances can be downloaded from:
https://mat.tepper.cmu.edu/COLOR/instances.html

---

## Requirements
```
python >= 3.8
jupyter
networkx
numpy
scipy
matplotlib
```

Install dependencies:
```bash
pip install jupyter networkx numpy scipy matplotlib
```

---

## Usage

Launch the notebook:
```bash
jupyter notebook TP_Methodes_Exactes_BB_SID02_Eq02.ipynb
```

Place the DIMACS `.col` instance files in the same directory as the
notebook before running, or update the file paths in the configuration
cells.

---


## References

- Brelaz, D. (1979). New methods to color the vertices of a graph.
  Communications of the ACM, 22(4), 251-256.
  https://doi.org/10.1145/359094.359101

- Mendez-Diaz, I. & Zabala, P. (2006). A Branch-and-Cut algorithm for
  graph coloring. Discrete Applied Mathematics, 154(5), 826-847.
  https://doi.org/10.1016/j.dam.2005.05.021

- Furini, F., Gabrel, V. & Ternier, I.-C. (2016). An Improved
  DSATUR-Based Branch-and-Bound Algorithm for the Vertex Coloring
  Problem. INFORMS Journal on Computing, 29(4), 686-701.
  https://www.researchgate.net/publication/309817697_An_Improved_DSATUR-Based_Branch-and-Bound_Algorithm_for_the_Vertex_Coloring_Problem

- De Lima, A.M. & Carmo, R. (2018). Exact Algorithms for the Graph
  Coloring Problem. Revista de Informatica Teorica e Aplicada (RITA),
  25(4), 57-73.
  https://doi.org/10.22456/2175-2745.80721


---

This project was developed as part of the TOIA course practical work.
