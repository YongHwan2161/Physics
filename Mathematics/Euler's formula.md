- $e$ is the base of the natural logarithm.
- $i$ is the imaginary unit, satisfying $i^2=-1$.
- $\theta$ is a real number representing an angle in radians.
$$
e^{i\theta}=\cos{\theta}+i\sin{\theta}
$$

When $\theta=\pi$, Euler's Formula gives the famous Euler's Identity:
$$e^{i\pi}+1=0$$
# Proof Using Taylor Series
The Taylor series expansion of $e^x, \sin{x}$, and $\cos{x}$ are fundamental in this approach. $\rightarrow$ [[Taylor Series]]
1. Exponential Function:
$$e^x=1+x+\frac{x^2}{2!}+\frac{x^3}{3!}+\frac{x^4}{4!}+\cdots$$
2. Sine Function:
$$\sin{x}=x-\frac{x^3}{3!}+\frac{x^5}{5!}-\frac{x^7}{7!}+\cdots$$
3. Cosine Function:
$$\cos{x}=1-\frac{x^2}{2!}+\frac{x^4}{4!}-\frac{x^6}{6!}+\cdots$$

Let $x=i\theta$, where $\theta$ is real. 참조: [[Imagenary number]]
$$\begin{align}
e^{i\theta}&=1+i\theta+\frac{(i\theta)^2}{2!}+\frac{(i\theta)^3}{3!}+\frac{(i\theta)^4}{4!}+\cdots\\
&=1+i\theta+\frac{-\theta^2}{2!}+\frac{-i\theta^3}{3!}+\frac{\theta^4}{4!}+\cdots\\
&=\left(1-\frac{\theta^2}{2!}+\frac{\theta^4}{4!}-\cdots\right)+i\left(\theta-\frac{\theta^3}{3!}+\frac{\theta^5}{5!}-\cdots\right)\\
&=\cos{\theta}+i\sin{\theta}
\end{align}$$

## Proof Using Differential Equations
- To Do
