# 참고문헌
- MathJax tutorial : https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference

# 사용예시 
## 텍스트 입력
`\text{}`: $\text{텍스트는 이 안에 입력하면 됩니다.}$

## 기호, 상수
- 그리스 문자 $\alpha\beta\gamma\delta\psi\mu\omega, \Delta\Omega\Phi\Pi\Gamma$
- $\epsilon,  \varepsilon$
- $\phi, \varphi, \Phi$
- Plank constant: \hbar : $\hbar$
- `\check`: $\check{I}$
- `\acute`: $\acute{J}$
- `\grave`: $\grave{K}$
- `\vec{}`: $\vec{u}\vec{AB}$, `\overrightarrwo{}`: $\overrightarrow{AB}$
- `\bar{}`: $\bar{z}$
- `\overline`: $\overline{A}, \overline{AAA}$
- `\underline`: $\underline{B}, \underline{BBB}$
- `\hat{}`: $\hat{x}$
- `\tilde{}`: $\tilde{x}$
- `\dot \ddot \dddot`: $\dot{x}, \ddot{x}, \dddot{x}$ 
- `\cdot`: $F\cdot x$
- `\cdots`: $\cdots$
- `\vdots`: $\vdots$
- `\ddots`: $\ddots$
- 
- `\mathring{}`: $\mathring{A}$
- `\degree`: $100\degree$
- `overbrace`: $\overbrace{abcdefg}_{dididid}^{dkdkdk}$
- `underbracd`: $\underbrace{abcdefg}_{dkdkdkd}^{dkdkdkdkkd}$
- 
## Fonts
- blackboard bold: \mathbb{} or \Bbb{}
  - $\mathbb{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$, $\Bbb{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- script letter: `\mathscr`: $\mathscr{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- calligraphic letter: `\mathcal`: $\mathcal{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- Fraktur letters: `\mathfrak`; $\mathfrak{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- sans-serif font: `\mathsf`: $\mathsf{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- roman font: `\mathrm`: $\mathrm{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- typewriter fint: `\mathtt`: $\mathtt{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- italics: `\mathit`: $\mathit{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- boldface: `\mathbf`: $\mathbf{ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz}$
- boldface symbol: `\boldsymbol`, `\pmb`:
$$\boldsymbol{\lambda}, \pmb{\lambda}$$
- 

## 화살표
- $\rightarrow, \leftarrow, \uparrow, \downarrow$
- $\Rightarrow, \Leftarrow, \Uparrow, \Downarrow$
- \rightarrwo = \to : $\rightarrow = \to$
- \leftarrow = \gets : $\leftarrow = \gets$
- \mapsto: $\mapsto$
- \implies: $\implies$
- \iff: $\iff$

## 삼각함수
- `\sin{}, \cos{}`; $\sin{\theta}, \cos{\theta}$
- 
## 연산자
- 집합 관계: - `\cup \cap \setminus \subset \subseteq \subsetneq \supset \in \notin \emptyset \varnothing` $\cup, \cap, \setminus, \subset, \subseteq, \subsetneq, \supset, \in, \notin, \emptyset, \varnothing$
- 편미분: \partial :  $\partial$
- 극한: 
  \lim : $\lim_{x\rightarrow 0}\frac{x}{2}$
  inline mode에서도 lim 아래에 첨자가 표시되게 하려면 \lim\limits를 사용한다. $\lim\limits_{x\to 1}\frac{x^2}{x}$

$$\lim_{x\rightarrow 0}\frac{x}{2}$$
- `\frac{}{}` :  $\frac xy$
- `times`: $A\times B$
- `\pm`, `\mp`: $\pm, \mp$
- 괄호:
  - `(), [], \{\}`: $(), [], \{\}$
  - `\left( \right)`: $\left(\frac{1}{2} + 3\right)$
  - `\left\{ \right\}`: $\left\{\frac{1}{2}+3 \right\}$

- 정의: `\equiv`: $L\equiv T-U$

- 합: \sum_{}^{} 
   - inline: $\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$
   - $$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$
- integral: `\int_{}^{}{}`: $\int_{0}^{\infty}{x^2dx}$
$$\int_{0}^{\infty}{x^2dx}$$
- `\left` and`\right` apply to all the following sorts of parentheses: `(` and `)` (x)(x), `[` and `]` [x][x], `\{` and `\}` {x}{x}, `|` |x||x|, `\vert` |x||x|, `\Vert` ∥x∥‖x‖, `\langle` and `\rangle` ⟨x⟩⟨x⟩, `\lceil` and `\rceil` ⌈x⌉⌈x⌉, and `\lfloor` and `\rfloor` ⌊x⌋⌊x⌋. `\middle` can be used to add additional dividers. There are also invisible parentheses, denoted by `.`: use `\left.x^2\right\rvert_3^5 = 5^2-3^2` to get
$$\left.x^2\right\rvert_3^5 = 5^2-3^2$$
## 수식 입력
$$\begin{equation}
\cos^2{\alpha}+\cos^2{\beta}+\cos^2{\gamma}=1
\tag{1.10}\end{equation}$$
## Aligned equations
- \begin{align}, \end{align} : &=을 기준으로 정렬할 수 있다.
```math
\begin{align}
\sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\
 & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\ 
 & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\
 & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\ 
 & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right)
\end{align}
```
$$
\begin{align}
\sqrt{37} & = \sqrt{\frac{73^2-1}{12^2}} \\
 & = \sqrt{\frac{73^2}{12^2}\cdot\frac{73^2-1}{73^2}} \\ 
 & = \sqrt{\frac{73^2}{12^2}}\sqrt{\frac{73^2-1}{73^2}} \\
 & = \frac{73}{12}\sqrt{1 - \frac{1}{73^2}} \\ 
 & \approx \frac{73}{12}\left(1 - \frac{1}{2\cdot73^2}\right)
\end{align}
$$

## Array
\begin{array}{column number}
`\begin{array}{1} \end{array}`
$$
 \left.\begin{array}{1}\begin{align}
 \lambda_{11}=\cos{(x_1', x_1)}&=\cos{\theta}\\
 \lambda_{12}=\cos{(x_1', x_2)}&=\cos{\left(\frac \pi2-\theta\right)=\sin{\theta}}\\
 \lambda_{21}=\cos{(x_2', x_1)}&=\cos{\left(\frac \pi2+\theta\right)}=-\sin{\theta}\\
 \lambda_{22}=\cos{(x_2', x_2)}&=\cos{\theta}
 \end{align}\end{array}\right\}
 $$
 $$\pmb{\lambda}=
 \left(\begin{array}{3}
 \lambda_{11} & \lambda_{12} &\lambda_{13}\\
 \lambda_{21}&\lambda_{22}&\lambda_{32}\\
 \lambda_{31}&\lambda_{32}&\lambda_{33}
 \end{array}\right)$$
## Cases
  $$\delta_{ik}=\begin{cases}
   0, & \text{if }i\neq k \\
   1, & \text{if } i=k
   \end{cases}$$
# Arbitrary operators
정의되지 않은 operator를 사용하고 싶은 경우 활용 가능
\operatorname{...} : $\operatorname{ran}$

## `bbox`
 $$\bbox[border:1px solid white]{\sum_{i}\lambda_{ij}\lambda_{ik}=\delta_{jk}}$$
 