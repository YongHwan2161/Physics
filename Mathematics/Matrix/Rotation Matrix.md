 Consider a point $P$ with coordinates $(x_1, x_2, x_3)$ with respect to a certain cordinate system. Next consider a different coordinate system, one that can be generated from the original system by a simple rotation; let the coordinates of the point $P$ with respect to the new coordinate system be $(x_1', x_2', x_3')$. The situation is illustrated for a two-dimensional case in Figure 1-2![[Pasted image 20241231125200.jpg]]
  The new coordinate $x_1'$ is the sum of the projection of $x_1$ onto the $x_1'$-aixs(the line $\overline{Oa}$) plus the projection of $x_2$ onto the $x_1'$-axis (the line $\overline{ab}+\overline{bc}$); that is, 참조: [[Trigonometric Functions]]
$$\begin{align}
x_1&=r\cos{\phi}&, x_2&=r\sin{\phi}\\
x_1'&=r\cos{(\phi-\theta)}&, x_2'&= r\sin{(\phi-\theta)}\\
x_1'&=r\{\cos{\phi}\cos{\theta}+\sin{\phi}\sin{\theta}\}&,x_2'&=r\{\sin{\phi}\cos{\theta}-\cos{\phi}\sin{\theta}\}\\
x_1'&=x_1\cos{\theta}+x_2\sin{\theta}&, x_2'&=x_2\cos{\theta}-x_1\sin{\theta}
\end{align}$$  $$\begin{align}
x_1'&=x_1\cos{\theta}+x_2\cos{\left(\frac\pi 2-\theta\right)}\\
x_2'&=x_1\cos{\left(\frac\pi 2+\theta\right)+x_2\cos{\theta}}
\end{align}$$
 Let us introduce the following notation: we write the angle between the $x_1'$-axis and the $x_1$-axis as $(x_1', x_1)$, and in general, the angle between the $x_1'$-axis and the $x_j$-axis is denoted by $(x_i', x_j)$. Furthermore, we define a set of numbers $\lambda_{ij}$ by
 $$\lambda_{ij}\equiv \cos{(x_i', x_j)}$$
 Therefore, we have ^df03d1
 $$
 \left.\begin{array}{1}\begin{align}
 \lambda_{11}=\cos{(x_1', x_1)}&=\cos{\theta}\\
 \lambda_{12}=\cos{(x_1', x_2)}&=\cos{\left(\frac \pi2-\theta\right)=\sin{\theta}}\\
 \lambda_{21}=\cos{(x_2', x_1)}&=\cos{\left(\frac \pi2+\theta\right)}=-\sin{\theta}\\
 \lambda_{22}=\cos{(x_2', x_2)}&=\cos{\theta}
 \end{align}\end{array}\right\}
 $$
 The equations of transformation now become
 $$\begin{array}{1}
x_1'=\lambda_{11}x_1+\lambda_{12}x_2\\
x_2'=\lambda_{21}x_1+\lambda_{22}x_2
 \end{array}$$
 Thus, in general, for three dimensions we have
 $$\begin{array}{1}
x_1'=\lambda_{11}x_1+\lambda_{12}x_2+\lambda_{13}x_3\\
x_2'=\lambda_{21}x_1+\lambda_{22}x_2+\lambda_{23}x_3\\
x_3'=\lambda_{31}x_1+\lambda_{32}x_2+\lambda_{33}x_3
 \end{array}$$
 or, in summation notation, 
 $$x_i'=\sum_{j=1}^{3}\lambda_{ij}x_j, (i=1, 2, 3)$$
 The inverse transformation is 
 $$\begin{align}
 x_1&=x_1'\cos{(x_1', x_1)}+x_2'\cos{(x_2', x_1)}+x_3'\cos{(x_3', x_1)}\\
 &=\lambda_{11}x_1'+\lambda_{21}x_2'+\lambda_{31}x_3'
 \end{align}$$
 or, in general, 
 ###### Eq. 1.8 
 $$x_i=\sum_{j=1}^{3}\lambda_{ji}x'_j, (i=1, 2, 3)$$
 The quantity $\lambda_{ij}$ is called th **direction cosine** of the $x'_i$-axis relative to the $x_j$-axis. It is convenient to arrange the $\lambda_{ij}$ into a square array called a **matrix**. The boldface symbol $\pmb{\lambda}$ denotes the totality of the individual elements $\lambda_{ij}$ when arranged as follows:
 $$\pmb{\lambda}=
 \left(\begin{array}{3}
 \lambda_{11} & \lambda_{12} &\lambda_{13}\\
 \lambda_{21}&\lambda_{22}&\lambda_{32}\\
 \lambda_{31}&\lambda_{32}&\lambda_{33}
 \end{array}\right)$$
 [[Example 1.1]]
## Properties of Rotation Matrices
![[Pasted image 20250102132655.jpg]]
$$\begin{equation}
\cos^2{\alpha}+\cos^2{\beta}+\cos^2{\gamma}=1
\tag{1.10}\end{equation}$$

^4b138a

 If we have two lines with direction cosines $\cos{\alpha}, \cos{\beta}, \cos{\gamma}$ and $\cos{\alpha'}, \cos{\beta'}, \cos{\gamma'}$, then the cosine of the angle $\theta$ between these lines (see Figure 1-4b) is given (see [[Problem 1-2]]) by
 $$\begin{equation}
 \cos{\theta}=\cos{\alpha}\cos{\alpha'}+\cos{\beta}\cos{\beta'}+\cos{\gamma}\cos{\gamma'}\tag{1.11}
 \end{equation}$$

^b35193

 With a set of axes $x_1,x_2, x_3$, let us now perform an arbitrary rotation about some axis through the origin. In the new position, we label the axes $x_1', x_2', x_3'$. The coordinate rotation may be specified by giving the cosines of all the angles between the various axes, in other words, by the $\lambda_{ij}$.
  Not all of the nine quantities $\lambda_{ij}$ are independent; in face, six relations exist among the $\lambda_{ij}$, so only three are independent. We find these six relations by using the trigonometric results stated in [[Rotation Matrix#^4b138a|1.10]] and [[Rotation Matrix#^b35193|1.11]].
   First, the $x_1'$-axis may be considered alone to be a line in the $(x_1, x_2, x_3)$ coordinate system; the direction cosines of this line are $(\lambda_{11}, \lambda_{12}, \lambda_{13})$. Similarly, the direction cosines of the $x_2'$-axis in the $(x_1, x_2, x_3)$ system are given by $(\lambda_{21}, \lambda_{22}, \lambda_{23})$. Because the angle between the $x_1'$-axis and the $x_2'$-axis is $\pi/2$, we have, from [[Rotation Matrix#^b35193|equation 1.11]],
   $$\lambda_{11}\lambda_{21}+\lambda_{12}\lambda_{22}+\lambda_{13}\lambda_{23}=\cos{\theta}=\cos{(\pi/2)}=0$$
   or
   $$\sum_{j}\lambda_{1j}\lambda_{2j}=0$$
   And, in general,
$$\begin{equation*}
   \sum_{j}\lambda_{ij}\lambda_{kj}=0, (i\neq k)
\end{equation*}$$
^1-12a

   Because the sm of the squares of the direction cosines of a line equal unity([[Rotation Matrix#^4b138a]|Equation 1.10]]), we have for the $x_1'$-axis in the $(x_1, x_2, x_3)$ system, 
   $$\lambda_{11}^2+\lambda_{12}^2+\lambda_{13}^2=1$$
   or
   $$\sum_{j}\lambda_{1j}^2=\sum_{j}\lambda_{1j}\lambda_{1j}=1$$
   and, in general, 
   $$\sum_{j}\lambda_{ij}\lambda_{kj}=1, (\text{if }i=k)$$
   ^1-12b
   
   which are the remaining three relations among the $\lambda_{ij}$.
   We may combine the results given by [[Rotation Matrix#^1-12a|Equation 1-12a]], and [[Rotation Matrix#^1-12b|Equation 1-12b]] as
   $$\sum_{j}\lambda_{ij}\lambda_{kj}=\delta_{ik}$$^1-13

where $\delta_{ik}$ is the **Kronecker delta symbol** 
   $$\delta_{ik}=\begin{cases}
   0, & \text{if }i\neq k \\
   1, & \text{if } i=k
   \end{cases}$$
   The validity of [[Rotation Matrix#^1-13|Equation 1-13]] depends on the coordinate axes in each of the systems being mutually perpendicular. Such systems are said to be **orthogonal**, and [[Rotation Matrix#^1-13|Equation 1-13]] is the **orthogonality condition**. The transformation matrix $\pmb{\lambda}$ specifying the rotation of any orthogonal coordinate system must then obey [[Rotation Matrix#^1-13|Equation 1-13]].
 If we were to consider the $x_i$-axes as lines in the $x_i'$ coordinate system and perform a calculation analogous to our preceding calculations, we would find the relation
 ###### Eq 1.15
 $$\bbox[border:1px solid white]{\sum_{i}\lambda_{ij}\lambda_{ik}=\delta_{jk}}$$
 
  The two orthogonality relations we have derived ([[Rotation Matrix#^1-13|1.13]] and [[Rotation Matrix#Eq 1.15|1.15]]) appear to different. Thus, it seems that we have an overdetermined system: twelve equations in nine unknowns.
  
 ## multiplication of a matrix
 The multiplication of a matrix $\mathbf{A}$ and a matrix $\mathbf{B}$ is defined only if the number of *columns* of $\mathbf{A}$ is equal to the number of *rows* of $\mathbf{B}$. (The number of rows of $\mathbf{A}$ and the number of columns of $\mathbf{B}$ are each arbitrary.) Therfore, the product $\mathbf{AB}$ is given by
 ###### Equation 1.19
 $$\begin{align}
\mathbf{C}&=\mathbf{AB}\\
C_{ij}&=[\mathbf{AB}]_{ij}=\sum_kA_{ik}B_{kj}
 \end{align}$$
  ## not-commutative of matrix multiplication
  It should be evident from [[Rotation Matrix#Equation 1.19|Equation 1.19]] that matrix multiplication is not commutative. Thus, if $\mathbf{A}$ and $\mathbf{B}$ are both square matrices, then the sums
  $$\sum_kA_{ik}B_{kj}\text{ and }\sum_kB_{ik}A_{kj}$$
  are both defined, but, in general, they will not be equal.
# 1.6 Further Definitions
## transposed matrix
 A **transposed matrix** is a matrix derived from an original matrix by interchange of rows and columns. We denote the **transpose** of a matrix $\mathbf{A}$ by $\mathbf{A}^t$. Acoording to definition, we habe
 ###### Eq. 1.22
 $$\lambda_{ij}^t=\lambda_{ji}$$
 Evidently, 
 ###### Eq. 1.23
 $$(\lambda^t)^t=\lambda$$
 [[Rotation Matrix#Eq. 1.8|Equatio 1.8]] may therefore be written as any of the following equivalent expressions:
 ###### Eq. 1.24b
 $$x_i=\sum_j\lambda_{ij}^tx_j'$$
 