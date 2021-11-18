# Shor's Algorithm

<!-- Description of the algorithm, mathematical statements and proofs. -->


```python
import markdown
md = markdown.Markdown(extensions=['pymdownx.arithmatex'])
```

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

Shor's Algorithm counts on the luck of finding an $a$ which goes through steps 3 and 4 without restarting the process. But we don't need to try it out many times because the probability of a random number $a\in G_N$ have order $r$ which is even and satisfy $a^{r/2}+1\not\equiv~(mod~N)$, is at least $50\%$!
