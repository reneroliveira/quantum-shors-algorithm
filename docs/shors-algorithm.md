# Shor's Algorithm

<!-- Description of the algorithm, mathematical statements and proofs. -->

## Pseudocode

Getting straight to the point, here's how Shor's algorithm works to factor a prime product $N=pq$:

1. Pick a random integer $a$ between $0$ and $N-1$;

2. Find the smallest $r$ such that $a^r\equiv 1 ~(mod ~N)$, i.e the period of the function $f(x)=a^x~(mod~N)$;

3. If r is odd, get back to step 1

4. If $a^{r/2}+1\equiv 0~(mod~N)$, get back to step 1.

5. Then $\{p,q\}=\{\gcd(a^{r/2}-1,N),\gcd(a^{r/2}+1,N)\}$.

## Why does it works?

Let $G_N$ be the set of all positive integers less than N, that are coprimes with N. One can show that this is a group under modulo-$N$ multiplication.

Step 1 chooses a random $a\in \mathbb{Z}_N$, if we are lucky to choose an $a$ which share factors with $N$, we just use Euclidean Algorithm for $\gcd(N,a)$ and finish the factoring process.

But in most cases we'll choose a number in $G_N$. Step 2 finds the order $r$ of $a$ in $G_N$.

Then, we calculate $x=a^{r/2}~(mod~N)$ (step 3 garantees $r$ is even), and open up the expression:

\begin{align*}
    (x-1)(x+1)&\equiv x^2-1~(mod ~N)\\
    &\equiv a^r-1~(mod ~N)\\
    &\equiv 0 ~(mod ~N),\\
\end{align*}

since $a^r\equiv1~(mod~N)$

Note that $x-1\not\equiv 0 ~(mod ~N)$, because $r$ is the smallest number s.t. $a^r-1\equiv0~(mod~N)$, so $a^{r/2}-1$ does not satisfy the equivalence. Step 4, also garantees $x+1\not\equiv0~(mod~N)$.

So, we have that neither $x-1$ nor $x+1$ is divisible by $N=pq$, but their product $(x-1)(x+1)$ is. Since $N$ has just two prime factors, $p$ must divide $x-1$ or $x+1$ and $q$ the other term.

## Probability of Success

Shor's Algorithm counts on the luck of finding an $a$ which goes through steps 3 and 4 without restarting the process. But we don't need to try it out many times because the probability of a random number $a\in G_N$ have order $r$ which is even and satisfy $a^{r/2}+1\not\equiv0~(mod~N)$, is at least $50\%$!

We'll prove a more general theorem:

> **Theorem 1:** Let $N=p_1^{\alpha_1}p_2^{\alpha_2}\ldots p_m^{\alpha_m}$ the prime factorization of a positive composite number $N$. If $a$ is an uniformly random number choosen from $G_N$, and $r$ its order modulo $N$, then:
> 
> $$\mathbb{P}\left(\text{r is even and }a^{r/2}+1\not\equiv0~(mod~N)\right )\geq1-\dfrac{1}{2^{m-1}}$$
> 

Then, in the case where $N=pq$ ($m=2$), the probability of success is greater or equal $1/2$.

At first, consider $N$ being an odd prime number (because even primes won't appear in encryption applications). We'll have $|G_N|=\varphi(N)=N-1$, an even number, so consider $2^d$ the large power of 2 diving $N-1$. As said before, it can be show that $G_N$ is a cyclic group under modulo $N$ multiplication. So there is a generator $g$ such that any element from $G_N$ can be written as $g^t~(mod~N)$. If $r$ is the order of $g^t$, then $g^{tr}\equiv1~(mod~N)$.

By [Lagrange's Theorem](https://en.wikipedia.org/wiki/Lagrange%27s_theorem_(group_theory)), $tr|N-1$. If $t$ is odd, then $2^d$ must divide $r$, so $r$ is even. If $t$ happens to be even, we'll have:

$$g^{(N-1)t/2}\equiv(g^{(N-1)})^{t/2}\equiv1~(mod~N),$$

so $r|(N-1)/2$ since $r$ is the order of $g^t$ and $(g^t)^{(N-1)/2}\equiv1~(mod~N)$. This implies that $2^d$ does not divide $r$, because if that was the case, we would have $2^{d+1}|N-1$, absurd by the definition of $2^d$ as largest power of two dividing $N-1$. So, the probability of a random number in $G_N$ have an order divisible by $2^d$ is 1/2 (because $t$ is equaly likely to be odd or even).

Now, consider $N$ being power of an odd prime $N=p^{\alpha}$. The same argument still holds replacing $N-1$ by $|G_N| = \varphi(N)=p^{\alpha-1}(p-1)$. So, if $2^d$ is the largest power of two dividing $\varphi(N)$, then a randomly choosen $a\in G_N$ have its order $r$ divisible by $2^d$ with probability $1/2$.

We're gonnna prove the general case where $N=p_1^{\alpha_1}p_2^{\alpha_2}\ldots p_m^{\alpha_m}$, as a consequence of the following theorem:

> **Theorem 2 (Chinese Remainder Theorem):** Suppose $n_1,n_2,\ldots,n_k$ are positive integers pairwise coprimes. Then the system of equations:
> 
> \begin{align*}
x &\equiv a_1~(mod~n_1)\\
x &\equiv a_2~(mod~n_2)\\
\vdots&~~~~~\vdots~~~~~~~~~\vdots\\
x &\equiv a_k~(mod~n_k)
\end{align*}
> 
> has a solution. Moreover, if $x_1$ and $x_2$ are solutions, $x_1\equiv x_2~(mod~N)$, where $N=n_1n_2\ldots n_k$. 

By the above theorem, choosing $a\in G_N$ randomly, is equivalent to choose for each $i=1,\ldots,m$, a random $a_i\in G_{p_i^{\alpha_i}}$, and requiring $a\equiv a_i~(mod~p_i^{\alpha_i})$ for all $i$. Let $r_i$ be the order of $a_i~(mod~p_i^{\alpha_i})$, and $2^{d_i}$ the largest power of two dividing $r_i$. The order $r$ of $a$ will be the lowest commom divisor of $r_1,\ldots,r_m$.

Shor's algorithm only fails if all powers $2^{d_i}$ agree. If $d_i=0~\forall~i=1,\ldots,m$, then $r$ will be odd. For each $i$, the probability of $r_i$ beaing odd is $1/2$, since we are in the case of power of a prime $p_i^{\alpha_i}$. Then of $r_i$'s will be odd, with probability $1/2^{m}$.

<!-- If $d_i=d$, with $d\geq1$ for all $i$, where gonna have for each $i$: -->

The other case of failure is when $r$ is even and $a^{r/2}\equiv-1~(mod~N)$. If this happens, then for all $i$, $a^{r/2}\equiv-1~(mod~p_i^{\alpha_i})$. So $r_i$ does not divides $r/2$, because if that was the case $a^{r/2}$ would be $1$ modulo $p_i^{\alpha_i}$. Since $r_i|r$, all of $d_i$'s must agree.

As seen before, $r_1$ will be divisible by a power of $2$ (the largest dividing $\varphi(p_1^{\alpha_2}$) with probability $1/2$. Then the probability of each other $r_i$'s have it's $d_i$ agreeing with $d_1$ will be at most $1/2$ (considering the worst case scenario).

Then $\mathbb{P}(r\text{ even and }a^{r/2}\equiv-1~(mod~N))\leq\dfrac1{2^m}$. And since $\mathbb{P}(r \text{ odd})=\dfrac1{2^m}$, then probability of failure is:

$$\mathbb{P}(r\text{ odd or }a^{r/2}\equiv-1~(mod~N))\leq\dfrac1{2^m}+\dfrac{1}{2^m}=\dfrac{1}{2^{m-1}}$$

Finally, the probability of sucess is 

$$\mathbb{P}(r\text{ even and }a^{r/2}\not\equiv-1~(mod~N))\geq1-\dfrac{1}{2^{m-1}} ~~\blacksquare$$
