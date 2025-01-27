# 1.1.18 Graphic Seuence
- A sequence $\mathbf{d}=(d_1, d_2, \cdots, d_n)$ is graphic if there is a simple graph with degree sequence $\mathbf{d}$. Show that:
- a) the sequences (7, 6, 5, 4, 3, 2) and (6, 6, 5, 4, 3, 3, 1) are not graphic,
  - the sequence (7, 6, 5, 4, 3, 2), the sum is odd. then it  cannot make graph. ref. [[Graphs and Their Representation#1.1.16 Degree Sequence|1.1.16]]
  - consider the sequence (6, 6, 5, 4, 3, 3, 1), $d_1=d_2=6$, but $V(G)=n=7$, if $d_1$ and $d_2$ does not have a loop, then all other vertices has at leat degree 2. but this sequence's $d_7=1$, then it is not simple graph.
# Erdős–Gallai theorem
- b) if $\mathbf{d}=(d_1, d_2, \cdots, d_n)$ is *graphic* and $d_1\ge d_2\ge \cdots \ge d_n$, then $\sum_{i=1}^n d_i$ is even and $$\sum_{i=1}^n d_i\le k(k-1)+\sum_{i=k+1}^n \operatorname{min}\{k, d_i\}, 1\le k\le n$$
- (Erdos and Gallai (1960) showed that these necessary conditions for a sequence
to be graphic are also sufficient.) ref. https://en.wikipedia.org/wiki/Erd%C5%91s%E2%80%93Gallai_theorem
## Proofs
- It is not difficult to show that the conditions of the Erdős–Gallai theorem are necessary for a sequence of numbers to be graphic. The requirement that the sum of the degrees be even is the [[Graphs and Their Representation#Equation 1.1 Handshaking Lemma|Handshaking Lemma]], already used by Euler by in his 1736 paper on the bridges of Königsberg. 
- The inequality between the sum of the $k$ largest degrees and the sum of the remaining degrees can be established by double counting: the left side gives the numbers of edge-vertex adjacencies among the $k$ highest-degree vertices, each such adjacency must either be on an edge with one or two high-degree vertices, each such adjacency must either be on an edge with one or two hige-degree endpoints, the $k(k-1)$ term on the right gives the maximum possible number of edge-vertex adjacencies in which both endpoints have high degree, and the remaining term on the right upper bounds the number of edges that have exactly one high degree endpoint. Thus, the more difficult part of the proof is to show that, for any sequence of numbers obeying these conditions, there exists a graph for which it is the degree sequence. 
- 1. Pick the top $k$ vertices. 
  - Consider thOe $k$ vertices with the largest degrees, $v_1, \cdots, v_k$ in the chosen labeling-these are precisely the vertices with the largest degrees $d_1, \cdots, d_k$. Denote this set by $S$. The complement set is $T:=V(G)\setminus S$  , of size $n-k$.
- 2. Counting Edges Incident to $S$
  - The sum $\sum_{i=1}^k k_i$ can be interpreted as:$$\sum_{i=1}^k d(v_i)=\underbrace{2e(S)}_{\text{edges inside S}}+\underbrace{e(S, T)}_{\text{edges from S to T}}$$
  - $e(S)$ denotes the number of edges within the set $S$. Each such edge is counted twice(once in the degree of each of its two endpoints).
  - $e(S, T)$ denotes the number of edges from a vertex in $S$ to a vertex in $T$. Each such edge is counted once in $\sum_{i=1}^k d(v_i)$.
- 3. Bounding $e(S)$ and $e(S, T)$
  - Edges within $S$: Since $S$ has $k$ vertices, the maximum number of edges in a simple graph on $S$ is $\binom{k}{2}=\frac{k(k-1)}{2}$. Therefore, $e(S)\le \binom k2 =\frac{k(k-1)}{2}$.
  - Edges from $S$ to $T$: For each vertex $v_j$ in $T$, its total degree is $d_j$. But it can connect to at most $\operatorname{min}(k, d_j)$, vertices in $S$:
    - Obviously, a vertex cannot connect to more vertices in $S$ than are in $S$ (that is, at most $k$).
    - Also, it cannot have more neighbors than its own degree $d_j$.
 - Summing over all $v_j\in T$ (that is $j=k+1, \cdots, n$) gives: $e(S,T)\le\sum_{j=k+1}^n\operatorname{min}\{k, d_j\}$
- 4. Combine the Two Bounds
  - Putting these pieces together, we have:$$\sum_{i=1}^k d_i=2e(S)+e(S,T)\le 2\cdot\frac{k(k-1)}{2}+\sum_{j=k+1}^n\operatorname{min}\{k,d_j\}$$
  - $\sum_{i=1}^k d_i\le k(k-1)+\sum_{j=k+1}^n\operatorname{min}\{k,d_j\}$

# 1.1.19 Havel-Hakimi-type
- Let $\mathbf{d}=(d_1,d_2,\cdots,d_n)$ be a nonincreasing sequence of nonnegative integers. Set $\mathbf{d}':=(d_2-1, d_3-1, \cdots, d_{d_1+1}-1, d_{d_1+2}, \cdots, d_n)$.
- a) Show that $\mathbf{d}$ is graphic if and only if $\mathbf{d}'$ is graphic.
- if $d_1\le d_2\le \cdots \le d_n\implies d_2-1\le d_3-1\le \cdots\le d_n-1$
- and $d_{d_1+1}\le d_{d_1+2}\le \cdots$
- 