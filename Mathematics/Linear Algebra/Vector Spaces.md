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
- $\alpha+\beta=\beta+\alpha$ for all $\alpha, \beta\in \mathbb{C}$
  - proof: suppose $\alpha=a+bi, \beta=c+di$, where $a,b,c,d\in\mathbb{R}$. Then $$\begin{align}\alpha+\beta&=(a+bi)+(c+di)\\&=(a+c)+(b+d)i\\&=(c+a)+(d+b)i\\&=(c+di)+(a+bi)\\&=\beta+\alpha\end{align}$$
  - 
### associativity
- $(\alpha+\beta)+\gamma=\alpha+(\beta+\gamma)$ 
  - proof: suppose $\alpha=a+bi, \beta=c+di,\gamma=e+fi$ where $\alpha,\beta,\gamma\in\mathbb{C}, \text{ and }a,b,c,d,e,f\in\mathbb{R}$
  - $$\begin{align}(\alpha+\beta)+\gamma&=((a+c)+(b+d)i)+(e+fi)\\
  &=(a+c+e)+(b+d+f)i\\
&=(a+bi)+((c+e)+(d+f)i)\\
&=\alpha+(\beta+\gamma)\end{align}$$
- $(\alpha\beta)\gamma=\alpha(\beta\gamma)$ for all $\alpha,\beta,\gamma\in\mathbb{C}$
  - proof: $$\begin{align}(\alpha\beta)\gamma&=\big((ac-bd)+(ad+bc)i\big)(e+fi)\\
  &=(ace-bde-adf-bcf)+(acf-bdf+ade+bce)i\\
&=(a+bi)\big((ce-df)+(cf+de)i\big)\\
&=\alpha(\beta\gamma)\end{align}$$
### identities
- $\lambda+0=\lambda$ and $\lambda1=\lambda$ for all $\lambda\in\mathbb{C}$
### additive inverse
- For every $\alpha\in\mathbb{C}$, there exists a unique $\beta\in\mathbb{C}$ such that $\alpha+\beta=0$
### multiplicative inverse
- For every $\alpha\in\mathbb{C}$ with $\alpha\neq0$, there exists a unique $\beta\in\mathbf{C}$ such that $\alpha\beta=1$
### distributive property
- $\lambda(\alpha+\beta)=\lambda\alpha+\lambda\beta$ for all $\lambda, \alpha, \beta\in\mathbb{C}$
  - proof: $$\begin{align}\lambda(\alpha+\beta)&=(m+ni)\big((a+c)+(b+d)i\big)\\
  &=(ma+mc-nb-nd)+(mb+md+na+nc)i\\
&=\big((ma-nb)+(mb+na)i\big)+\big((mc-nd)+(md+nc)i\big)\\
&=\lambda\alpha+\lambda\beta\end{align}$$

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
## example: lists versus sets
- The lists (3, 5) and (5, 3) are not equal, but the sets {3, 5} and {5, 3} are equal.
- The lists (4, 4) and (4, 4, 4) are not equal (they do not have the same length), although the sets {4, 4} and {4, 4, 4} both equal the set {4}.
# $\mathbf{F}^n$
## definition: $\mathbf{F}^n$, coordinate
- $\mathbf{F}^n$ is the set of all lists of length $n$ of elements of $\mathbf{F}$:$$\mathbf{F}^n=\{(x_1,\cdots,x_n):x_k\in\mathbf{F}\text{ for }k=1,\cdots,n\}$$ For $(x_1,\cdots,x_n)\in\mathbf{F}^n$ and $k\in\{1,\cdots,n\}$, we say that $x_k$ is the $k^{th}$ coordinate of $(x_1,\cdots,x_n)$
## definition: addition in $\mathbf{F}^n$
- Addition in $\mathbf{F}^n$ is defined by adding corresponding coordinates:$$(x_1,\cdots,x_n)+(y_1,\cdots,y_n)=(x_1+y_1,\cdots,x_n+y_n)$$
## commutativity of addition in $\mathbf{F}^n$
- If $x, y\in\mathbf{F}^n$, then $x+y=y+x$
- Proof: Suppose $x=(x_1,\cdots,x_n)\in\mathbf{F}^n$ and $y=(y_1,\cdots,y_n)\in\mathbf{F}^n$. Then$$\begin{align}x+y&=(x_1,\cdots,x_n)+(y_1,\cdots,y_n)\\
&=(x_1+y_1,\cdots,x_n+y_n)\\
&=(y_1+x_1, \cdots,y_n+x_n)\\
&=(y_1,\cdots,y_n)+(x_1,\cdots,x_n)\\
&=y+x\end{align}$$ where the second and fourth equalities above hold because of the definition of addition in $\mathbf{F}^n$ and the third equality holds because of the usual commutativity of addition in $\mathbf{F}$.$$\tag*{$\Box$}$$
## Definition: additive inverse in $\mathbf{F}^n, -x$
- For $x\in\mathbf{F}^n$, the additive inverse of $x$, denoted by $-x$, is the vector $-x\in\mathbf{F}^n$ such that $$x+(-x)=0$$ Thus if $x=(x_1,\cdots,x_n)$, then $-x=(-x_1,\cdots,-x_n)$.
## Definition: scalar multiplication in $\mathbf{F}^n$
- The product of a number $\lambda$ and a vector in $\mathbf{F}^n$ is computed by multiplying each coordinate of the vector by $\lambda$:$$\lambda(x_1,\cdots,x_n)=(\lambda x_1,\cdots,\lambda x_n)$$ here $\lambda\in\mathbf{F}$ and $(x_1,\cdots,x_n)\in\mathbf{F}^n$.
# Definition of Vector Space
## difinition: addition, scalar multiplication
- An *addition* on a set $V$ is a function that assigns an element $u+v\in V$ to each pair of elements $u,v\in V$.
- A *scalar multiplication* on a set $V$ is a function that assigns an element $\lambda v\in V$ to each $\lambda\in\mathbf{F}$ and each $v\in V$.
## 1.20 definition: *vector space*
- A *vector space* is a set $V$ along with an addition on $V$ and a scalar multiplication on $V$ such that the following properties hold.
  - commutativity
    - $u+v=v+u$ for all $u,v\in V$
  - associativity
    - $(u+v)+w=u+(v+w)$ and $(ab)v=a(bv)$ for all $u,v,w\in V$ and for all $a,b\in\mathbf{F}$.
  - additive identity
    - There exists an element $0\in V$ such that $v+0=v$ for all $v\in V$.
 - additive inverse
   - For every $v\in V$, there exixts $w\in V$ such that $v+w=0$.
- multiplicative identity
  - $1v=v$ for all $v\in V$.
- distributive properties
  - $a(u+v)=au+av$ and $(a+b)v=av+bv$ for all $a,b\in \mathbf{F}$ and all $u,v\in V$.
- 
  - Show that in the definition of a vector space [[Vector Spaces#1.20 definition *vector space*|1.20]], the additive inverse condition can be replaced with the condition that $$0v=0 \text{ for all }v\in V$$
- Here the 0 on the left side is the number 0, and the 0 on the right side is the additive identity of $V$.
- (The phrase a "condition can be replaced" is a difinition means that the collection of objects satisfying the definition is unchanged if the original condition is replaced with the new condition.)
  - Let $v$ be any vector in $V$. Consider the scalar -1 (which exists because $\mathbf{F}$ is a field). By the distributive property of scalar multiplication over field addition, we have:$$\begin{align}&v+(-1)v=1v+(-1)v=(1+(-1))v\\
  &(1+(-1))v=0v\\
&v+(-1)v=0v=0\end{align}$$
  - Thus, for every $v\in V$, there exists an additive inverse (namely, $(-1)v$). Consequently, the condition that every vector has an additive inverse is automatically satisfied if we assume $0v=0$ for all $v\in V$.
### example of not vector space
#### Empty set is not a vector space
- The empty set is not a vector space. The empty set fails to satisfy only one of the requirements listed in the definition of a vector space ([[Vector Spaces#1.20 definition *vector space*|definition:vector space]]). Which one?
  - The empty set fails to have an additive identity. In a vector space, one of the requirements is that there must exist an element $0\in V$ such that for every $v\in V$, the equatin $v+0=v$ holds. Since the empty set contains no elements at all, it does not contain an additive identity (or any element), and therefore it cannot be a vector space.
  - 
- Let $\infty$ and $-\infty$ denote two distinct objects, neither of which is in $\mathbb{R}$. Define an addition and scalar multiplication on $\mathbb{R}\cup\{\infty, -\infty\}$ as you could guess from the notation. Specifically, the sum and product of two real numbers is as usual, and for $t\in \mathbb{R}$ define $$t\infty=\begin{cases}-\infty & \text{if }t<0\\0&\text{if }t=0\\\infty&\text{if }t>0\end{cases} \quad\quad t(-\infty)=\begin{cases}\infty&\text{if }t<0,\\0&\text{if }t=0,\\-\infty&\text{if }t>0,\end{cases}$$
- and $$\begin{align}t+\infty&=\infty+t=\infty+\infty=\infty\\
t+(-\infty)&=(-\infty)+t=(-\infty)+(-\infty)=-\infty\\
\infty+(-\infty)&=(-\infty)+\infty=0\end{align}$$
- With these operations of addition and scalar multiplication, is $\mathbb{R}\cup\{\infty,-\infty\}$ a vector space over $\mathbb{R}$? Explain.
  - Exhibiting the failure of associativity:
  - Choose $u=\infty, v=-\infty, \text{ and } w=1\quad (\text{with} 1\in\mathbb{R})$,
  - 1. Compute $(u+v)+w=(\infty+(-\infty))+1=0+1=1$
  - 2. Compute $u+(v+w)=\infty+(-\infty + 1)=\infty+(-\infty)=0$.
  - Since, $(u+v)+w\neq u+(v+w)$.
## definition: vector, point
- Elements of a vector space are called *vectors* or *points*.
## definition: real vector space, complex vector space
- A vector space over $\mathbb{R}$ is called *real vector space*
- A vector space over $\mathbb{C}$ is called complex vector space
## notation: $\mathbb{F}^S$
- If $S$ is a set, then $\mathbb{F}^S$ denotes the set of functions from $S$ to $\mathbb{F}$.
- For $f,g\in \mathbb{F}^S$, the sn $f+g\in\mathbb{F}^S$ is the function defined by $$(f+g)(x)=f(x)+g(x)$$ for all $x\in S$.
- For $\lambda\in\mathbb{F}$ and $f\in\mathbb{F}^S$, the product $\lambda f\in \mathbb{F}^S$ is the function defined by $$(\lambda f)(x)=\lambda f(x)$$ for all $x\in S$.
## unique additive identity
- A vector space has a unique additive identity.
- proof: Suppose $0$ and $0'$ are both additive identities for some vector space $V$. Then $0'=0'+0=0+0'=0$, where the first equality holds because 0 is an additive identity, the second equlity comes from communicativity, and the third identity, the second equality comes from commutativity, and the third equality holds because 0' is an additive identity. Thus 0'=0, proving that $V$ has only one additive identity.
## unique additive inverse
- Every element in a vector space has a unique additive inverse.
- Proof: Suppose $V$ is a vector space. Let $v\in V$. Suppose $w$ and $w'$ are additive inverses of $v$. Then $$w=w+0=w+(v+w')=(w+v)+w'=0+w'=w'$$
- Thus $w=w'$, as desired.
## notation: $-v, w-v$
- Let $v,w\in V$, Then
- $-v$ denotes the additive inverse of $v$;
- $w-v$ is defined to be $w+(-v)$.
## notation: $V$
- For the rest of this book, $V$ denotes a vector space over $\mathbb{F}$.
## the number $0$ times a vector
- $0v=0$ for every $v\in V$
- Proof: For $v\in V$, we have $0v=(0+0)v=0v+0v$. 
- Adding the additive inverse of $0v$ to both sides of the equation above gives $0=0v$, as desired.
## a number times the vector 0
- $a0=0$ for every $a\in\mathbb{F}$.
- Proof: For $a\in\mathbb{F}$, we have $a0=a(0+0)=a0+a0$
- Adding the additive inverse of $a0$ to both sides of the equation above gives $0=a0$ as desired.
## the number -1 times a vector
- $(-1)v=-v$ for every $v\in V$.
- Proof: For $v\in V$, we have $v+(-1)v=1v+(-1)v=(1+(-1))v=0v=0$.
- This equation says that $(-1)v$, when added to $v$, gives 0. Thus $(-1)v$ is the additive inverse of $v$, as desired.
# Exercises 1B
## 1. 
- Prove that $-(-v)=v$ for every $v\in V$.
- Let $v$ be any elemetn in the vector space $V$. By definition, the additive inverse $-v$ satisfies $v+(-v)=0$, $(-v)+(-(-v))=0$, add $v$ to both sides, 
- $-(-v)=v$.
- this completes the proof
## 2. 
- Suppose $a\in\mathbf{F}, v\in V$, and $av=0$. Prove that $a=0$ or $v=0$.
  - Case 1: If $a=0$, then the claim is immediately true. [[Vector Spaces#the number $0$ times a vector|0 times a vector]]
  - Case 2: Suppose $a\neq 0$. Since $\mathbf{F}$ is a field, the multiplicative inverse $a^{-1}$ exists. Multiply both sides of the equation $av=0$ by $a^{-1}$:$$\begin{align}&a^{-1}(av)=a^{-1}0\\
  &(a^{-1}a)v=1v=v\\
&a^{-1}\mathbf{0}=\mathbf{0}\\
&v=\mathbf{0}\end{align}$$
  - In either case, we conclude that if $av=0$, then $a=0$ or $v=0$, as reauired.
## 3. 
- Suppose $v,w\in V$. Explain why there exists a unique $x\in V$ such that $v+3x=w$.
## 7. 
- Suppose $S$ is a nonempty set. Let $V^S$ denote the set of functions from $S$ to $V$. Define a natural addition and scalar multiplication on $V^S$, and show that $V^S$ is a vector space with these definitions.
- **Solution:**