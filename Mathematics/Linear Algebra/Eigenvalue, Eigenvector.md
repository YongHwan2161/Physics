# symmetric matrices
## Spectral Theorem
- Real sysmmetric matrices have only real eigenvalues and admits an orthogonal diagonalization.
- Claim. Let $\mathbf{M}$ be a real symmetrix $n\times n$ matrix. Then:
  1. All eigenvalues of $\mathbf{M}$ are real.
  2. Moreover, $\mathbf{M}$ can be eiagonalized by an orthogonal matrix, i.e. there exists an orthogonal matrix $P$ such that $P^T MP$ is diagonal.
- In particular, (1) implies all roots of its characteristic polynomial $\operatorname{det}(\mathbf{M}-x\mathbf{I})$ lie in $\mathbb{R}$.
- We outline a classical argument in several steps. There are multiple approaches, but the one below relies on the so-called "max-min" or "Reyleigh quotient" method to find at least one real eigenvalue and proceed by induction.
- Step 1: Real symmetric matrices are diagonalizable over $\mathbb{C}$
- From basic linear algebra, every $n\times n$ matrix $\mathbf{M}$ over $\mathbb{C}$ admits a Schur decomposition:
- $M=QTQ^{-1}$, $M=QDQ^{-1}$
- The next question is: why must they be real? Because $M$ is real symmetric, it is also self-adjoint in the same that $M^T=M$. Self-adjointness forces the eigenvalues to be real. Below we show a more elementary real-based argument via the Rayleigh quotient.
- Step 2: Rayleigh Quotient and a Real Eigenvector
- Consider the Rayleigh quotient for $M$:$$R(x)=\frac{\mathbf{x}^T M\mathbf{x}}{\mathbf{x}^T\mathbf{x}}, \text{   ($\mathbf{x}\neq \mathbf{0}$)}$$
  1. $R(\mathbf{x})$ is real for all real $\mathbf{x}$.
  2. $R(\mathbf{x})$ attains a maximum and a minimum over the unit sphere $\mathbf{x}:|\mathbf{x}|=1$ (by compactness).
- Let $\mathbf{v}$ be a vector at which $R(\mathbf{x})$ attains the maximum. One can then show (using Lagrange multipliers or direct variational arguments) that $M\mathbf{v}=\lambda_{max}, \mathbf{v}$ with $\lambda_{\operatorname{max}}=R(\mathbf{v})$. This exhibits a real eigenvalue $\lambda_{\operatorname{max}}\in \mathbb{R}$ with a real eigenvector $\mathbf{v}$.
- Step 3: Induction on Dimension
- Once you know $M$ has a real eigenvalue $\lambda_{\operatorname{max}}$, you can find a real eigenvector $\mathbf{v}$. Form the subspace $V_1-\operatorname{span}\mathbf{v}$. Then consider the orthogonal complement $V_1^{\perp}$ . One shows $M$ leaves $V_1^{\perp}$ invariant because $M$ is symmetric (so $\mathbf{v}^TM\mathbf{u}=\mathbf{u}^TM\mathbf{v}$ for any $\mathbf{u}$). Thus you can restric $M$ to $V_1^{\perp}$(an $(n-1)$-dimentional space) and apply the same reasoning. By induction on $n$, you eventually find a real orthonoraml basis of eigenvectors.
- Hence one concludes:
- 1. All eigenvalues are real (since each restriction or step yields a real eigenvalue).
- 2. We can choose an orthonormal set of eigenvectors filling $\mathbb{R}^n$. In that basis, $M$ is diagonal, and the change-of-basis matrix is orthogonal.
- 