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
   ### Figure 1.1.
   ![[Pasted image 20250110204648.jpg]]
- An edge with identical ends is called a *loop*, and an edge with distinct ends a *link*. Two or more links with the same pair of ends are said to be *parallel edges*.In the graph $G$ of [[Graphs and Their Representation#Figure 1.1.|Figure 1.1]], the edge $b$ is a *loop*, and all other edges are links; the edges $d$ and $f$ are parallel edges.
- Throughout the book, the letter $G$ denotes a graph. Moreover, when there is no scope for ambiguity, we omit the letter $G$ from graph-theoretic symbols and write, for example, $V$ and $E$ instead of $V(G)$ and $E(G)$. In such instances, we denote the numbers of vertices and edges of $G$ by $n$ and $m$, respectively.
- A graph is *finite* if both its vertex and edge set are finite. In this book, we mainly study finite graphs, and the term 'graph' always means 'finite graph'. The graph with no vertices (and hence no edges) is the *null graph*. Any graph with just one vertex is referred to as *trivial*. All other graphs are *nontrivial*. We admit the null graph solely for mathematical convenience. Thus, unless otherwise specified, all graphs under discussion should be taken to be nonnull.
- A graph is *simple* if it has no loops or parallel edges. The graph $H$ in [[Graphs and Their Representation#Figure 1.1.|fig. 1.1]] is simple, whereas the graph $G$ in [[Graphs and Their Representation#Figure 1.1.|fig. 1.1]] is not. Much of graph theory is concerned with the study of simple graphs.
- A set $V$, together with a set $E$ of two-element subsets of V, defines a simple graph $(V, E)$, where the ends of an edge $uv$ are precisely the vertices $u$ and $v$. Indeed, in any simple graph we may dispense with the incidence function $\psi$ by renaming each edge as the unordered pair of its ends. In a diagram of such a graph, the labels of the edges may then be omitted.
# Special Families of Graphs
- Certain types of graphs play prominent roles in graph theory. A ==*complete== graph* is a simple graph in which any two vertices are adjacent, an *empty graph* one in which no two vertices are adjacent (that is, one whose edge set is empty). A graph is *bipartite* if its vertex set can be partitioned into two subsets $X$ and $Y$ so that every edge has one end in $X$ and one end in $Y$; such a partition $(X, Y)$ is call ==*bipartition*== of the graph, and $X$ and $Y$ its *parts*. We denote a bipartite graph $G$ with bipartition $(X, Y)$ by $G[X, Y]$. If $G[X, Y]$ is simple and every vertex in $X$ is joined to every vertex in $Y$, then $G$ is called a *complete bipartite graph*. A *star* is a complete bipartite graph $G[X, Y]$ with $|X|=1$ or $|Y|=1$. [[Graphs and Their Representation#Figure 1.2|figure 1.2]] shows diagrams of a complete graph, a complete bipartite graph, and a star.
###### Figure 1.2
![[Pasted image 20250110211649.jpg]]
- A *==path*== is a simple graph whose vertices can be arranged in a linear sequence in such a way that two vertices are adjacent if they are consecutive in the sequence, and are nonadjacent otherwise. Likewise, a ==*cycle*== on three or more vertices is a simple graph whose vertices can be arranged in a cyclic sequence in such a way that two vertices are adjacent if they are consecutive in the sequence, and are nonadjacent otherwise; a cycle on one vertex consists of a single vertex with a loop, and a cycle on two vertices consists of two vertices joined by a pair of parallel edges. The ==*length*== of a path or a cycle is the number of its edges. A path or cycle of length $k$ is called a *k-path* or *k-cycle*, respectively; the path or cycle is *odd* or *even* according to the parity of $k$. A 3-cycle is often called a *triangle*, a 4-cycle a *quadrilateral*, a 5-cycle a *pentagon*, a 6-cycle a *hexagon*, and so on. [[Graphs and Their Representation#Figure 1.3|Figure 1.3]] epicts a 3-path and a 5-cycle.
###### Figure 1.3
![[Pasted image 20250110213155.jpg]]
- A graph is *connected* if, for every partition of its vertex set into two nonempty sets $X$ and $Y$, there is an edge with one end in $X$ and one end in $Y$; otherwise the graph is *disconnected*. In other words, a graph is disconnected if its vertex set can be partitioned into two nonempty subsets $X$ and $Y$ so that no edge has one end in $X$ and one end in $Y$. (It is instructive to compare this definition with that of a bipartite graph.) Examples of connected and disconnected graphs are displayed in 
###### Figure 1.4
