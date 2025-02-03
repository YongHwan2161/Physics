# Monic Polynomial
- A polynomial is said to be **monic** if the coefficient of its highest-degree term is 1. Let's explain this concept in detail and then see why it applies to the characteristic polynomial of a graph's adjacency matrix.
## General Definition of a Monic Polynomial
- Consider a polynomial in one variable $x$ of degree $n$:
- $P(x)=c_nx^n+c_{n-1}x^{n-1}+\cdots+c_1x+c_0$
- Monic polynomial: The polynomial $P(x)$ is monic if its leading coefficient $c_n$ is equal to 1. In that case, we can write:$$P(x)=x^n+c_{n-1}x^{n-1}+\cdots+c_1x+c_0$$
- For example, $x^3-2x+5$ is a monic polynomial because the coefficient of $x^3$ is contrast, $2x^3-2x+5$ is not monic because the coefficient of $x^3$ is 2.
- 