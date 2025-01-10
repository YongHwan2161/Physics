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
   Most of the definitions and concepts in graph theory are suggested by this graphical representation. The ends of an edge are said to be *incident*