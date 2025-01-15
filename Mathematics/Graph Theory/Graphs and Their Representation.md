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
## Simple Graph
- A graph is *simple* if it has no loops or parallel edges. The graph $H$ in [[Graphs and Their Representation#Figure 1.1.|fig. 1.1]] is simple, whereas the graph $G$ in [[Graphs and Their Representation#Figure 1.1.|fig. 1.1]] is not. Much of graph theory is concerned with the study of simple graphs.
- The maximum number of edges in a simple graph on $n$ vertices occurs when every pair of distinct vertices is joined by exactly one edge.
- Such a graph, in which every pair of distinct vertices is adjacent, is called the [[Graphs and Their Representation#complete graph|complete graph]] on $n$ vertices, denoted $\mathbf{K}_n$.
- A set $V$, together with a set $E$ of two-element subsets of V, defines a simple graph $(V, E)$, where the ends of an edge $uv$ are precisely the vertices $u$ and $v$. Indeed, in any simple graph we may dispense with the incidence function $\psi$ by renaming each edge as the unordered pair of its ends. In a diagram of such a graph, the labels of the edges may then be omitted.
# Special Families of Graphs
## complete graph
- Certain types of graphs play prominent roles in graph theory. A ==*complete== graph* is a simple graph in which any two vertices are adjacent, an *empty graph* one in which no two vertices are adjacent (that is, one whose edge set is empty). 
## Bipartite graph
- A graph is *bipartite* if its vertex set can be partitioned into two subsets $X$ and $Y$ so that every edge has one end in $X$ and one end in $Y$; such a partition $(X, Y)$ is call ==*bipartition*== of the graph, and $X$ and $Y$ its *parts*. We denote a bipartite graph $G$ with bipartition $(X, Y)$ by $G[X, Y]$.
- No edge connects two vertices within $X$.
- No edge connects two vertices within $Y$.
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
- A graph is *connected* if, for every partition of its vertex set into two nonempty sets $X$ and $Y$, there is an edge with one end in $X$ and one end in $Y$; otherwise the graph is *disconnected*. In other words, a graph is disconnected if its vertex set can be partitioned into two nonempty subsets $X$ and $Y$ so that no edge has one end in $X$ and one end in $Y$. (It is instructive to compare this definition with that of a bipartite graph.) Examples of connected and disconnected graphs are displayed in [[Graphs and Their Representation#Figure 1.4|Figure 1.4]].
###### Figure 1.4
![[Pasted image 20250111210724.jpg]]
- As observed earlier, examples of graphs abound in the real world. Graphs also arise naturally in the study of other mathematical structures such as polyhedra, lattices, and groups. There graphs are generally defined by means of an adjacency rule, prescribing which unordered pairs of vertices are edges and which are not. A number of such examples are given in the exercises at the end of this section and in Section 1.3.
- For the sake of clarity, we observe certain conventions in representing graphs by diagrams: we do not allow an edge to intersect itself, nor let an edge pass through a vertex that is not an end of the edge; clearly, this is always possible. However, two edges may intersect at a point that does not correspond to a vertex, as in the drawings of the first two graphs in [[Graphs and Their Representation#Figure 1.2|Figure 1.2]]. A  graph which can be drawn in the plane in such a way that edges meet only at points corresponding to their common ends is called a *planar graph*, and  such a drawing is called a *planar embedding* of the graph. For instance, the graphs $G$ and $H$ of Examples 1 and 2 are both planar, even though there are crossing edges in the particular drawing of $G$ shown in [[Graphs and Their Representation#Figure 1.1.|Figure 1.1]]. The first two graphs in [[Graphs and Their Representation#Figure 1.2|Figure 1.2]], on the other hand, are not planar, as proved later.
- Although not all graphs are planar, every graph can be drawn on some surface so that its edges intersect only at their ends. Such a drawing is called an *embedding* of the graph on the surface. Figure 1.21 provides an example of an embedding of a graph on the torus. Embeddings of graphs on surfaces are discussed in Chapter 3 and, more thoroughly, in Chapter 10.
# Incidence and Adjacency Matrices
- Although drawings are a convenient means of specifying graphs, they are clearly not suitable for storing graphs in computers, or for applying mathematical methods to study their properties. For these purposes, we consider two matrices associated with a graph, its incidence matrix and its adjacency matrix.
- Let $G$ be a graph, with vertex set $V$ and edge set $E$. The *incidence matrix* of $G$ is the $n\times m$ matrix $\mathbf{M}_G:=(m_{ve})$, where $m_{ve}$ is the number of times (0, 1, or 2) that vertex $v$ and edge $e$ are incident. Clearly, the incidence matrix is just another way of specifying the graph.
- The *adjacency matrix* of $G$ is the $n\times n$ matrix $\mathbf{A}_G:=(a_{uv})$, where $a_{uv}$ is the number of edges joining vertices $u$ and $v$, each loop counting as two edges. Incidence and adjacency matrices of the graph $G$ of [[Graphs and Their Representation#Figure 1.1.|Figure 1.1]] are shown in [[Graphs and Their Representation#Figure 1.5|Figure 1.5]].
###### Figure 1.5
![[Pasted image 20250111213533.jpg]]
- Because most graphs have many more edges than verties, the adjacency matrix of a graph is generally much smaller than its incidence matrix and thus requires less storage space. When dealing with simple graphs, an even more compact representation is possible. For each vertex $v$, the neighbours of $v$ are listed in some order. A list $(N(v):v\in V)$ of these lists is called an *adjacency list* of the graph. Simple graphs are usually stored in computers as adjacency lists.
- When $G$ is a bipartite graph, as there are no edges joining pairs of vertices belonging to the same part of its bipartition, a matrix of smaller size than the adjacency matrix may be used to record the numbers of edges joining pairs of vertics. Suppose that $G[X, Y]$ is a bipartite graph, where $X:=\{x_1, x_2, \cdots, x_r\}$ and $Y:=\{y_1, y_2, \cdots, y_s\}$. We define the *bipartite adjacency matrix* of $G$ to be the $r\times s$ matrix $\mathbf{B}_G=(b_{ij})$, where $b_{ij}$ is the number of edges joining $x_i$ and $y_j$.
# Vertex Degrees
- The *degree* of a vertex $v$ in a graph $G$, denoted by $d_G(v)$, is the number of edges of $G$ incident with $v$, each loop counting as two edges. In particular, if $G$ is a simple graph, $d_G(v)$ is the number of neighbours of $v$ in $G$. A vertex of degree zero is called an *isolated vertex*. We denote by $\delta(G)$ and $\Delta(G)$ the minimum and maximum degrees of the vertices of $G$, 
## Average Degree
- and by $d(G)$ their *average degree*, $\frac 1n \sum_{v\in V}d(v)$. The following theorem established a fundamental identity relating the degrees of the vertices of a graph and the number of its edges.
## Theorem 1.1 
- For *any graph* $G$, 
###### Equation 1.1
$$\sum_{v\in V}d(v)=2m$$
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
## Regular graph
- A graph $G$ is *k-regular* if $d(v)=k$ for all $v\in V$; a *regular graph* is one that is $k$-regular for some $k$. For instance, the [[Graphs and Their Representation#complete graph|complete graph]] on $n$ vertices is $(n-1)$-regular, and the complete bipartite graph with $k$ vertics in each part is $k$-regular. For $k=0, 1$ and 2, $k$-regular graphs have very simple structures and are easily characterized (Exercise 1.1.5). By contrast, 3-regular graphs can be remarkably complex. These graphs, also referred to as $\text{cubic}$ graphs, play a prominent role in graph theory. We present a number of interesting examples of such graphs in the section.
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

## Exercises
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
### 1.1.5
- For $k=0, 1, 2$, characterize the $k$-regular graphs.
- 