
## classical energy expression
$$E = \frac{p^2}{2m}+V(x)$$
## plane wave
- for a free particle, the quantum state (wavefunction) is typically taken as a plane wave:$$\Psi(x,t)=e^{i(kx-\omega t)}$$
- $k=\frac{p}{\hbar}$
- $E=\hbar\omega$
## Time derivative of plane wave
$$\frac{\partial}{\partial t}\Psi(x,t)=\frac{\partial}{\partial t}e^{i(kx-\omega t)}=-i\omega e^{i(kx-\omega t)}-i\omega\Psi(x,t)$$
- multiply $i\hbar$
$$i\hbar\frac{\partial\Psi(x,t)}{\partial t}=\hbar\omega\Psi(x,t)=E\Psi(x,t)$$
## Energy operator
$$\hat{E}=i\hbar \frac{\partial}{\partial t}$$
## momentum operator
$$\hat{p}=-i\hbar\frac{\partial}{\partial x}$$
- [[#Energy operator]]와 [[#momentum operator]]를 [[#classical energy expression]]에 대입
$$i\hbar\frac{\partial}{\partial t}=-\frac{\hbar^2}{2m}\frac{\partial^2}{\partial x^2} + V(x)$$
1차원(x축)에서 Schrödinger equation은 다음과 같이 정의된다. 여기서 $\Psi(x, t)$는 wave function, $\hbar$는 [[plank's constant 1#^a86a94|plank constant]]이다.
편미분에 대해서는 [[편미분(partial derivative)]] 참고.
$$i\hbar\frac{\partial\Psi(x, t)}{\partial t} = -\frac{\hbar^2}{2m}\frac{\partial^2\Psi(x, t)}{\partial x^2}+V(x)\Psi(x)\tag{1}$$

$$i\hbar\frac{\partial\Psi(x, t)}{\partial t} = \frac{p^2}{2m} + V(x)\tag{2}$$

hamiltonian($H$)는 다음과 같이 정의된다.
$$H = \frac{p^2}{2m} + V(x)\tag{3}$$

식 (2)과 (3)를 합치면 다음과 같이 표현할수 있다.
$$
i\hbar \frac{\partial\Psi(x, t)}{\partial t} = H\Psi(x, t)
\tag{4}$$
# The Statistical Interpretation
$$\int_a^b{\left.|\Psi(x, t)\right|^2dx=\text{probability of finding the particle between a and b, at time t}}$$
