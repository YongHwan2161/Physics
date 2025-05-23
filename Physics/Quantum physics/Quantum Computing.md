
# Suppressing quantum errors by scaling a surface code logical qubit
[[s41586-022-05434-1.pdf|Suppressing quantum errors by scaling a surface code logical qubit]]

# Classical Shannon Theory

# Entropy and Shannon's Information Theory
- Shannon quantified this by taking the base 2 logarithm of the probability of a given message occuring. That is, if we denote the information content of a message by $I$, and the probability of its occurrence by $p$, then $$I=-\log_2{p}$$
- The negative sign ensures that the information content of a message is positive, and that the less probable a message, the higher is the information content.
- A message that is unlikely to occur has a low probability an therefore has a large information content.
- A message that is very likely to occur has a high probability and therefore has a small information content.
- 섀넌의 이항 엔트로피(binary entropy)는 정보의 척도다. 어떤 이항 무작위 변수(binary random variable)에 대한 확률 분포 $(p, 1-p)$가 주어져 있을 때, 섀넌의 binary entropy는 다음과 같다. $$h_2(p)\equiv -p\log_2 p-(1-p)\log_2(1-p)$$
- Next let's develop a more formal definition. Let $X$ be a random variable characterized by a probability distribution $\vec{p}$, and suppose that it can assume one of the values $x_1,x_2,\cdots,x_n$ with probabilities $p_1,p_2,\cdots,p_n$. Probabilities satisfy $0\le p_i\le 1$ and $\sum_i p_i=1$.
- If we are certain what the message is, the Shannon entropy is zero.
- The more uncertain we are as to what comes next, the higher the Shannon entropy.
- Now suppose that we require $l_i$ bits to represent each $x_i$ in $X$. Then the *average bit rate* required to encode $X$ is $$R_X=\sum_{i=1}^n l_i p_i$$
- The Shannon entropy is the lower bound of the average bit rate $H(X)\le R_X$
- The worst-case scenario in which we have the least information is a distribution where the probability of each item is the same--meaning a uniform distribution. Again, suppose that it has $n$ elements. The probability of finding each $x_i$ if the distribution is uniform is $1/n$. So sequence $X$ with $n$ elements occurring with uniform probabilities $1/n$ has entropy $-\sum \frac 1n \log_2{\frac 1n}=\sum \frac 1n \log_2 n=\log_2 n$. This tells us that the Shannon entropy has the bounds $$0\le H(X)\le \log_2 n$$
- The *relative entropy* of two variables $X$ and $Y$ characterized by probability distibutions $p$ and $q$ is $$H(X||Y)=\sum p\log_2 \frac pq=-H(X)-\sum p\log_2 q$$
- Suppose that we take a fixed value $y_i$ from $Y$. From this we can get a conditional probability distribution $p(X|y_i)$ which are the probabilities of $X$ given that we have $y_i$ with certainty. Then $$H(X|Y)=-\sum_j p(x_j|y_i)\log_2 (p(x_j|y_i))$$
- This is known as the *conditional entropy*. The conditional entropy satisfies $H(X|Y)\le H(X)$
- To obtain equality in $H(X|Y)=H(X)$, the variables $X$ and $Y$ must be independent. So $H(X, Y)=H(Y)+H(X|Y)$
- We are now in a position to difine *mutual information* of the variables $X$ and $Y$. In words, this is the difference between the entropy of $X$ and the entropy of $X$ given knowledge of what value $Y$ has assumed, that is, $$I(X|Y)=H(X)-H(X|Y)$$
- This can also be written as $$I(X|Y)=H(X)+H(Y)-H(X,Y)$$
# Qubits and Quantum States
