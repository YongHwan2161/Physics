# Defiinition
For a function $f$ that is infinitely differentiable at a point $a$, the Taylor Series of $f(x)$ around the point $a$ is given by:
$$f(x)=f(a)+f'(a)(x-a)+\frac{f''(a)}{2!}(x-a)^2+\frac{f'''(a)}{3!}(x-a)^3+\cdots$$
In summation notation, this is expressed as:
$$f(x)=\sum_{n=1}^{\infty}\frac{f^{(n)}(a)}{n!}(x-a)^n$$
Where:
- $f^{(n)}(a)$ denotes the $n$-th derivative of $f$ evaluated at $a$.
- $n!$ is the factorial of $n$

## Maclaurin Series
A special case of the Taylor Series is the **Maclaurin Series**, where the expansion is around $a=0$:
$$f(x)=f(0)+f'(0)x+\frac{f''(0)}{2!}x^2+\frac{f'''(0)}{3!}x^3+\cdots=\sum_{n=1}^{\infty}\frac{f^{(n)}(0)}{n!}x^n$$
## Examples of Taylor Series
### Exponential Function: $e^x$
참조: [[함수의 미분]], [[Exponential Function]]
$$f(x)=e^x, f^{(k)}(x)=e^x$$
At x=0:
$$f^{(k)}(0)=e^0=1$$
Taylor series:
$$e^x=\sum_{k=0}^\infty\frac{1}{k!}x^k=1+x+\frac{x^2}{2!}+\frac{x^3}{3!}+\cdots$$
### Sine Function: $\sin{x}$
참조: [[함수의 미분#삼각함수의 미분]]
$$\begin{align}
f(x)&=\sin{x}\\
f'(x)&=\cos{x}\\
f''(x)&=-\sin{x}\\
f'''(x)&=-\cos{x}\\
&\vdots
\end{align}$$
At $x=0$:
$$\begin{align}
f(0)&=0\\
f'(0)&=1\\
f''(0)&=0\\
f'''(0)&=-1\\
&\vdots
\end{align}$$
Taylor series:
$$\sin{x}=\sum_{k=0}^\infty\frac{(-1)^k}{(2k+1)!}x^{2k+1}=x-\frac{x^3}{3!}+\frac{x^5}{5!}+\cdots$$
### Natural Logarithm: $\ln({1+x})$
## Convergence of Taylor series
