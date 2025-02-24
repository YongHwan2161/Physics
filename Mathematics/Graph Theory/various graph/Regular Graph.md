## Regular graph
- A graph $G$ is *k-regular* if $d(v)=k$ for all $v\in V$; a *regular graph* is one that is $k$-regular for some $k$. For instance, the [[Graphs and Their Representation#complete graph|complete graph]] on $n$ vertices is $(n-1)$-regular, and the complete bipartite graph with $k$ vertics in each part is $k$-regular. For $k=0, 1$ and 2, $k$-regular graphs have very simple structures and are easily characterized (Exercise 1.1.5). By contrast, 3-regular graphs can be remarkably complex. These graphs, also referred to as $\text{cubic}$ graphs, play a prominent role in graph theory. We present a number of interesting examples of such graphs in the section.
- For $k=0, 1, 2$, characterize the $k$-regular graphs.
- Case 1: $k=0$
  - A 0-regulat graph is one in which every vertex has degree 0.
  - This implies there are no edges in the graph.
  - The graph consists of isolated vertices, with no connections between them.
  - For $|V|=n$, the graph is simply a set of $n$ isolated points.
- Case 2: $k=1$
  - A 1-regulat graph is one in which every vertex has degree 1.
  - This means each vertex is connected to exactly one other vertex.
  - A 1-regular graph is a collection of disjoint edges.
  - For $|V|=n$, the graph consists of $n/2$ edges if $n$ is even.(if $n$ is odd, there is no 1-regular graph ?)
- Case 3: $k=3$
  - A 2-regular graph is one in which every vertex has degree 2.
  - This implies each vertex is part of exactly two edges.
  - A 2-regular graph is a collection of disjoint cycles.
  - The graph may consist of one cycle (if it is connected) or multiple cycles (if disconnected).
### 1.1.22
- a) Let $G$ be a $k$-regular graph. Show that:
  - i) $\mathbf{MM}^t=\mathbf{A}+k\mathbf{I}$, where $\mathbf{I}$ is the $n\times n$ identity matrix.
    - The $(u, v)$ entry of the product $\mathbf{MM}^T$ is given by $$(\mathbf{MM}^T)_{uv}=\sum_{e\in E}m_{ue}m_{ve}$$
    - We now consider two cases:
    - 1. When $u=v$: in this case, $$(\mathbf{MM}^T)_{uu}=\sum_{e\in E}m_{ue}^2=\sum_{e\in E}m_{ue}=k$$
    - 2. when $u\neq v$: $$(\mathbf{MM}^T)_{uv}=\sum_{e\in E}m_{ue}m_{ve}=\begin{cases}
   1, & \text{if } u \text{ and } v \text{ are adjacent},\\
   0, & \text{if } u \text{ and } v \text{ are not adjacent}.
   \end{cases}$$
   - For $u=v:(\mathbf{MM^T})_{uu}=k$
   - Fo $u\neq v:(\mathbf{MM}^T)_{uv}$ (the $(u,v)$ entry of $\mathbf{A}$)
   - Thus, we can write $$\mathbf{MM}^T=\mathbf{A}+k\mathbf{I}\tag*{$\Box$}$$
   - ii) $k$ is an eigenvalue of $G$, with corresponding eigenvector $\mathbf{1}$, the $n$-vector in which each entry is 1.
     - We wish to show that $G$ is a $k$-regular graph, then $A_G\mathbf{1}=k\mathbf{1}$
     - The adjacency matrix $A_G=(a_{ij})$ is defined by $$
   a_{ij} = \begin{cases}
   1, & \text{if vertices } v_i \text{ and } v_j \text{ are adjacent},\\
   0, & \text{otherwise}.
   \end{cases}$$
     - The product $(A_G, \mathbf{1})$ (the $i$th entry of $A_G, \mathbf{1}$) is given by $$(A_G, \mathbf{1})_i=\sum_{j=1}^n a_{ij}\cdot1=\sum_{j=1}^n a_{ij}$$
     - Since $G$ is $k$-regular, every vertex $v_i$ has degree $k$. Thus, for every $i$, $\sum_{j=1}^n a_{ij}=k$
     - $(A_G\mathbf{1}_i)=k$  for all $i=1,2,\cdots,n$
     - 
  - b) Let $G$ be a complete graph of order $n$. Denote by $\mathbf{J}$ the $n\times n$ matrix all of whose entries are 1. Show that:
    - i) $\mathbf{A}=\mathbf{J}-\mathbf{I}$
      - no loop, and $k=n-1$
    - ii) $\operatorname{det}(\mathbf{J}-(1+\lambda)\mathbf{I})=(1+\lambda-n)(1+\lambda)^{n-1}$
    - We wish to show that if $$
\mathbf{J} = \begin{pmatrix} 1 & 1 & \cdots & 1\\ 1 & 1 & \cdots & 1\\ \vdots & \vdots & \ddots & \vdots \\ 1 & 1 & \cdots & 1 \end{pmatrix}
$$
    - $\mathbf{X}:=\mathbf{J}-(1+\lambda)\mathbf{I}$,
    - $\operatorname{det}\left(\mathbf{J}-(1+\lambda)\mathbf{I}\right)=(1+\lambda-n)(1+\lambda)^{n-1}$
    - In many texts the characteristic polynomial of $\mathbf{J}$ is written as
    - $\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)=\big((1+\lambda)-n\big)(1+\lambda)^{n-1}$
    - $\operatorname{det}\big(\mathbf{J}-(1+\lambda)\mathbf{I}\big)=(-1)^n\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)$,
    - $\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)=(1+\lambda-n)(1+\lambda)^{n-1}$ is the standard one.
    - For the sake of this solution, we now explain the standard argument leading to $\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)=(1+\lambda-n)(1+\lambda)^{n-1}$
    - Step 1. Eigenvalues of J
      - The matrix $\mathbf{J}$ is well known to have rank 1. It's eigenvalues are:
      - $n$ (with multiplicity 1), with corresponding eigenvector $\mathbf{1}=(1,1,\cdots,1)^T$
      - 0 (with multiplicity $n-1$); any vector orthogonal to $\mathbf{1}$ is an eigenvector with eigenvalue 0.
    - Step 2. Eigenvalues of $(1+\lambda)\mathbf{I}-\mathbf{J}$
      - Let, $x=1+\lambda$. then $x\mathbf{I}-\mathbf{J}$
      - $(x\mathbf{I}-\mathbf{J})\mathbf{v}=(x-\mu)\mathbf{v}$
      - $x-n=(1+\lambda)-n$(with multiplicity 1)
      - $x-0=x=1+\lambda$(with multiplicity $n-1$)
    - Step 3. Determinant from the Eigenvalues
      - The determinant of a matrix equals the product of its eigenvalues (counting multiplicity). Hence,
      - $\operatorname{det}\big(x\mathbf{I}-\mathbf{J}\big)=\big((1+\lambda)-n\big)(1+\lambda)^{n-1}$
      - $\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)=(1+\lambda-n)(1+\lambda)^{n-1}$
    - Step 4. Relating $\operatorname{det}\big(\mathbf{J}-(1+\lambda)\mathbf{I}\big)$ to $\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)$
      - Since $\mathbf{J}-(1+\lambda)\mathbf{I}=-\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)$
      - $\operatorname{det}\big(\mathbf{J}-(1+\lambda)\mathbf{I}\big)=(-1)^n\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)$
      - $\operatorname{det}\big(\mathbf{J}-(1+\lambda)\mathbf{I}\big)=(-1)^n(1+\lambda-n)(1+\lambda)^{n-1}$
      - $\operatorname{det}\big((1+\lambda)\mathbf{I}-\mathbf{J}\big)=(1+\lambda-n)(1+\lambda)^{n-1}$
      - If the problem statement asks for $\operatorname{det}\big(\mathbf{J}-(1+\lambda)\mathbf{I}\big)$ exactly as written, then the answer is $$\bbox[border:1px solid white]{\operatorname{det}\big(\mathbf{J}-(1+\lambda)\mathbf{I}\big)=(-1)^n(1+\lambda-n)(1+\lambda)^{n-1}}$$
- c) Derive from (b) the eigenvalues of a complete graph and their multiplicities, and determine the corresonding eigenspaces.
  - We start with the fact that if $G$ is the complete graph $K_n$, then its adjacency matrix is $A=J-I$, where 1) $J$ is the $n\times n$ matrix in which every entry is 1, 2) $I$ is the $n\times n$ identity matrix.
  - Recall from part (b) that $$\operatorname{det}\big((1+\lambda)I-J\big)=(1+\lambda-n)(1+\lambda)^{n-1}$$
  - $n$ with multiplicity 1 (with eigenvector $\mathbf{1}=(1,1,\cdots,1)^T$),
  - $0$ with multiplicity $n-1$.
  - Now observe that $A=J-I\implies A+I=J$
  - $(A+I)x=Jx$,
  - $(\mu+1)x=Jx$
  - 1. Case 1: $\mu +1=n$. Then $\mu = n-1$
  - 2. Case 2: $\mu+1=0$. Then $\mu=-1$
  - Thus, the eigenvalues of the complete graph $K_n$ are:
    - $n-1$ with multiplicity 1,
    - $-1$ with multiplicity $n-1$.
  - Determining the Eigenspaces
  - 1. For the eigenvalue $n-1$: Since $A+I=J$ and $J\mathbf{1}=n\mathbf{1}$, if we let $x=1$ (the all-ones vector), then $(A+I)\mathbf{1}=J\mathbf{1}=n\mathbf{1}$
  - $A\mathbf{1}=(n-1)\mathbf{1}$
  - 2. For the eigenvalue $-1$: If $\mu =-1$, then $Ax=-x$
  - $(A+I)x=0\implies Jx=0$
  - $\{x\in\mathbb{R}^n:x_1+x_2+\cdots+x_n=0\}$
  - Final Answer
    - Eigenvalues of $K_n$: 
    - $n-1$ (with multiplicity 1),
    - $-1$ (with multiplicity $n-1$)
    - Eigenspaces:
    - The eigenvector corresponding to $n-1$ is the all-ones vector $\mathbf{1}=(1,1,\cdots,1)^T$
    - The eigenspace corresponding to $-1$ is the $(n-1)$-dimensional subspace of $\mathbb{R}^n$ consisting of all vectors whose entries sum to zero:$$\{x\in\mathbb{R}^n:x_1+x_2+\cdots+x_n=0\}$$
    - $$
\boxed{
\begin{array}{l}
\text{Eigenvalues of } K_n: \quad n-1 \text{ (multiplicity 1)} \text{ and } -1 \text{ (multiplicity } n-1\text{)}.\\
\text{Eigenspaces: } \quad \text{Span}\{\mathbf{1}\} \text{ for } n-1, \quad \{x \in \mathbb{R}^n : \sum_{i=1}^n x_i=0\} \text{ for } -1.
\end{array}
}
$$
