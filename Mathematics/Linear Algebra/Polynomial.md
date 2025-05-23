# Monic Polynomial
- A polynomial is said to be **monic** if the coefficient of its highest-degree term is 1. Let's explain this concept in detail and then see why it applies to the characteristic polynomial of a graph's adjacency matrix.
## General Definition of a Monic Polynomial
- Consider a polynomial in one variable $x$ of degree $n$:
- $P(x)=c_nx^n+c_{n-1}x^{n-1}+\cdots+c_1x+c_0$
- Monic polynomial: The polynomial $P(x)$ is monic if its leading coefficient $c_n$ is equal to 1. In that case, we can write:$$P(x)=x^n+c_{n-1}x^{n-1}+\cdots+c_1x+c_0$$
- For example, $x^3-2x+5$ is a monic polynomial because the coefficient of $x^3$ is contrast, $2x^3-2x+5$ is not monic because the coefficient of $x^3$ is 2.
## Monic Characteristic Polynomial of a Matrix
- Let $A$ be an $n\times n$ matrix with entries in some field (for our purposes, the integers or real numbers). The characteristic polynomial of $A$ is defined by $P_A(x)=\operatorname{det}(x\mathbf{I}-A)$,
- Why is $P_A(x)$ monic?
- When you form the matrix $x\mathbf{I}-A$, the diagonal entries are $x-a_{ii}$ and the off-diagonal entries are $-a_{ij}$. When you compute the determinant, one of the terms comes from taking the product of the diagonal entries: $$(x-a_{11})(x-a_{22})\cdots(x-a_{nn})$$
- $1\cdot 1\cdots 1=1$
- $P_A(x)=x^n+(\text{lower order terms})$
## Why Being Monic Matters: The Rational Root Theorem
- The Rational Root Theorem states that if a polynomial with integer coefficients $$P(x)=x^n+c_{n-1}x^{n-1}+\cdots+c_0$$
- $p$ must divide the constant term $c_0$
- $q$ must divide the leading coefficient.
- Since the polynomial is monic, its leading coefficient is 1. Therefore, $q$ must divide 1. The only positive divisor of 1 is 1 itself, so $q=1$. Hence, any rational root $\frac pq$ must actually $p$
## Application to Eigenvalues of a Graph
- For a graph $G$, the adjacency matrix $A_G$ is a real symmetric matrix with entries 0 or 1. Its characteristic polynomial $$P_G(x)=\operatorname{det}(x\mathbf{I}-A_G)$$