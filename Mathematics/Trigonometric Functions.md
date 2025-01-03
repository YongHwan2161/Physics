# Basic Trigonometric Functions
The primary trigonometric functions include:
- Sine($\sin{\theta}$)
- Cosine$(\cos{\theta})$
- Tangent$(\tan{\theta}=\frac{\sin{\theta}}{\cos{\theta}})$
- Cotangent$(\cot{\theta}=\frac{\cos{\theta}}{\sin{\theta}})$
- Secant$(\sec{\theta}=\frac{1}{\cos{\theta}})$
- Cosecant$(\csc{\theta}=\frac{1}{\sin{\theta}})$

# 삼각함수의 덧셈
## proof using [[Euler's formula]]


$$\begin{align}
e^{i(\theta_1+\theta_2)}&=e^{i\theta_1}\cdot e^{i\theta_2}\\
                    &=(\cos{\theta_1}+i\sin{\theta_1})(\cos{\theta_2}+i\sin{\theta_2})\\
                    &=(\cos{\theta_1}\cos{\theta_2}-\sin{\theta_1}\sin{\theta_2})+i(\sin{\theta_1}\cos{\theta_2}+\cos{\theta_1}\sin{\theta_2})\\
                &=\cos{(\theta_1+\theta_2)}+i\sin{(\theta_1+\theta_2)}
\end{align}$$

$$\begin{align}
\cos{(\theta_1+\theta_2)}&=\cos{\theta_1}\cos{\theta_2}-\sin{\theta_1}\sin{\theta_2}\\
\sin{(\theta_1+\theta_2)}&=\sin{\theta_1}\cos{\theta_2}+\cos{\theta_1}\sin{\theta_2}\\
\cos{(\theta_1-\theta_2)}&=?
\end{align}$$
# Derivative of Trigonometric Functions
## [[함수의 미분#Derivative of Sine Function|Derivative of Sine Function]]$(\sin{\theta})$
$$f(\theta)=\sin{\theta}, f'(\theta)=\cos{\theta}$$


# taylor series expansions
자세한 내용은 [[Taylor Series]] 참조
$$\sin{x}=x-\frac{x^3}{3!}+\frac{x^5}{5!}-\frac{x^7}{7!}+\cdots$$

$$\cos{x}=1-\frac{x^2}{2!}+\frac{x^4}{4!}-\frac{x^6}{6!}+\cdots$$

