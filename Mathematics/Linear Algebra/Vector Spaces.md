# $\mathbb{R}^n$ and $\mathbb{C}^n$
## definition: complex numbers, $\mathbf{C}$
- A complex number is an ordered pair $(a, b)$, where $a, b\in \mathbb{R}$, but we will write this as $a+bi$
- The set of all complex numbers is denoted by $\mathbb{C}$:$$\mathbb{C}=\{a+bi:a, b\in\mathbb{R}\}$$
- Addition and multiplication on $\mathbb{C}$ are defined by$$\begin{align}
&(a+bi)+(c+di)=(a+c)+(a+d)i\\
&(a+bi)(c+di)=(ac-bd)+(ad+bc)i\\
&\text{here $a, b, c, d\in \mathbb{R}$}\end{align}$$

- $i^2=-1$
## properties of complex arithmetic
### commutativity
- $\alpha+\beta=\beta+\alpha$ and 
- commutativity of complex multiplication
- $\alpha\beta=\beta\alpha$ for all $\alpha,\beta\in\mathbb{C}$
  - proof: suppose $\alpha=a+bi, \beta=c+di$, where $a,b,c,d\in\mathbb{R}$. Then the definition of multiplication of complex numbers shows that $$\begin{align}\alpha\beta&=(a+bi)(c+di)\\&=(ac-bd)+(ad+bc)i\end{align}$$ and $$\begin{align}\beta\alpha&=(c+di)(a+bi)\\&=(ca-db)(cb+da)i\end{align}$$
  - The equations above and the commutivity of multiplication and addition of real numbers show that $\alpha\beta=\beta\alpha$
  - 
### associativity
- $(\alpha+\beta)+\gamma=\alpha+(\beta+\gamma)$ and $(\alpha\beta)\gamma=\alpha(\beta\gamma)$ for all $\alpha,\beta,\gamma\in\mathbb{C}$
### identities
- $\lambda+0=\lambda$ and $\lambda1=\lambda$ for all $\lambda\in\mathbb{C}$
### additive inverse
- For every $\alpha\in\mathbb{C}$, there exists a unique $\beta\in\mathbb{C}$ such that $\alpha+\beta=0$
### multiplicative inverse
- For every $\alpha\in\mathbb{C}$ with $\alpha\neq0$, there exists a unique $\beta\in\mathbf{C}$ such that $\alpha\beta=1$
### distributive property
- $\lambda(\alpha+\beta)=\lambda\alpha+\lambda\beta$ for all $\lambda, \alpha, \beta\in\mathbb{C}$
## definition: $-\alpha$, subtraction, $1/\alpha$, division
Suppose, $\alpha, \beta\in\mathbb{C}$
- Let $-\alpha$ denote the additive inverse of $\alpha$. Thus, $-\alpha$ is the unique complex number such that$$\alpha+(-\alpha)=0$$
- Subtraction of $\mathbb{C}$ is defined by$$\beta-\alpha=\beta+(-\alpha)$$
- For $\alpha\neq0$, let $1/\alpha$ and $\frac {1}{\alpha}$ denote the numtiplicative inverse of $\alpha$. Thus, $1/\alpha$ is the unique complex number such that $$\alpha(1/\alpha)=1$$
- For $\alpha\neq0$, divition by $\alpha$ is defined by$$\beta/\alpha=\beta(1/\alpha)$$
## notion: $\mathbf{F}$
- $\mathbf{F}$ stands for either $\mathbb{R}$ or $\mathbb{C}$
- For $\alpha\in\mathbf{F}$ and $m$ a positive integer, we define $\alpha^m$ to denote the product of $\alpha$ with itself $m$ times:$$\alpha^m=\underbrace{\alpha\cdots\alpha}_{m\text{ times}}$$
- This definition implies that $$(\alpha^m)^n=\alpha^{mn}\text{ and }(\alpha\beta)^m=\alpha^m\beta^m$$ for all $\alpha,\beta\in\mathbb{F}$ and all positive integers $m,n$
- The set $\mathbb{R}^2$, which you can think of as a plane, is the set of all ordered pairs of real numbers:$\mathbb{R}^2=\{(x,y):x,y\in\mathbb{R}\}$
- $\mathbb{R}^3=\{(x,y,z):x,y,z\in\mathbb{R}\}$
## definition: list, length
- Suppose $n$ is a nonnegative integer. A *list* of *length* $n$ is an ordered collection of $n$ elements (which might be numbers, other lists, or more abstract objects).
- Two lists are equal if and only if they have the same length and the same elements in the same order.
- many mathematicians call a list of length $n$ an $n$-tuple.