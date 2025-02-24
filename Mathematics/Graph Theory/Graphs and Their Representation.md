# Definitions and Examples
 Many real-world situations can conveniently be described by means of a diagram consisting of a set of points together with lines joining certain pairs of these points. For example, the points could represent people, with lines joining pairs of friends; or the points might be communication centres, with lines representing communication links. Notice that in such diagrams one is mainly interested in whether two given points are joined by a line; the manner in which they are joined is immaterial. A mathematical abstraction of situations of this type rise to the concept of a graph.
  A *graph G* is an ordered pair $(V(G), E(G))$ consisting of a set $V(G)$ of *vertices* and a set $E(G)$, disjoint from $V(G)$, of *edges*, together with an $\text{incidence function }\psi_G$ that associates with each edge of $G$ an unordered pair of (not necessarily distinct) vetices of $G$. If $e$ is an edge and $u$ and $v$ are vertices such that  $\psi_G(e)=\{u, v\}$, then $e$ is said to *join* $u$ and $v$, and the vertices $u$ and $v$ are called the *ends* of $e$. We denote the numbers of vertices and edges in $G$ by $v(G)$ and $e(G)$; these two basic parameters are called the *order* and *size* of $G$, respectively. 
   Two examples of graphs should serve to clarify the definition. For notational simplicity, we write $uv$ for the unordered pair $\{u, v\}$.
   ## Example 1.
   $$G=(V(G), E(G))$$
   where
   $$\begin{align}
   V(G)&=\{u, v, w, x, y\}\\
   E(G)&=\{a, b, c, d, e, f, g, h\}
   \end{align}$$
   and $\psi_G$ is defined by
   $$\begin{array}{4}
   ψ_G(a) = uv& ψ_G(b) = uu & ψ_G(c) = vw& ψ_G(d) = wx\\
   ψ_G(e) = vx &ψ_G(f) = wx &ψ_G(g) = ux &ψ_G(h) = xy
   \end{array}$$
# Drawings of Graphs
 Graphs are so named because they can be represented graphically, and it is this graphical representation which helps us understand many of their properties. Each vertex is indicated by a point, and each edge by a lne joining the points representing its ends. 
  There is no single correct way to draw a graph; the relative positions of points representing vertices ad the shapes of lines representing edges usually have no significance. 
   Most of the definitions and concepts in graph theory are suggested by this graphical representation. The ends of an edge are said to be *incident* with the edge, and *vice versa*. Two vertices which are incident with a common edge are adjacent, as are two edges which are incident with a commen vertex, and two distinct adjacent vertices are *neighbours*. The set of neighbours of a vertex $v$ in a graph $G$ is nodoted by $N_G(v)$.
   ###### Figure 1.1.
![[Pasted image 20250111210600.jpg]]
- An edge with identical ends is called a *loop*, and an edge with distinct ends a *link*. Two or more links with the same pair of ends are said to be *parallel edges*.In the graph $G$ of [[Graphs and Their Representation#Figure 1.1.|Figure 1.1]], the edge $b$ is a *loop*, and all other edges are links; the edges $d$ and $f$ are parallel edges.
- Throughout the book, the letter $G$ denotes a graph. Moreover, when there is no scope for ambiguity, we omit the letter $G$ from graph-theoretic symbols and write, for example, $V$ and $E$ instead of $V(G)$ and $E(G)$. In such instances, we denote the numbers of vertices and edges of $G$ by $n$ and $m$, respectively.
- A graph is *finite* if both its vertex and edge set are finite. In this book, we mainly study finite graphs, and the term 'graph' always means 'finite graph'. The graph with no vertices (and hence no edges) is the *null graph*. Any graph with just one vertex is referred to as *trivial*. All other graphs are *nontrivial*. We admit the null graph solely for mathematical convenience. Thus, unless otherwise specified, all graphs under discussion should be taken to be nonnull.
# Special Families of Graphs
## complete graph
- Certain types of graphs play prominent roles in graph theory. A ==*complete== graph* is a simple graph in which any two vertices are adjacent, an *empty graph* one in which no two vertices are adjacent (that is, one whose edge set is empty). 
## Bipartite graph
- A graph is *bipartite* if its vertex set can be partitioned into two subsets $X$ and $Y$ so that every edge has one end in $X$ and one end in $Y$; such a partition $(X, Y)$ is call ==*bipartition*== of the graph, and $X$ and $Y$ its *parts*. We denote a bipartite graph $G$ with bipartition $(X, Y)$ by $G[X, Y]$.
- No edge connects two vertices within $X$.
- No edge connects two vertices within $Y$.
### Complete Bipartite Graph
- If $G[X, Y]$ is simple and every vertex in $X$ is joined to every vertex in $Y$, then $G$ is called a *complete bipartite graph*. A *star* is a complete bipartite graph $G[X, Y]$ with $|X|=1$ or $|Y|=1$. [[Graphs and Their Representation#Figure 1.2|figure 1.2]] shows diagrams of a complete graph, a complete bipartite graph, and a star.
###### Figure 1.2
![[Pasted image 20250110211649.jpg]]
## Path
- A *==path*== is a simple graph whose vertices can be arranged in a linear sequence $v_1, v_2, \cdots , v_k$ in such a way that two vertices are adjacent if they are consecutive in the sequence.
- vertices that are not consecutive are nonadjacent. 
## Cycle
- Likewise, a ==*cycle*== on three or more vertices is a simple graph whose vertices can be arranged in a cyclic sequence in such a way that two vertices are adjacent if they are consecutive in the sequence, and are nonadjacent otherwise; a cycle on one vertex consists of a single vertex with a loop, and a cycle on two vertices consists of two vertices joined by a pair of parallel edges. The ==*length*== of a path or a cycle is the number of its edges. A path or cycle of length $k$ is called a *k-path* or *k-cycle*, respectively; the path or cycle is *odd* or *even* according to the parity of $k$. A 3-cycle is often called a *triangle*, a 4-cycle a *quadrilateral*, a 5-cycle a *pentagon*, a 6-cycle a *hexagon*, and so on. [[Graphs and Their Representation#Figure 1.3|Figure 1.3]] epicts a 3-path and a 5-cycle.
###### Figure 1.3
![[Pasted image 20250110213155.jpg]]


- As observed earlier, examples of graphs abound in the real world. Graphs also arise naturally in the study of other mathematical structures such as polyhedra, lattices, and groups. There graphs are generally defined by means of an adjacency rule, prescribing which unordered pairs of vertices are edges and which are not. A number of such examples are given in the exercises at the end of this section and in Section 1.3.
- For the sake of clarity, we observe certain conventions in representing graphs by diagrams: we do not allow an edge to intersect itself, nor let an edge pass through a vertex that is not an end of the edge; clearly, this is always possible. However, two edges may intersect at a point that does not correspond to a vertex, as in the drawings of the first two graphs in [[Graphs and Their Representation#Figure 1.2|Figure 1.2]]. A  graph which can be drawn in the plane in such a way that edges meet only at points corresponding to their common ends is called a *planar graph*, and  such a drawing is called a *planar embedding* of the graph. For instance, the graphs $G$ and $H$ of Examples 1 and 2 are both planar, even though there are crossing edges in the particular drawing of $G$ shown in [[Graphs and Their Representation#Figure 1.1.|Figure 1.1]]. The first two graphs in [[Graphs and Their Representation#Figure 1.2|Figure 1.2]], on the other hand, are not planar, as proved later.
- Although not all graphs are planar, every graph can be drawn on some surface so that its edges intersect only at their ends. Such a drawing is called an *embedding* of the graph on the surface. Figure 1.21 provides an example of an embedding of a graph on the torus. Embeddings of graphs on surfaces are discussed in Chapter 3 and, more thoroughly, in Chapter 10.
# Incidence and Adjacency Matrices
- Although drawings are a convenient means of specifying graphs, they are clearly not suitable for storing graphs in computers, or for applying mathematical methods to study their properties. For these purposes, we consider two matrices associated with a graph, its incidence matrix and its adjacency matrix.
## Incidence matrix
- Let $G$ be a graph, with vertex set $V$ and edge set $E$. The *incidence matrix* of $G$ is the $n\times m$ matrix $\mathbf{M}_G:=(m_{ve})$, where $m_{ve}$ is the number of times (0, 1, or 2) that vertex $v$ and edge $e$ are incident. Clearly, the incidence matrix is just another way of specifying the graph.
## Adjacency matrix
- The *adjacency matrix* of $G$ is the $n\times n$ matrix $\mathbf{A}_G:=(a_{uv})$, where $a_{uv}$ is the number of edges joining vertices $u$ and $v$, each loop counting as two edges. Incidence and adjacency matrices of the graph $G$ of [[Graphs and Their Representation#Figure 1.1.|Figure 1.1]] are shown in [[Graphs and Their Representation#Figure 1.5|Figure 1.5]].
###### Figure 1.5
![[Pasted image 20250111213533.jpg]]
- Because most graphs have many more edges than verticies, the adjacency matrix of a graph is generally much smaller than its incidence matrix and thus requires less storage space. When dealing with simple graphs, an even more compact representation is possible. For each vertex $v$, the neighbours of $v$ are listed in some order. A list $(N(v):v\in V)$ of these lists is called an *adjacency list* of the graph. Simple graphs are usually stored in computers as adjacency lists.
- When $G$ is a bipartite graph, as there are no edges joining pairs of vertices belonging to the same part of its bipartition, a matrix of smaller size than the adjacency matrix may be used to record the numbers of edges joining pairs of vertics. Suppose that $G[X, Y]$ is a bipartite graph, where $X:=\{x_1, x_2, \cdots, x_r\}$ and $Y:=\{y_1, y_2, \cdots, y_s\}$. We define the *bipartite adjacency matrix* of $G$ to be the $r\times s$ matrix $\mathbf{B}_G=(b_{ij})$, where $b_{ij}$ is the number of edges joining $x_i$ and $y_j$.
### 1.1.21 Eigenvalues of a Graph
- Recall that the eigenvalues of a squar matrix $\mathbf{A}$ are the roots of its characteristic polynomial $\operatorname{det}(\mathbf{A}-x\mathbf{I})$. An eigenvalue of a graph is an eigenvalue of its adjacency matrix. Likewise, the characteristic polynomial of a graph is the characteristic polynomial of its adjacency matrix. Show that:
- a) every eigenvalue of a grap is real,
  - In other words, if you take the adjacency matrix $A_G$ of a graph $G$ and look at its characteristic polynomial $\operatorname{deg}(A_G-x\mathbf{I})$, all the roots of this polynomial are real numbers.
  - The Adjacency Matrix is Real Symmetric
  - A (simple) graph $G$ has an adjacency matrix $A_G=(a_{ij})$, where $$a_{ij}=\begin{cases}1, & \text{if vertex $v_i$ is adjacent to vertex $v_j$}\\
  0, & \text{otherwise}\end{cases}$$
  
  - Of course, it is also a real matrix (all entries are 0 or 1). Hence $A_G$ is a real symmetric matrix.
  - Real Symmetric Matrices Have Real Eigenvalues
  - From basic linear algebra, one learns: A real symmetric ($n\times n$) matrix $M$ can be diagonalized by an orthogonal matrix. Equivalently, all the roots of its characteristic polynomial $\operatorname{det}(M-x\mathbf{I})$ are real numbers. ref. [[Eigenvalue, Eigenvector#symmetric matrices|symmetric matrix]]
  - Therefore, the adjacency matrix $A_G$, being real symmetric, must have real eigenvalues.
  - In symbols, if $\lambda$ is an eigenvalue of $A_G$, then $\lambda\in \mathbb{R}$.
- b) every rational eigenvalue of a graph is integral. 
  - The key point is that the adjacency matrix of a graph is a (0, 1)-matrix with integer entries, so its characteristic polynomial has integer (in fact, integral) coefficients. Once one knows the eigenvalue is rational, the Rational Root Theorem forces it to be an integer.
  - Let $G$ be any (simple) graph on $n$ vertices.
  - Its adjacency matrix $A_G$ is an $n\times n$ real symmetric matrix with integer entries in 0, 1.
  - The characteristic polynomial of $A_G$ is then a **[[Polynomial|Monic polynimial]]** in $\mathbb{Z}[x]$:$$P_G(x)=\operatorname{det}(x\mathbf{I}-A_G)=x^n+a_{n-1}x^{n-1}+\cdots+a_1x+a_0   \text{ where }a_i\in \mathbb{Z}$$
  - **Claim**. If $\lambda$ is a rational eigenvalue of $A_G$, then $\lambda\in\mathbb{Z}$.
  - Because $\lambda$ is an eigenvalue, it is a root of the characteristic polynimial $P_G(x)$. That is, $P_G(\lambda)=0$.
  - 1. $q$ divides the leading coefficient (which is 1), implying $q=\pm1$.
  - 2. $p$ divides the constant term $a_0$. But since $q=1$, that mean $\lambda=\frac pq=p\in\mathbb{Z}$.
  - Hence any rational root $\lambda$ must be an integer.
  - Thus any rational eigenvalue of the (integer) adjacency matrix of a graph must be integral.

# Vertex Degrees
- The *degree* of a vertex $v$ in a graph $G$, denoted by $d_G(v)$, is the number of edges of $G$ incident with $v$, each loop counting as two edges. In particular, if $G$ is a simple graph, $d_G(v)$ is the number of neighbours of $v$ in $G$. A vertex of degree zero is called an *isolated vertex*. 
## minimum and maximum degrees
- We denote by $\delta(G)$ and $\Delta(G)$ the minimum and maximum degrees of the vertices of $G$, 
## Average Degree
- and by $d(G)$ their *average degree*, $\frac 1n \sum_{v\in V}d(v)$. The following theorem established a fundamental identity relating the degrees of the vertices of a graph and the number of its edges.
## Theorem 1.1 
- For *any graph* $G$, 
###### Equation 1.1 Handshaking Lemma
$$\sum_{v\in V}d(v)=2m=2e(G)$$
### Proof 
- Consider the incidence matrix $\mathbf{M}$ of $G$. The sum of the entries in the row corresponding to vertex $v$ is precisely $d(v)$. Therefore $\sum_{v\in V}d(v)$ is just the sum of all the entries in $\mathbf{M}$. But this sum is also $2m$, because each of the $m$ column sums of $\mathbf{M}$ is 2, each edge having two ends.$$\tag*{$\Box$}$$
## Corollary 1.2
- In any graph, the number of vertices of odd degree is even.
### Proof 
- Consider [[Mathematics/Graph Theory/Graphs and Their Representation#Equation 1.1|Equation 1.1]] modulo 2. We have
$$d(v)\equiv\left\{
\begin{array}{2}
1(\text{mod 2})& \text{if }d(v)\text{ is odd}\\
0(\text{mod 2})& \text{if }d(v)\text{ is even.}
\end{array}\right.$$
- Thus, modulo 2, the left-hand side is congruent to the number of vertices of odd degree, and the right-hand side is zero. The number of vertices of odd degree is therefore congruent to zero modulo 2.$$\tag*{$\Box$}$$
### 1.1.16 Degree Sequence
- If $G$ has vertices $v_1,v_2,\cdots,v_n$, the sequence $(d(v_1), d(v_2), \cdots, d(v_n))$ is called a *degee sequence* of $G$. Let $\mathbf{d}:=(d_1, d_2, \cdots, d_n)$ be a nonincreasing sequence of nonnegative integers, that is $d_1\ge d_2\ge \cdots\ge d_n\ge 0$. Show that:
- a) there is a graph with degree sequence $\mathbf{d}$ if and only if $\sum_{i=1}^n d_i$ is even.
  - $\Rightarrow$ Necessity: The sum of degrees in a graph is even 
  - we know from the [[Graphs and Their Representation#Equation 1.1 Handshaking Lemma|Handshaking Lemma]] that $\sum_{i=1}^n d(v_i)=2e(G)$, then $\sum_{i=1}^n d_i$ is an even integer.
  - $\Leftarrow$ Sufficiency: Constructing a graph when $\sum d_i$ is even
  - Conversely, assume we have a sequence $\mathbf{d}=(d_1, \cdots, d_n)$ of nonnegative integers whose sum is even. We show there exists some graph $G$ (with possible loops and parallel edges) realizing $\mathbf{d}$ as its degree sequence.
  - 1. Total half-edges (stubs).
   - Because $\sum_{i=1}^n d_i$ is even, let $m:=\frac 12 \sum_{i=1}^n d_i$
  - 2. Pairing the stubs to form edges.
   - We have a total of $\sum d_i=2m$ half-edges across all vertices. We can simply pair up these $2m$ half-edges is any manner we like:
   - If a pair of half-edges both come from the same vertex $v_i$, we form a loop at $v_i$
   - If they com from different vertices $v_i$ and $v_j$, we form a link(possibly a parallel edge if we use the same pair $(v_i, v_j)$ multiple times).
  - 3. Degrees are as prescribed.
   - Each vertex $v_i$ had wxactly $d_i$ half-edges (stubs) to start with; they all get paired in the above step. Hence the resulting graph has vertex $v_i$ of degree $d_i$.
- b) there is a loopless graph with degree sequence $\mathbf{d}$ if and only if $\sum_{i=1}^n d_i$ is even and $d_1\le \sum_{i=2}^n d_i$. 
  - $(\Rightarrow)$for loopless graph, $\operatorname{max}(d_1)=n-1$, and the graph has no loop, then the another $n-1$ vertices are share the same edges with $v_1$. then $d_1\le \sum_{i=2}^n d_i$.
  - $(\equiv)$  if $\sum_{i=1}^n d_i$ is not even or $d_1> \sum_{i=2}^n d_i$, then  the graph has loop, but $\sum_{i=1}^n d_i$ is always even, then $d_1> \sum_{i=2}^n d_i\Rightarrow$ the graph has loop.
  - for loopless graph, $d_1\le \sum_{i=2}^n d_i$, therefore, if $d_1$ has loop, then $d_1>\sum_{i=2}^n d_i$.
### 1.1.17 Complement of a Graph
- Let $G$ be a simple graph. The complement $\overline{G}$ of $G$ is the simple graph whose vertex set is $V$ and whose edges are the pairs of nonadjacent vertices of $G$.
- a) Express the degree sequence of $\overline{G}$ in terms of the degree sequence of $G$.
  - $\mathbf{d_G}=(d_{v_1}, \cdots, d_{v_n}), \text{ where } v_i\in V$
  - $\mathbf{d}_{\overline{G}}=(d_{u_1}, \cdots, d_{u_i}) \text{where } u_i\in V$
  - because edges of $\overline{G}$ are the pairs of nonadjacent vertices of $G$, and simple graph, 
  - $d_{\overline{G}}(v)=(n-1)-d_G(v)$
  - $\mathbf{d}_{\overline{G}}=(d_{v_n}, \cdots, d_{v_1})$
- b) Show that if $G$ is disconnected, then $\overline{G}$ is connected. Is the converse true?
  - let complete bipartite graph $G$, and  $X(G), Y(G)$ and $v_x\in X, v_y\in Y, (v_x\neq v_y)$
  - $|X|=n_x, |Y|=n_y=n-n_x$
  - For every $x\in X$ and every $y\in Y, (x, y)$ is an edge in $\overline{G}$.
  - Therefore, restricted to the bipartition ($X, Y$), the graph $\overline{G}$ contains all possible edges between $X$ and $Y$. In other words, $\overline{G}$ includes the [[Graphs and Their Representation#Complete Bipartite Graph|complete bipartite graph]] $K_{|X|,|Y|}$ on than same vertex partition. That is enough to see that $\overline{G}$ is connected:
  - A complete bipartite graph $K_{a,b}$ (with $a,b>0$) is connected: any vertex in $X$ can reach any vertex in $Y$ by a single edge, and any two vertices within $X$ can reach each other by going via (at most) one vertex in $Y$, and vice versa.
  - 2. Is the Converse True?
   - if $G$ is connected, then $\overline{G}$ is disconnected.
   - consider a cycle of length 5, $G$ and $\overline{G}$ are all connected.
## [[Regular Graph]]
# Proof Technique: Counting in Two Ways
- in proving [[Graphs and Their Representation#Theorem 1.1|Theorem 1.1]], we used a common proof technique in combinatorics, known as *counting in two ways*. It consists of considering a suitable matrix and computing the sum of its entries in two different ways: firstly as the sum of its row sums, and secondly as the sum of its column sums. Equating these two quantities results in an identity. In the case of [[Graphs and Their Representation#Theorem 1.1|Theorem 1.1]], the matrix we considered was the incidence matrix of $G$. In order to prove the identity of Exercise 1.1.9a, the appropriate matrix to consider is the bipartite adjacency matrix of the bipartite graph $G[X, Y]$. In both these cases, the choice of the appropriate matrix is fairly obvious. However, in some cases, making the right choice requires ingenuity.
- Note that an upper bound on the sum of the column sums of a matrix is clearly also an upper bound on the sum of its row sums (and vice versa). The method of counting in two ways may therefore be adapted to establish inequalities. The proof of the following proposition illustrates this idea.
## Proposition 1.3
- Let $G[X, Y]$ be a bipartite graph without isolated vertices such that $d(x)\ge d(y)$ for all $xy\in E$, where $x\in X$ and $y\in Y$. Then $|X|\le |Y|$, with equality if and only if $d(x)=d(y)$ for all $xy\in E$.
## Proof
- The first assertion follows if we can find a matrix with $|X|$ rows and $|Y|$ columns in which each row sum is one and each column sum is at most one. Such a matrix can be obtained from the bipartite adjacency matrix $\mathbf{B}$ of $G[X, Y]$ by dividing the row corresponding to vertex $x$ by $d(x)$, for each $x\in X$. (This is possible to since $d(x)\neq 0$.) Because the sum of the etries of $\mathbf{B}$ in the row corresponding to $x$ is $d(x)$, all row sums of the resulting matrix $\widetilde{\mathbf{B}}$ are equal to one. On the other hand, the sum of the entries in the column of $\widetilde{B}$ corresponding to vertex $y$ is $\sum 1/d(x)$, the sum being taken over all edges $xy$ incident to $y$, and this sum is at most one because $1/d(x)\le 1/d(y)$ for each edge $xy$, by hypothesis, and because there are $d(y)$ edges incident to $y$. The above arument may be expressed more concisely as follows.
$$|X|=\sum_{x\in X}\sum_{\substack{y\in Y \\xy\in E}}{\frac{1}{d(x)}}=\sum_{\substack{x\in X\\y\in Y}}\sum_{xy\in E}{\frac{1}{d(x)}}\le\sum_{\substack{x\in X\\y\in Y}}\sum_{xy\in E}{\frac{1}{d(y)}}=\sum_{y\in Y}\sum_{\substack{x\in X\\xy\in E}}{\frac{1}{d(y)}}=|Y|$$
- Furthermore, if $|X|=|Y|$, the middle inequality must be an equality, implying that $d(x)=d(y)$ for all $xy\in E$.$$\tag*{$\Box$}$$
- An application of this proof technique to a problem in set theory about geometric configurations is described in Exercise 1.3.15.

# Exercises
### 1.1.1 
- Let $G$ be a [[Graphs and Their Representation#Simple Graph|simple graph]]. Show that $m\le \binom{n}{2}$, and determine when equality holds.
#### solution
- 참조: [[Binomial#Binomial expression|Binomial expression]]
$$\binom{n}{2}=\frac{n!}{2!(n-2)!}=\frac{n(n-1)}{2}$$
- In a simple graph on $n$ vertices, we can choose at most one edge between any two distinct vertices.
- The totoal number of distinct unordered pairs of vertices in an $n$-vertex set is precisely $$\binom{n}{2}=\frac{n(n-1)}{2}$$.
- Because each of these $\binom{n}{2}$ possible pairs can contribute at most one edge in a simple graph, it follows that $$m=e(G)\le \binom{n}{2}$$
###### When does equality hold?
- The equality holds if and only if every pair of distinct vertices is and edge in $G$. In other words, this happens exectly when $G$ in the [[Graphs and Their Representation#complete graph|complete graph]] $K_n$. 
### 1.1.2
- Let $G[X, Y]$ be a simple [[Graphs and Their Representation#Bipartite graph|bipartite graph]], where $|X|=r$ and $|Y|=s$. 
a) Show that $m\le rs$.
- Each vertex in $X$ can be connected to every vertex in $Y$.
- Thus the maximum number of edges occurs when every vertex n $X$ is connected to every vertex in $Y$.
- If $|X|=r$ and $|Y|=s$, the total possible edges are:
$$\text{Maximum edge}=r\cdot s$$
- A simple graph does not allow multiple edges between the same pair of vertices.
- Therefore, the number of edges $m$ in a simple bipartite graph $G$ satisfies:$$m\le r\cdot s$$
- 
b) Deduce that $m\le n^2/4$.
- $n=r+s$, then $n^2=r^2+2rs +s^2$.
- $(r-s)^2=r^2-2rs+s^2\ge 0$
- $2rs\le r^2 +s^2$
- $4rs\le n^2$
- $$m\le rs\le n^2\tag*{$\Box$}$$
c) Describe the simple bipartite graphs $G$ for which equality holds in (b).
- when $r=s$, then $m\le rs=n^2/4$,and complete simple bipartite graph, $m=rs=n^2/4$.

### 1.1.3
- Show that:
a) every [[Graphs and Their Representation#Path|path]] is bipartite,
- Consider a path $P$ with vertices $v_1, v_2, \cdots, v_k$ arranged in a linear sequency.
- The edges in $P$ are:$$(v_1, v_2), (v_2,v_3),\cdots,(v_{k-1},v_k)$$
- Place vertices at odd positions $(v_1,v_3,v_5,\cdots)$ in $X$.
- Place vertices at even positions $(v_2,v_4,v_6,\cdots)$ in $Y$.
b) a cycle is bipartite if and only if it its length is even.
- The edges in $P$ are: $$(v_1, v_2), (v_2,v_3),\cdots,(v_{k-1},v_k), (v_k,v_1)$$
- if $k$ is odd, then $(v_1,v_3,\cdots,v_k)\in X$. and $\{v_2,v_4,\cdots,v_{k-1}\}\in Y$. then last edge $(v_k, v_1)$, two vertices are in $X$.
- if $k$ is even, then $\{v_1,v_3,\cdots,v_{k-1}\}\in X$, and $\{v_2,v_4,\cdots,v_k\}\in Y$, then $v_k\in X$ and $v_1\in Y$ 
### 1.1.4
- Show that, for any graph $G$, $\delta(G)\le d(G)\le \Delta(G)$.
- 참고: [[#Average Degree]]
-  $d(G)=\frac 1n \sum_{v\in V}d(v)$
### 1.1.6
- a) Show that, in any group of two or more people, there are always two who have exactly the same number of friends within the group.
  - Since the graph is simple, no person can have more than $n-1$ friends.
  - **Pigoenhole principle** states that if $m$ items are placed into $n$ containers, and $m>n$, then at least one container must hold more than one item.
  - Assumption: Each vertex in the graph $G$ has a unique degree. This means the degrees of vertices must include all integers from 0 to $n-1$ without repetition.
  - **Contradiction**: If one vertex has degree 0(no friends), then no vertex can have degree $n-1$ because the vertex with degree $n-1$ would need to be connected to all other $n-1$ vertices, which includes the vertex with degree 0, a contradiction.
  - Therefore, not all $n$ degrees from 0 to $n-1$ can be assigned uniquely to the $n$ vertices.
  - By the pigenhole principle, at least two vertices must have the same degree.
- b) Describe a group of five people, any two of whom have exactly one friend in common. Can you find a group of four people with this same property?
  1. We seek a simple graph on five vertics, say $A, B, C, D, E$, in which any two vertices share exactly one common neighbor. A classical example is the "**windmill**"(or "friendship") construction made by **two triangles** sharing a single common vertex.
  - Pick one vertex to be "universal", call it $A$.
  - Form one triangle on vertices $\{A, B, C\}$.
  - Form another triangle on vertices $\{A,D,E\}$.
  - This 5-vertex graph is sometimes called the "windmill graph" with 2 "blades"(two triangles) sharing one vertex. It is a special case of the ore general **Friendship Theorem** in graph theory(which states that if a finite graph has the property that every two vertices share exactly one common neighbor, then that graph must be a "windmill" or triangles all meeting in a single common vertex).
  2. The (Non)Existence of Such a 4-Person Group
   - If we try the complete graph $K_4$, each pair of vertices has two common neighbors.
   - If we try a cycle $C_4$(a square), adjacent vertices share 0 common neighbors, and opposite vertices share 0.
   - If we try a star $K_{1,3}$(one center and three leaves), any leaf-leaf pair shares exactly the center as a neighbor (good), but center-leaf pairs share no other neighbor in common.
   - One can check all other 4-vertex graphs (threre are not many) and see none of them gives "exactly one common friend" for every pair of vertices.
### 1.1.7 $n$-Cube
- The $n$-cube $Q_n(n\ge 1)$ is the graph whose vertex set is the set of all $n$-tuples of 0s and 1s, where two $n$-tuples are adjacent if they differ in precisely one coordinate.
#### a) Draw $Q_1, Q_2, Q_3$, and $Q_4$.
  1. $Q_1$
     - The vertices are all 1-tuples of 0s and 1s, namely: $(0), (1)$
     - These two vertices differ in exactly one coordinate (since there is only one coordinate), so there is one edge connecting them: $Q_1: (0)-(1)$
     - Is is simply a line(or an edge with two endpoints)
2. $Q_2$
   - The vertices are all 2-tupes of 0s and 1s: (0, 0), (0, 1), (1, 0), (1, 1).
   - This set naturaly forms the corners of a square
3. $Q_3$
   - The vertices are all 3-tuples of 0s and 1s. Thee are $2^3=8$ such vertices:$(0,0,0),(0,0,1),(0,1,0),(0,1,1),(1,0,0),(1,0,1),(1,1,0),(1,1,1)$
   - They form the corners of a cube in 3D.
4. $Q_4$
   - The vertices are all 4-tuples of 0s ad 1s. There are $2^4=16$ such vertices:$(0,0,0,0),(0,0,0,1),(0,0,1,0),(0,0,1,1),(0,1,0,0),(0,1,0,1),(0,1,1,0),(0,1,1,1),(1,0,0,0),(1,0,0,1),(1,0,1,0),(1,0,1,1),(1,1,0,0),(1,1,0,1),(1,1,1,0),(1,1,1,1)$
   - Geometrically, $Q_4$ is a **4D hypercube**. A common 2D depiction shows a "cube within a cube", with corresponding vertices connected.
#### b) Determine $v(Q_n)$ andn $e(Q_n)$.
- $v(Q_n)=2^n$
- $e(Q_n):$ $n=1$이면 $e(Q_1)=1$, $e(Q_2)=4=2\times 2, e(Q_3)=12=3\times 4, e(Q_4)=4\times 2^3$ 
- [[Graphs and Their Representation#Equation 1.1|Handshaking Lemma]]
- Because each vertex is an $n$-tuple of bits, changing exactly one coordinate yields exactly $n$ neighbors. So each vertex has degree $n$.
- There are $2^n$ vertices, each of degree $n$. Thus $$\sum_{n\in V}d(v)=2^n\times n=2e(Q_n)$$
- $$e(Q_n)=n2^{n-1}$$
#### c) Show that $Q_n$ is bipartite for all $n\ge 1$.
- $Q_n$ has vertices corresponding to all $n$-bit ninary strings.
- Two vertices (bit-strings) are adjacent if and only if they differ in exactly one coordinate.
- Let $$\begin{align}
X&:=\text{\{all bit-strings of length $n$ with an even numbers of 1s\}},\\
Y&:=\text{\{all bit-strings of length $n$ with an odd numbers of 1s\}}
\end{align}$$
- Take any edge {$u, v$}. By definition of $Q_n$, $u$ and $v$ differ in exactly one bit.
- Changing exactly one bit in a bit-string switches the parity of the number of 1s.
- If $u$ has an even number of 1s, then $v$ has an odd number of 1s, and vice versa.
### 1.1.8 
The *boolean lattice* $BL_n(n\ge 1)$ is the graph whose vertex set is the set of all subsets of $\{1,2,\cdots,n\}$, where two subsets $X$ and $Y$ are adjacent if their symmetric difference has precisely one element.
#### a) Draw $BL_1, BL_2, BL_3$, and $BL_4$
- Edges: two subsets $X$ and $Y$ are adjacent if their symmetric difference $X\Delta Y$ has exactly one element.$$X\Delta Y=(X\backslash Y)\cup (X\backslash X)$$
- Equivalently, $X$ and $Y$ differ by exactly one element from its neighbors, $BL_n$ is **isomorphic to $n$-cube $Q_n$**. One can think of each subset as an $n$-bit string (indicator vector) and "differing by exactly one element" corresponds to "differing in exactly one bit."
1. $BL_1$
   - Subsets of {1}: $\emptyset$ and {1}
   - These two differ by one element, so there is oe edge. This matches $Q_1$.
2. $BL_2$
   - Subsets of {1, 2}: $\emptyset$, {1}, {2}, {1,2}
   - Graphically, it forms a square, just like $Q_2$.
3. $BL_3$
4. ...

#### b) Determine $v(BL_n)$ and $e(BL_n)$.
- refer to [[Graphs and Their Representation#1.1.7 $n$-Cube|1.1.7]]
- 
#### c) Show that $BL_n$ is bipartite for all $n\ge 1$.
- refer to [[Graphs and Their Representation#1.1.7 $n$-Cube|1.1.7]] 
### 1.1.9
- Let $G[X,Y]$ be a bipartite graph.
#### a) Show that $\sum_{v\in X}d(v)=\sum_{v\in Y}d(V)$.
- every edge of $G[X,Y]$, $e(G)$ has one vertex $x_i\in X$ and $y_i\in Y$.
- Represent $G[X,Y]$ by its bipartite adjacency matrix $\mathbf{B}$. Rows correspond to vertices in $X$. Columns correspond to vertices in $Y$.  An entry $b_{xy}$ is the number of edges between $x\in X$ and $y\in Y$.
- For each $x\in X$, summing row $x$ of $\mathbf{B}$ counts exactly the edges incident to $x$. Thus the row sum is $d(x)$. Hence $$\sum_{x\in X}d(x)=\sum_{\text{all row sums}}=\text{sum of all entries of B}$$
- Similarly, for each $y\in Y$, summing column $y$ of $\mathbf{B}$ counts the edges incident to $y$. $$\sum_{y\in Y}d(y)=\sum_{\text{all column sums}}=\text{sum of all entries of B}$$
- $$\sum_{x\in X}d(x)=\sum_{y\in Y}d(y)\tag*{$\Box$}$$
#### b) Deduce that if $G$ is $k$-[[Graphs and Their Representation#Regular graph|regular]], with $k\ge1$, then $|X|=|Y|$.
- $$\sum_{x\in X}d(x)=k|X|, \sum_{y\in Y}d(y)=k|Y|$$
- $$|X|=|Y|\tag*{$\Box$}$$
### 1.1.10 $k$-Partite Graph
- A $k$-partite graph is one whose vertex set can be partitioned into $k$ subsets, or parts, in such a way that no edge has both ends in the same part. (Equivalently, one may think of the vertices as being colourable by $k$ colours so that no edge joins two vertices of the same colour.) Let $G$ be a simple $k$-partite graph with parts of sizes $a_1, a_2, \cdots, a_k$. Show that $m\le \frac 12 \sum_{i=1}^k a_i(n-a_i)$.($n\equiv v(G)$)
#### soluion
- $\operatorname{max}\left\{\sum_{v\in a_i}d(v)\right\}=a_i(n-a_i)$
- $$m=e(G)\le \frac 12 \sum_{i=i}^k \sum_{v\in a_i}d(v)=\frac 12 \sum_{i=1}^k a_i(n-a_i)\tag*{$\Box$}$$
### 1.1.11 Turan Graph
- A $k$-partite graph is complete if any two vertices in different parts are adacent. A simple complete $k$-partite graph on $n$ vertices whose parts are of equal or almost equal sizes (that is, $\lfloor n/k\rfloor$ or $\lceil n/k\rceil$) is called a Turan graph and denoted $T_{k, n}$
- a) Show that $T_{k, n}$ has more edges than any other simple complete $k$-paitite graph on $n$ vertices.
  - all part differ in size at most 1.
  - $$e(G) = \frac 12 \sum_{i=1}^k a_i(n-a_i)$$
  - $$\sum_{i=1}^k a_i(n-a_i)=n\sum_{i=1}^k a_i -\sum_{i=1}^k a_i^2=n^2-\sum_{i=1}^ka_i^2$$
  - $$e(G)=\frac 12 \left(n^2-\sum_{i=1}^ka_i^2\right)$$
  - Maximizing $e(G)$ is therefore equivalent to minimizing $\sum a_i^2$ 
  - It is a well-known "convexity" (or "equalizing") fact that for a fixed sum $n$, the sum of squares $\sum a_i^2$ is minimized precisely when the $a_i$ are as equal as possible-differing from one another at most 1.
- b) Determine $e(T_{k, n})$
  - if $a_i=\frac nk$, $e(T_{k, n})=\frac 12 \left(n^2-\frac{n^2}{k}\right)=\frac{(k-1)n^2}{2k}$
  - if $a_i \neq \frac nk ??$
  - Case 2: $n$ is not divisible by $k(n=kq+r, \text{ where }0<r<k)$
  - $$e(G)=\frac 12 \left(n^2-\sum_{i=1}^ka_i^2\right)$$
### 1.1.12
- a) Show that if $G$ is simple and $m>\binom{n-1}{2}$, then $G$ is connected.([[Graphs and Their Representation#Connected Graph|connected graph]], [[Binomial#Binomial expression|binomal]])
  - $\binom{n-1}{2}=\frac{(n-1)!}{2!(n-3)}=\frac{(n-2)(n-1)}{2}$, which is the maximum number of edges possible in a graph with $n-1$ vertices(ref. [[Graphs and Their Representation#1.1.1|Exercise 1.1.1]]).
  - Contrapositive staement: If $G$ is disconnected, then $m\le \binom{n-1}{2}$.
  - If $G$ is disconnected, its vertex set can be partitioned into two non-empty, disjoint subsets $X$ and $Y$ such that there are no edges between $X$ and $Y$.
  - Let $|X|=k$ and $|Y|=n-k$, where $1\le k\le n-1$
  - Within $X$: The maximum number of edges is $\binom{k}{n}$
  - Within $Y$: The maximum number of edges if $\binom{n-k}{2}$
  - Therefore, the total maximum number of edges in $G$ is: $m_{max}=\binom{k}{2}+\binom{n-k}{2}$
  - $$\begin{align}
  \binom{k}{2}+\binom{n-k}{2}&=\frac{k!}{2(k-2)!}+\frac{(n-k)!}{2(n-k-2)!}\\
  &=\frac{k(k-1)}{2}+\frac{(n-k-1)(n-k)}{2}
  \end{align}$$
  - The expression $\binom{k}{n}+\binom{n-k}{2}$ is maximized when the difference between $k$ and $n-k$ is as large as possible. This occurs when one partition has as many vertices as possible while the other has as few as possible. then $k=1$ and $n-k=n-1$
  - then $$m_{max}=\binom{n-1}{2}\tag*{$\Box$}$$
  - b) For $n>1$, find a disconnected simple graph $G$ with $m=\binom{n-1}{2}$
   - $m_{\text{max}=\binom{n-1}{2}}$
### 1.1.13
- a) Show that if $G$ is simple and $\delta>\frac 12 (n-1)$, then $G$ is connected.(ref. [[#minimum and maximum degrees|minimum degree]])
- In a disconnected graph, consider the smallest component with $k$ vertices.
- $$\delta>\frac 12(n-1)\le\frac 12(k+(n-k+1))$$
- However, within the smallest component:$d(v)=\delta\le k-1$
- Therefore:$$k-1\ge\delta>\frac 12(n-1)$$
- $$\begin{align}
k>\frac 12(n-1)+1\\
k\ge\Bigg\lceil{\frac n2}\Bigg\rceil+1
\end{align}$$
- But:$$k\le n-1$$
- This leads to a contradiction, as $\big\lceil\frac n2\big\rceil+1>n-1$ for $n\ge3$
- Therefore, $G$ must be connected.
- b) For $n$ even, find a disconnected $\frac 12(n-2)$-regular simple graph.
- $$m=\frac{n(n-2)}{2}\times \frac 12 = \frac{n(n-2)}{4}$$
- if disconnected, [[#1.1.12]]$$m\le\frac{(n-1)(n-2)}{2}$$
- then, $$\frac{n(n-2)}{4}\le\frac{(n-1)(n-2)}{2}$$
- therefore, $$\begin{align}n^2-2n&\le 2n^2-6n+4\\
n^2-4n+4&\ge0\\
(n-2)^2&\ge0\\
n&\ge2
\end{align}$$
- partition: two subsets of $\frac n2$ vertices each.
- Graphs: Two complete graph $K_{\frac n2}$ graphs(each is a single edge).
- then, each vertex within $K_{\frac n2}$ has degree $\frac n2 -1=\frac{n-2}{2}$
### 1.1.14
- For a simple graph $G$, show that the diagonal entries of boh $\mathbf{A}^2$ and $\mathbf{MM}^t$(where $\mathbf{M}^t$ denotes the transpose of $\mathbf{M}$) are the degrees of the vertices of $G$. [[#Incidence and Adjacency Matrices]]
- The *adjacency matrix* of $G$ is the $n\times n$ matrix $\mathbf{A}_G:=(a_{uv})$, where $a_{uv}$ is the number of edges joining vertices $u$ and $v$
- for a simple graph, $a_{uu}=0, (u\in V(G))$ because it has no loop.
- $$(\mathbf{A}^2)_{11}=a_{11}a_{11}+a_{12}a_{21}+a_{13}a_{31}+\cdots+a_{1n}a_{n1}$$
- Because $a_{uv}\in \{0,1\}$, $$\left(\mathbf{A^2}\right)_{uu}=\sum_{v\in V(G)}a_{uv}a_{vu}=\sum_{v\in V(G)}a_{uv}=deg(u)$$
- in simple graph, $a_{ij}$ has only 0 or 1. and $a_{ij}=a_{ji}$. then 
- $$(\mathbf{A}^2)_{11}=\text{degree of vertex 1}$$
- 참조: [[#Incidence matrix]]
- $\mathbf{M}_G:=(m_{ve}), \mathbf{M}_G^t:=(m_{ev})$
- $(m_{ve})$ has only 0 and 1, because it has no loop(simple graph).
- since $m_{ue}\in \{0,1\}$ and each edge connects exactly two vertices, 
- $$\sum_{e\in E(G)}m_{ue}^2=\sum_{e\in E(G)}n_{ue}=\operatorname{deg}(u)$$
- $$\mathbf{MM^t}_{ii}=\text{degree of each vertex $v_i$}$$
### 1.1.15
- Show that the rank over $GF(2)$ of the incidence matrix of a graph $G$ is at most $n-1$, with equality if and only if $G$ is connected. 참조: [[Field Theory#GF(2)|GF(2)]], [[Rank]]
- Given a (finite) graph $G$ on $n$ vertices, let $\mathbf{M}_G$ be its indicence matrix over $GF(2)$. We wish to show:
- 1. $\operatorname{rank}(\mathbf{M}_G)\le n-1$
- 2. Moreover, $\operatorname{rank}(\mathbf{M}_G)=n-1$ iff $G$ is connected. (For loops: a loop at vertex $v$ contributes "2 copies of $v$", but since $2=0$ in $GF(2)$, effectively it contributes a single 1 if we define it carefully. However, loops do not affect the standard argument about connectivity, so we often treat the simple-graph case first.)
#### 1. A Global Linear Dependence Among Rows
- Over GF(2), summing all row vectors of $\mathbf{M}_G$ gives the zero vector. Concretely, 
  - Each edge $e$ appears exactly twice among the incidence entries (once for each endpoint),
  - In GF(2), "2" is the same as "0"
- Hence when you add all row vectors in GF(2), each column sum becomes zero:
- $$\underbrace{1+1}_{=0 in GF(2)}​​=0\implies v\in V(G)\sum_{v\in V(G)}​(\operatorname{row}_v​)=0.$$
- Because we found a nontrivial linear combination that gives $\mathbf{0}$, the rows cannot all be linearly independent. ref. [[Linear indepence#Definition|linearly dependency]]
- This shows there is at least one nontrivial linear dependence among the row. thus,$$\operatorname{rank}(\mathbf{M}_G)\le n-1$$
- 1. If $G$ is connected, $\operatorname{rank}(M_G)=n-1$
- The idea is that when $G$ is connected, there is exactly one linear dependence among the row vectors-namely, the one we already found (the sum of all rows equals zero). To make this rigorous, one can use a spanning tree $T$ of $G$.
- Since $G$ is connected, pick any spanning tree $T$ with $n-1$ edges. ref. [[Tree#Spanning Tree]]
- Look at the columns of $\mathbf{M}_G$ that correspond specifically to the edges of $T$.
#### Restrict the Incidence Matrix to Tree Edges
- It can be shown (by induction, or by explicit construction) that the rows of $\mathbf{M}_G$ restricted to those $n-1$ tree-edge columns are linearly independent over $GF(2)$ up to exactly one global sum. 
- In the full icidence matrix $\mathbf{M}_G$, some columns correspond to the edges in $T$, and the rest correspond to edges not in $T$.
- Focus just on the $n-1$ columns corresponding to the tree edges $e_1,\cdots,e_{n-1}$. Call this submatrix $M_T$. It is an $n\times (n-1)$ matrix over $GF(2)$.
- Equivalently, you can argue there is no smaller nontrivial dependency than the one forcing the sum of all rows to be zero.
- Here is one intuitive way(among several) to see why the rows of $M_T$ are as independent as possible in $GF(2)$.
- 1. Root the Tree. Pick any vertex as a "root", say $v_1$. A spanning tree with a chosen root gives you a parent-child structure: Every vertex $v_i\neq v_1$ has a unique parent in the tree, denoted $p(i)$, such that $(v_i, p(i))$ is exactly one of the tree edges.
- 2. Incidence Patterns in $M_T$.
  - For each edge $(v_i, p(i))$, exactly those two vertices' rows(row $\operatorname{row}(v_i)$ and row $\operatorname{row}(p(i))$) have a 1 in the column corresponding to this edge.
  - Over $GF(2)$, that means if you look at row $\operatorname{row}(v_i)$ and row $\operatorname{row}(p(i))$, they differ in exactly that column (and possibly coincide in others).
- 3. No Smaller Dependence.
  - Suppose there is a nonempty subset of rows that sum to zero. In $GF(2)$, summing a set of row vectors to zero means that, for each column, an even number of those rows have a 1 in that column.
  - But if the tree is acyclic and connected, you can show via induction or a direct parent-child argument that the only way this can happen for all the tree-edge columns at once is if you include all the vertices (all rows). In that scenario, precisely every tree edge is counted twice (once by each endpoint), and so each column sum is zero.
  - Hence, the unique dependence among the rows is the global sum of all $n$ rows.
- Because that one global sum is a dependence, the dimension of the row space of $M_T$ cannot be $n$; it is at most $n-1$. But you cannot find any fewer rows that sum to zero, so the rank of $M_T$ is exactly $n-1$.
#### Why This Implies $\operatorname{rank}(M_G)=n-1$
- Recall we only used the edges in $T$ to construct $M_T$. Adding more edges (the columns of edges not in $T$) to form the full incidence matrix $M_G$ might only increase the possible row-dependence relations, so $\operatorname{rank}(M_G)$ can never exceed $\operatorname{rank}(M_T)$. We already know $\operatorname{rank}(M_G)\le n-1$. Therefore, from the chain:$$\operatorname{rank}(M_T)\le \operatorname{rank}(M_G)\le n-1$$
- but, $\operatorname{rank}(M_G)=n-1$. Thus, by "pinching" the inequality, we get $$\operatorname{rank}(M_G)=n-1\text{   provided $G$ is connected}$$



### 1.1.20 
- Let $S$ be a set of $n$ points in the plane, the distance between any two of which is at least one. Show that there are at most $3n$ pairs of points of $S$ at distance exactly one.
  - Our goal is to show $e(G)\le3n$
  - By definition, each vertex $v\in S$ has $\operatorname{deg}(v)$ neighbors exactly 1 unit away from $v$. We want to sow that $\operatorname{deg}(v)\le6$ for each $v$. If this is true, then the sum of all degrees in the graph satisfies $$2e(G)=\sum_{v\in S} \operatorname{deg}(v)\le 6n\implies e(G)\le 3n$$
  - Why each point has at most 6 unit neighbors.
  - Suppose $v$ is a point in $S$. Around $v$, draw a circle of radius 1. Its neighbots in $G$ must lie on that circle(distance exactly 1 from $v$), and they must at least distance 1 from each other (since all points in $S$ are pairwise at least 1 apart).
