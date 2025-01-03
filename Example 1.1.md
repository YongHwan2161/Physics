A point $P$ is represented in the $(x_1, x_2, x_3)$ system by $P(2, 1, 3)$. In another coordinate system, the same point is represented as $P(x_1', x_2', x_3')$ where $x_2$ has been rotated toward $x_3$ arond the $x_1$-axis by an angle of $30^0$. Find the rotation matrix and determine $P(x_1', x_2', x_3')$.
![[Pasted image 20250102130050.jpg|300x300]]

*Solution*. 
 The direction cosines $\lambda_{ij}$ can be determined from from figure sing the definition of $\lambda_{ij}$ ([[Rotation Matrix#^df03d1|참조]]).
$$\begin{align}
\lambda_{11}&=\cos{(x_1', x_1)}=\cos{(0\degree)}=1\\
\lambda_{12}&=\cos{(x_1', x_2)}=\cos{(90\degree)}=0\\
\lambda_{13}&=\cos{(x_1', x_3)}=\cos{(90\degree)}=0\\
\lambda_{21}&=\cos{(x_2', x_1)}=\cos{(90\degree)}=0\\
\lambda_{22}&=\cos{(x_2', x_2)}=\cos{(30\degree)}\approx0.866\\
\lambda_{23}&=\cos{(x_2', x_3)}=\cos{(60\degree)}=0.5\\
\lambda_{31}&=\cos{(x_3', x_1)}=\cos{(90\degree)}=0\\
\lambda_{32}&=\cos{(x_3', x_2)}=\cos{(120\degree)}=-0.5\\
\lambda_{33}&=\cos{(x_3', x_3)}=\cos{(30\degree)}\approx 0.866\\
\end{align}$$

$$\pmb{\lambda}=\left(\begin{array}{3}
1&0&0\\
0&0.866&0.5\\
0&-0.5&0.866
\end{array}\right)$$


